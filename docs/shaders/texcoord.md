# Manipulating Texture Coordinates (UVs)

This page presents a variety of practices related to displaying textures in various flavors using shaders. While this list is not exhaustive, you will find here many features that you can even combine together, for instance you can use a **Panner** on a **Deformation Map** to distort another texture, then use **Gradient Mapping** to output its color. See Final Section (Example) for a more complex production example.

## Sampling textures using coordinates

Firstly, let's look at reading a texture to display it in a Shader. This method is called **Sampling** : It requires a texture to read, and **Texture Coordinates**, often called UVs to read them and display each texel at the correct position either on mesh (using mesh UVs) or on screen.

![](./img/uv-read.png)

These **texture coordinates** can be altered before reading the texture: for instance, in the example below, we apply a multiplier to the UVs to apply some tiling.

![](./img/uv-tiling.png)



During the **Sampling** of the Texture, a **Sampler State** is used to control how the texture read will behave. By default, most Game Engines will store these settings for each texture, but you can also override them in your shader editors. 

For more details, see [Texture Sampling](../textures/sampling.md).

![](img/uv-sampler-override.png)

## Scrolling UVs (Panner)

A pretty handy use case of manipulating the UVs before reading the texture is that you can dynamically alter them. One simple use of this manipulation is texture scrolling : after reading the UVs, we add an offset based on the current game time to apply a scrolling effect.

![](./img/uv-scroll.gif)

> **Why do you use subtract instead of Add?**
>
> It would be legitimate to think that adding an offset would move the texture in that direction, however, if you take as 1D example, the range [0.0 .. 1.0] with 1/s scroll speed, would become at 0.1s : [0.1 .. 1.1].  So you would read the texture, a bit more to the right. However here, what we want is not to move our eye to the right, but instead move the texture to the right. So we need to go the opposite direction.
>
> **Why do you use the fraction node?**
>
> There are two reasons we use a `frac(time*scrollDirection)` :
>
> 1) First: So you can preview the scrolling in the modified UVs, before it's used in the Sample Texture Node. No doing so would result in large values and in our case, the subtract node preview would appear full black (negative values). The fraction instruction has limited impact on the shader performance and can help solve issues in the second case.
> 2) Some node-based shader editors (such as unreal) can detect that a part of the computation network can be computed at CPU. If such a thing is possible, this means that the `time*scroll` value sent to the shader will always be low, thus preventing UV precision errors when time becomes big. If your shadergraph too does not handle that, produced will produce precision errors with large time values.



## Rotating UVs (Rotator)

Rotating UVs can be useful to apply simple movement to non-moving geometry, for instance making in-place markers in your game so the player see a target position in a game level.

![](img/rotator-purpose.gif)

The [math behind such a transformation](https://gist.github.com/ayamflow/c06bc0c8a64f985dd431bd0ac5b557cd) of texture coordinates are however not as trivial, as they involve sine/cosine transformations. Most of the time, game engines provide an implementation for 2D rotations of coordinates. However, if you find this missing here's the HLSL code (retranscribed from the snipped from ayamflow) :

```c
float2 rotateUV(float2 uv, float rotation, float2 center)
{
    float2 delta = uv - center;
    
    return float2(
      cos(rotation) * delta.x + sin(rotation) * delta.y + center.x,
      cos(rotation) * delta.y - sin(rotation) * delta.x + center.y
    );
}
```



Implementing this function in a shadergraph produces quite a messy, yet working graph. Anyway, we can still package this transformation graph into a subgraph to simplify its use.

![](img/rotator-shadergraph.gif)

 

## Rectangular to Radial UV

Sometimes, we want to deform our texture sampling to make effects such as vortices or radial effects. However, using a simple rotation of the UVs it is not enough if we want to have scrolling that goes pushes outwards, such as the example below.

![](img/uv-radial-scroll.gif)

In order to achieve that, we are going to convert the rectangular UV space to a **Radial UV Space**.

<u>The transformation of UVs happens in a radial coordinate space:</u>

* **Circular Coordinate (U)** : a coordinates that revolves around a central point. It is computed using an arc-tangent2 function (which is equivalent to `atan(y/x)`) It produces the angle around the point where the current point coordinate is. It is expressed in radians in the range [-π ... π] in order to use it as a more user-friendly coordinate, we remap the range to [0..1]
* **Radial Coordinate (V)** : a coordinates that represents the distance to a central point.

![](img/uv-rectangular-to-radial.png)

#### Animated Radial Scrolling / Rotation

By converting traditional UV space into Radial Space, textures can be panned and rotated into that space to create both rotation, and radial scrolling:

- **Rotation** will happen when applying panning to the the **transformed U coordinate**
- Radial scrolling will happen when applying panning to the **transformed V coordinate** 

![](img/uv-radial-scroll-various.gif)

#### Spirals and Twirls

By extension, the coordinates can be modified to apply a rotation angle that increases based on the distance to the center. 

* U Coordinate stays the same
* We add the Distance (U coordinate) to the Angle (V Coordinate). Intensity is controlled by multiplying the distance by a scale : Please note that in order to conserve texture tiling, we need to apply integer scales only.

![](img/uv-radial-spiral.png)



## UV Deformation

A common, more sophisticated effect involves adding the result of a deforming texture to the texture coordinates before reading the final texture. It is called Texture Coordinate Deformation (or UV Deformation).

![GIF of Texture Coordinate Deformation result](img/uv-deform-example.gif)

In order to implement such deformation, we need to understand a bit more what we can do with our UVs: In the previous example, we added a function of time and a direction to subtract to the pixels. So it was applied in a evenly to all pixels.

Now, let's ask ourselves: What If we could apply a different direction, for each pixel? We would not have a continuous scrolling in one direction, but could have a scrolling that flows in **many** directions. 

But we are not yet there. Let's put the infinite flow scrolling on hold, pause the time and look at this :

![Screenshot of normal map deforming UVs](img/uv-distortion-map.png)

In this example, My UVs have been subtracted by not a single, uniform value, but instead by the result of a distortion map. The values are then applied to the UVs, and these distorted UVs will sample the texture with more or less strength.

![GIF showing the strength of the deformation](img/uv-deform-intensity.gif) 

Finally, a common use case for deformation, is to scroll the deformation map, and apply it to the UVs. It will make the deformation animated. You can also scroll the UVs for both the Deformation Map and the Color map at different speeds to create some parallax effect.

![GIF Showing the panner + deformed UVs](img/uv-deform-panner.gif)

### Authoring Deformation Maps

In the examples above, the Deformation Map was authored using specific generation software to be already in the [-1.. 1] range. Such software like [Mixture for Unity](https://github.com/alelievr/Mixture/) , can help you author these maps for your projects. However, you can also author these textures from simple Normal Maps. 

Normal maps are usually encoded in the [0..1] range as **unsigned-normalized** (which means that the neutral value is the 127 gray). However, if you need to use these maps as deformers, please make sure you read them correctly, using a texture node that samples the normal map correctly, or remap the value from the [0...1] range to the [-1...1] range by yourself.

![Deform using Normal Maps](img/uv-deform-normalmap.png)

Regardless of the input texture type, the deformation maps need to be imported as **linear (non-sRGB)** in all cases.

## Flow Mapping

Following our previous example: Now that we know how to perform UV deformation, let's go back at our initial question: *Can I scroll infinitely every pixel in any direction?*

The simple answer is : *Yes! .... but not that easy!*

For our use case, here is a deformation map that defines 8 flows going outwards, and a twirl at its center.

![](img\flowmap-handpainted.png)

If we look back to our previous example, when we applied the deformation intensity to the deformation map, the intensity acted as our initial "time", and the pixels of the texture acted as our initial "direction". Now, let's use our hand painted map.

![Using Fake time as intensity](img/flowmap-as-time.gif)

In this example, we used a slider to simulate time, the deformation will start at zero, then grow infinitely.... and after a quick value (around 0.4), it becomes so stretched that the flow becomes hard to read (and we can even see inverse flow deformation). But, the interesting part of the animation was the beginning of the movement, when it was *not so deformed*, around 0.0 to 0.4.

Now, instead of running our time value between 0 and infinity, let's cycle it between -0.5 and 0.5. To do so, we are gonna change the range of the slider from [0 .. 2], to [-0.5 .. 0.5]

![ Showing time cycled between -0.5 and 0.5](img/flowmap-as-time-02.gif)

This time, the deformation is way better, we get inverse deformation when time is at -0.5, no deformation at 0 then forward deformation at 0.5. We have good continuity of flow, but now the problem is that it **does not cycle**.

#### Making the flow loop

The flow mapping workflow involves a process where two sequences of deformation ranges [-0.5 .. 0.5] are blended and alternate along time, **so the gap in the cycle of one sequence becomes hidden when we show the other sequence**. 

To do so, we offset the time of the second sequence by *half a period* : when the first sequence is at -0.5 (or 0.5), the other is at 0. Then, we blend between the two sequences to hide the gap (green line). 

![Figure showing the two sequences](img/flowmap-sequence-loop.png)

When applied to our graph, it looks like this:

![Screenshot of the graph](img/flowmap-graph-full.png)

**Let's break this down a little:**

In that big graph, we first compute the time for each sequence, cycled in the [-0.5...0.5] range (`fraction` loops into the [0..1] range, then subtract remaps loops to [-0.5 to 0.5]. In order to offset the sequence B, we just apply a -0.5 value to the time **before** computing the cycle.

![](img/flowmap-graph-time.png)

Using this cycling time, we can now sample deformed color (just like we did in the beginning), but this time using the looped time instead of the slider. 

Here's what it looks only for sequence A:

![](img/flowmap-graph-sample.png)

When used to sample Sequences A and B, we can see that we have both deformations alternating, each with its gap offset in time. At that point, we just need to do the blend between these two flow sequences.

![](img/flowmap-graph-alternate.gif)

To do so, we can blend between the two sequences using a lerp, to hide the gap in sequence A by showing the Sequence B in the meantime, and vice versa. In order to alternate between the two sequences we use an **alternating blend factor**.

![](img/flowmap-graph-blend.gif)

Finally, by applying this blend factor, we obtain this infinite scrolling effect : In order to compute the blend factor, we used a **triangle wave function** to interpolate between the two sequences, it has a period of two, so it alternates every second from A to B, then B to A. 

<u>To enlighten a bit more how it works :</u> the example below shows each sequence with a different tint for color maps.

![](img/flowmap-graph-blend-twocolor.gif)

For convenience in the master graph, we used an utility subgraph node for the triangle wave. Here are the contents of that function. (Please note that there is also a triangle wave node built-in with Shadergraph, but it outputs in the -1..1  range)

![Triangle Wave function](img/triangle-wave.gif)



> **Question: Can I tweak the speed of blending between the two sequences? The intensity of the flow?** 
>
> Absolutely! For simplicity convenience, this example was made in the -0.5...0.5] range. But the flow mapping can be tweaked in two ways:
>
> 1) By dividing input time value by the period you want to use to blend between the two sequences.
> 2) By applying an intensity multiplier to the values you read from the deformation map.

> **Question : How to simplify this graph? ** 
>
> As flow mapping can be a bit complex to handle. It is often recommended to either do a HLSL custom node function or a sub-graph node to simplify the workflow.

#### The shader Code

If you want to implement flow mapping in a code fashion here's a HLSL code snippet that can help you.

```c
float4 SampleFlowMap(
    	float2 uv,
    	sampler2d colorMap, 
    	sampler2d flowMap, 
    	float cycleDuration, 
    	float flowIntensity, c
    	float time)
{
    time /= max(cycleDuration,0.01f);               // ensure no divide by zero
    float2 t = float2(time, time-1) % 2.0 - 1.0;    // two sequence times
    float2 d = tex2d(flowMap, uv).rg * 0.5 - 0.5;   // Assume flowmap unsigned-normalized
    float2 uv1 = uv + (d * t.x * flowIntensity);
    float2 uv2 = uv + (d * t.y * flowIntensity);
    float4 c1 = tex2d(colorMap, uv1);
    float4 c2 = tex2d(colorMap, uv2);
    return lerp(c1,c2, 1.0-abs(t.x));
}
```

#### Authoring Flowmap Textures

Authoring flowmaps can be done using a wide variety of tools today. However, these tools are either old, unmaintained, or lacking functionality. 

![](img/flowmap-painter.gif)

* [Flowmap Painter](http://teckartist.com/?page_id=107) : Historically the standalone free tool widely used by early adopter artists. Quite simple and rudimentary, with only few options but a really good preview. Not updated since 2012.
* [**Substance Painter** Plugin](https://substance3d.adobe.com/documentation/spdoc/flow-map-painting-143327274.html) : Integrated to substance workflow, can benefit from other masks.
* [Flowmap Painter for **Blender**](https://www.blendernation.com/2021/03/03/free-flow-map-painter-addon/) : Does the minimum (flow painting), can do some preview but not out-of-the-box.
* [**Krita** flow brush](https://docs.krita.org/en/reference_manual/brushes/brush_settings/options.html) : Rudimentary options, but built-in to the free painting tool. A pain to set up as there are no out-of-the-box brush preset. Lacks preview.

## Gradient Mapping

Color mapping is the use of a grayscale texture as coordinates to read inside a gradient texture. Basically, this method maps an input black to white gradient to any gradient any more complex gradient.



The easiest example is to start using UVs separate channels as input for the gradient map. The U and V (horizontal and vertical) gradients will map their values to the gradient.

![](img/gradientmap-uvs.png)

Similarly, we can use a Sphere Mask, that creates an inside-out radial gradient, to map the values of the gradient map.

![](img/gradientmap-spheremask.png)

But where the mapping is mostly used, is after reading a Grayscale Texture. In that case, a noise, that is multiplied by our sphere mask, then used as input for our gradient map.

![](img/gradientmap-texture.png)



The interesting part is when we apply a scale multiplier to the input gradient, **before mapping it to the gradient**: the resulting color of the gradient mapping creates an extinction effect that traverses the gradient. 

In our case, the fire pixels that were white at high scale values, will extinguish and fade, not using a White > Gray > Black transition, but instead will use the gradient values to Fade through White > Yellow > Orange > Dark Red > Black, thus feeling more similar to how fire and flame fades.



![gradientmap-scale](img/gradientmap-scale.gif)



#### Authoring assets

Authoring Gradient maps is quite straightforward, as Photoshop and Krita already implement filters to apply Gradient Mapping to a grayscale gradient.

* **Photoshop** : Layer > Adjustment Layers > Gradient Map
* **Krita** :  Filter > Map > Gradient Map

![](./img/gradient-map.gif)

#### Importing Gradient Maps and Gradient-map compatible grayscale textures

The general setup for importiung color-mapped textures is as following:

* **Temperature (Grayscale Maps intended for gradient mapping)** : must be imported as grayscale and linear (non-sRGB)
* **Gradient Maps** should generally be imported as **sRGB** with no texture wrapping (**Clamp** outside 0..1 UV range).
  * *Exception for HDR* : If you are able to bake HDR values into EXR gradients, these textures should be not imported as sRGB (because HDR must be stored as linear values). 
  * *Exception for Cycling Gradients*: If you want to cycle your Gradient Map (for example using an input gradient that is not limited to the [0..1] range), you can use a different mode than clamp for texture Wrapping in the texture import options.


#### The shader Code

The base of shader code is pretty straightforward but can be enriched depending on the needs of your shader.

```c
// we sample the grayscale temperature map and we scale it by the temperature scale
// for instance, the particle's alpha.
// if you need to apply soft-particle depth fading, this is a good place to multiply
// the temperature by the soft particl fading factor.
float temperature = tex2D(_TemperatureMap, input.uv).r * temperatureScale;

// then we use the temperature as UVs to sample the gradient map
// for this, we prefer a tex2DLOD as we do not need mip-mapping derivative 
// computation
float4 finalColor = tex2DLOD(_GradientMap, float4(temperature,0,0,0)).rgba;
```

#### Pros and Cons

| Pros                                                         | Cons                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| - Unified Color mapping enables consistent colors for fire all across the game.<br />-  Fire Color fades more accurately compared to an already mapped flame.<br />- Enables the use of a single temperature map through multiple lookup tables. | - Use of a Lookup table Induces a texture dependency and may degrade performance in some cases.<br /><br />-Requires a custom shader to enable the feature.<br />-Range is limited to the extents of the texture. HDR needs a separate scale channel (divider) or the use of FP16/FP32 textures. |

## Complex Example #1 - Fire Particle

The following example summarizes how the practices described in this page relate to each other. It is quite similar to what a particle shader could look like (except that it is not bound to particle color, and do not expose shader properties).

![](img/uv-complex-example.png)

<u>It combines the following:</u>

1) A tiled, panning deformation noise.
2) A panning grayscale noise, whose UVs are deformed by the deformation noise.
3) A sphere mask, that is combined by the grayscale noise, and scaled in the range [0..10] so it can fade, or go really intense.
4) Gradient Mapping of that resulting Masked, and Scaled Noise, to produce the final result.

![](img/uv-complex-result.gif)
