# High Level Shading Language

This language is close to the C language, and uses a reduced instruction set dedicated to GPU computing. Depending on the engine you use, shaders file contents can vary but you will find here the common denonimator for writing hlsl.

### Text vs. Graph

Authoring shaders can take many forms in terms of tools, depending on the engine you are working with, but the two main ways to author them are : text-based (the old-fashioned way) and using a node-based shader editor (most useful for artists).

These two methods have their pros and cons but the learning curve of text-based shader editing is way more daunting than a graphical approach, but instead gives a better understanding and more possibilities than a node-based graph.

The main advantage of using a node-based shader editor lies in the preview and the experimental context it enables for the user. Most of the node-based shader tools that know success enable per-node preview and this is precisely where the power come from. Prototyping made easy. A single tree can be reused and modified, and all the intermediate steps update accordingly. This is a must to learn how shading works, because it's visual, and visual is the way artists understand more how stuff works.

However, in production node-based tools have proven poor maintenance, especially if the shaders are the fruit of N iterations of trial and error. Refactoring and understanding a tentacular graph can become nearly impossible when in production.

## How is structured HLSL?

Let's start with a base template that displays a textured mesh with a tiling texture which color is multiplied by vertex colors:

```C
// declaration of functions
#pragma ps pixelShader
#pragma vs vertexShader

// data structure : before vertex shader (mesh info)
struct vertexInfo
{
    float3 position : POSITION;
    float2 uv: TEXCOORD0;
    float3 color : COLOR;
}

// data structure : vertex shader to pixel shader
// also called interpolants because values interpolates through the triangle
// from one vertex to another
struct v2p
{
    float4 position : SV_POSITION;
    float3 uv : TEXCOORD0;
    float3 color : TEXCOORD1;
}

// uniforms : external parameters
sampler2D MyTexture;
float2 UVTile;
matrix4x4 worldViewProjection;

// vertex shader function
v2p vertexShader(vertexInfo input)
{
    v2p output;
    output.position = mul(worldViewProjection, float4(input.position,1.0));
    output.uv = input.uv * UVTile;
    output.color = input.color;
    return output;
}

// pixel shader function
float4 pixelShader(v2p input) : SV_TARGET
{
	float4 color = tex2D(MyTexture, input.uv);
	return color * input.color;
}
```

## Base Declarations

Base declarations are in our example, arbitrary. It is not always the case but in our example we used a `#pragma` statement to tell the compiler that :

* the pixel shader (ps) function will be called `pixelShader`
* the vertex shader (vs) function will be called `vertexShader`

`ps` and `vs` are arbitrary in the example and can be different from one engine to another. For example Unity uses `#pragma vertex vertexShader` with the use of `vertex` keyword to define the vertex shader, and `fragment` for a pixel shader and `surface` for its specific, high-level surface definition.

So if you take a look at the end of the example you will see

`v2p vertexShader(vertexInfo input)` and`float4 pixelShader(v2p input) : SV_TARGET`  that are declarations of the vertex and pixel shader.

## Structures

Structures are declared most of the time using a `struct` statement with structure element in enclosed braces. In the example we define two structs `vertexInfo` and `v2p` (vertex-to-pixel) that will contain the data to pass from one shader function to another.

> If you take a look at the `vertexShader` function declaration : `v2p vertexShader(vertexInfo input)` it tells that the function has a `vertexInfo` structure named `input` that we can use in the body, and needs to output a `v2p` structure (using a  `return` statement);

Structures are declaring elements of a given type that are passed through a specific data channel, defined by a **semantic** : a keyword that describes the channel where the data is passed.

```c
struct vertexInfo
{
    float3 position : POSITION;
    float2 uv: TEXCOORD0;
    float3 color : COLOR;
}
```

For the `vertexInfo` structure we have 3 elements: 

* position which is a vector of 3 float values (X,Y,Z) passed to the POSITION semantic which represents the vertex position.
* uv, vector of 2 components (U,V) passed to the TEXCOORD0 semantic, corresponding to the first UV channel.
* color, vector of 3 components (R,G,B) passed to the COLOR semantic, corresponding to the vertex color channel.

#### Semantics for a Vertex-To-Pixel data structure

For the case of an vertex declaration, these semantics correspond to the data contained into the mesh. But when it comes to declare a structure of data that will be used between the vertex shader and pixel shader, the semantics are not the same. In this case, we need to use the `SV_POSITION` semantic for the screen-space position (projected position of the vertex on screen), then any TEXCOORD(N) semantic for other values such as UVs or color.

Passing data through these semantics will tell the rasterizer that, for every pixel of the triangle, the value need to be interpolated (for example vertex color interpolated from every vertex of a triangle) 

TEXCOORD(N) is limited though so you cannot use an infinity of values passed from vertex to pixel. Every channel can contain from 1 to 4 channels (float, float2, float3 or float4).

It is utterly interesting to use these interpolants to benefit from automatic vertex interpolation (for UVs, normals, colors, etc.)

> Performance Note: you can also use the `nointerpolation` keyword before the type to specify that you do not want the values to be interpolated through the triangle. Use this case when you pass constant data computed in vertex shader that do not need interpolation, to save some bandwidth

## Uniforms

Uniforms are the external parameters of the shader and are called uniforms because they do not vary from one vertex to another, or from one pixel to another, they are `uniform`. 

In a material system, some of these values will be exposed so the user can set them.

In our example we have 3 uniforms for our shader:

```
sampler2D MyTexture;
float2 UVTile;
matrix4x4 worldViewProjection;
```

These are declared at the base level of the code, not in a function block (in this case they would be variables local to the scope of the enclosing braces they are in).

* MyTexture is a sampler2D that can be used to read its texture using a `tex2d` function
* UVTile is a float2 parameter that will be used to set the tiling ratio for the texture.
* worldViewProjection is a matrix of 4 by 4 elements that the engine shall use to project the model on screen (basically the transformation matrix that will place the object in the world, and look at it using a camera so it gets projected on screen).

## Vertex Shader

The vertex shader is the function that will take all the vertices of the mesh and will transform them to screen-space projected positions, so the rasterizer can define which pixels on screen have to be drawn for this model.

In this function we do the following:

* we transform the model vertices from local space to screen-space using the worldViewProjection matrix.
* we transform the UV by scaling them by the UVTile scale factor, so we can tile the texture when we will read it.
* we pass the vertex color to the pixel shader without modification

## Pixel Shader

Pixel shader function is executed after all triangles are rasterized on screen, and for each pixel will compute the output color for it.

This function will output a float4 value (color + alpha) and uses the semantic SV_TARGET that corresponds to a single render target (often, the backbuffer) where the pixels will be drawn

In this function we do the following:

* we store locally a float4 named color containing the result of a texture read (`tex2d`) using the `input.uv` we just scaled in the vertex shader, and using the uniform `MyTexture`
* we return a color that corresponds to the value of the color previously read, multiplied by the vertex color