hide_toc: True

# Lexicon

This lexicon is aimed to provide quick information about terms we encounter in visual effects. To get more information, some of the definitions include links to wikipedia or dedicated websites. 

### Aliasing

A rendering artifact that happens when a pixel tries to approximate a value that is not constant in it sub-pixel space. These artifacts can be removed by using various techniques such as Mip-Mapping, Anti-Aliasing, Alpha To Coverage, ...

### Alpha

The translucency value of a pixel, or a texel. Often stored inside a dedicated and optional channel.


### Alpha to Coverage

A feature of the Multisampling (MSAA) used often in foliage and vegetation rendering. The technique uses the alpha value of the Pixel Shader as a coverage mask in order to apply multisampling to the extents of the mask.

### ALU

Arithmetic and Logic Unit, a part of the GPU chipsets that handles the arithmetic (Math) operations. It is often paired with TU (Texture Units), that are in charge of reading textures.

### Anti-Aliasing

A rendering technique used to remove Aliasing on an image. Many techniques (both software and hardware) exist. Hardware Techniques include MSAA (MultiSampling Anti Aliasing), FSAA (FullScreen AntiAliasing). Software techniques include FXAA, SMAA, and also TFSAA (Temporal FullScreen AntiAliasing).

### Back Buffer

The main Render Target Buffer where the main rendering occurs. Often, this buffer contains color data. At the end of the frame rendering, this buffer is dumped to the Front Buffer, which is sent to the video output. If not using double buffering, the rendering happens directly in front buffer.

### Backface Culling

A culling method used in the Shader configuration to remove faces facing away from the camera. It is not based on the normal of the face but instead on the winding of the three vertices when projected on screen. This method has three settings ClockWise, CounterClockWise, and None. The latter will ignore the test so all the polygons will render as two-sided.

### Banding

An artifact happening on low luminosity gradients where precision does not allow intermediate values. It is often removed or attenuated by using a slight dithering pattern.

### BCX Block Compression)

A hardware texture compression technique used mainly on DirectX and OpenGL GPUs, and based on compressing blocks of 4x4 pixels using a palette. This texture compression involves multiples scenarios that can be used in variants of the compression.

### Beam (Geometry)

A sequence of connected quads that goes from one point to another. Compared to a trail, a beam is defined by its start and its end.

### Billboard Geometry

Two triangles that form a quad, used for rendering. Often, this geometry orientation is based on a facing direction, and an up axis.

### Blend Mode 

A rendering property, mainly defined in a shader that describes the blend operations the ROP have to apply to the currently drawn pixel.

### Bloom 

A Post-Processing technique that applies a glow effect on the brightest parts of the image. If is often based on a luminance threshold, and uses one or many passes of Blur to extend.

### Blur

A shading method involving sampling a texture multiple times around the target pixel, using a pattern and weights, and resulting of a median of all the samples. Many algorithms exist.

###Card

See [Sprite](#Sprite)

### Clear

A rendering operation that clears a render target out of its data. It is often made at the start of a frame, or when reusing Render Targets.

### Culling 

A method of removing unnecessary triangle of pixels before drawing them. Many methods exist at different levels of optimization : Frustum Culling, Occlusion Culling, Distance Culling, Depth Buffer Culling, Back-face Culling,

### Deferred Rendering 

A rendering pipeline that involves the use of a G-Buffer to render object in different layers (diffuse, specular, normal,...), and applies lighting to these buffers instead of individual objects.

### Depth of Field (DOF)

A post-processing technique that relies on Depth Buffer and Blur Passes on the Backbuffer to simulate a camera Depth of Field Effect.

### Depth Buffer 

A buffer that tags along the Framebuffer (or G-Buffer) in most rendering pipelines. This buffer stores the depth of every pixel written on screen (except if shader is configured not to write depth). This buffer helps for Depth Buffer Culling,

### Depth Buffer Culling 

A culling method used mainly during pixel rasterization, compares the depth of the pixel currently drawn with the value of the depth buffer to decide whether the pixel has to be rendered or not.

### Distance Culling 

Distance culling can be used in various ways, for particle system entities, it is a simple distance to camera test to stop rendering or even updating entire systems or single particles. It can also be a distance test similar to depth buffer culling for pixels, but instead uses the far plane depth distance to cull pixels during rasterization.

### Distortion

Also referred as Refraction or Screen Space Refraction. A screen-space method of pixel displacement that simulates refraction. Some implementations exist, ones using an accumulation buffer to distort in one pass the Framebuffer, or direct passes that reads a resolved version of the framebuffer.

### Divergence 

In GPU computing, divergence is the amount of different values a variable can have within a group of pixels. Divergence on modern GPU hardware has performance drawbacks in cases of conditional code.

### Drag 

In fluid dynamics, a force opposite to the relative motion of an object inside a surrounding fluid (air, water). This is a synonym of air resistance, or fluid resistance.

### Draw Call

When the CPU accesses the GPU and requests the drawing of geometry, the invocation of a shader, or a compute shader. Since draw calls involve communication between CPU and GPU, they induce latency and tend to be bottlenecks if they become too many.

### Dithering 

A screen-space pixel pattern used to approximate a higher precision color or opacity using perceptual diffusion. It has many use cases in shading, including banding reduction, Temporal Opacity, or Alpha to Coverage masks.

### DXT (DirectX Texture) 

See [Block Compression]

### Flipbook 
A kind of texture sheet that contains a sequence of images packed in rows and/or columns and meant to be played sequentially.

### FillRate 

A graphics performance measure unit. The amount of pixlels a GPU is able  to draw, for a given operation and for a period of time.

While GPU vendors give fillrate information for really simple pixel writes, this value is dependent on the shader or operation complexity and the precision of the buffer where the pixels are written. The value is often measured in pixels per second.

### Fog 

The opacity of the air layer, resulting in a gradually colored veil, based on pixel depth. Many implementations exist, from simple hardware vertex fog to atmospheric scattering fog postprocesses.

### Forward Rendering 

A rendering pipeline that renders objects directly with their lights applied (base forward rendering), or that applies lights per-object (additive forward rendering).

This is a pipeline that scales poorly with an increasing number of dynamic lights, since they are computed for each object that recieves the light. Meanwhile, this method does not require a G-Buffer and saves Video Memory.

The main advantage to use this kind of rendering pipeline is to be able to easily filter lights (based on lighting channels for example) on drawn objects, and to be able to easily apply a different lighting model per object.

### Force 

A physics interaction applied to an object (rigid body, particle).

### Frame (Rendering)

In rendering, the frame is the whole set of GPU operations done to achieve the rendering of one image. It involves the clearing of buffers, rendering of all geometry, computing of depth and lighting, and also post-processing.

### Frustum 

The enclosing region of view of a camera, a decal or a spotlight.

For a camera :This can be represented by a pyramid which tip (the eye position) has been truncated to form the near plane, and which the base represents the far plane.

### Frustum Culling 

A rendering method involving an intersection test between a frustum and bounding primitives (often bounding boxes of objects) to test if these objects need to be rendered.

For the case of a camera it is the operation that ignores all off-screen objects into rendering.

###G-Buffer

A set of separate render targets where all material attributes (albedo, normal, specular, ...) are rendered separately for opaque objects. This is mainly used in deferred rendering to enable a single lighting pass on all the scene. In artist's terms this can be similar real-time compositing and lighting in nuke.

(show GBuffer image)

###GPU

Graphics Processing Unit. Often known as Graphics Adapter or Graphics device. A piece of hardware that is in charge or computing and rendering elements of display. It is generally composed of a Chipset made of many simple computing units (cores), a cache memory and a dedicated memory (VRAM)

### Gradient Mapping

The operation of using a gradient LookUp Texture to remap grayscale values. It is often used for fire rendering.

### Half-Resolution Translucency 

A rendering optimization method that renders some low-frequency translucent objects into a buffer of half the width and half the height of the framebuffer, then composites this half resolution buffer into the framebuffer. This results in a very efficient pass that tends to multiply the fillrate by 4, at the expense of depth aliasing.

###Linear color space

The color space used for computations in linear rendering. Linear rendering happens in a color space where all math computations are correct (contrary to the Gamma Space, which is correct visually but not mathematically). In linear space Color from textures is converted into linear space, render is done, and at the end of rendering, linear color is converted back into Gamma Space to be displayed on screen.

### LookUp Texture

A texture which data are used to lookup values

### Mip-Mapping 

A hardware property of texture sampling, involving the read in lower resolution variants of a texture, in order to save texture caching. The decision of the best mipmap is most of the time automatically decided by the GPU based on texel density on screen, but also can be overridden.

### Momentum 

The physics intrinsic movement property of a body. It is often named velocity as it describes the amount of displacement per unit of time.

### Motion Blur

A Post-process that applies directional blur based on the movement of one pixel on screen, to simulate longer aperture times of a camera.

### Motion Tiling (or Temporal Tiling)

A phenomenon that appears on looping sequence of images (such as flipbooks) that includes a peculiar moving element. These artifacts are similar to Spatial Tiling Artifacts as they stand out and often ruin the effect.

### Multisampling (MSAA)

 An aliasing reduction method that involves a larger framebuffer and a mask to elect pixels to a multisampling median pass. By default, triangle edge pixels are candidates for this mask, but other pixels can be elected manually, such as masks used for Alpha to Coverage masks.

### Near/Far Plane

Properties of camera rendering: the range defined by the minimum and the maximum distance where pixels can be projected on screen.

### Normal

The orientation of a [vertex](#vertex) or a geometric primitive. It is represented by a [normalized](#normalization) vector pointing outwards. It can also be encoded in [Normal Map](#normal-map) textures for higher detail.

### Normalization

The operation of resizing a 3D vector so its length become one. It is often used for geometric transformations and can be computed for any vector, at the exception of the zero-size vector.

###Normal Map

A special texture containing per-texel Normal information. It is used to provide more detail than just the geometry normals of the 3D Object.

### Order-Independant Translucency (OIT)

A class of rendering techniques that ensures correct alpha display sorting without having to perform sorting before rendering. 


### Overdraw 

When drawing objects on screen, the overlap of rasterized areas of pixels lead sometimes to drawing multiple times the size of a screen while keeping packed in a small space. 

This phenomenon and lead to dramatic performance degradation and can be reduced using depth culling, stencil operations, or half-resolution rendering.

### Particle

A simulated item part of a larger physics or pseudo-physics simulation, often rendered as geometry or billboards.

### Physically-Based Rendering 

A method of rendering that relies on physical properties of the matter (albedo, normal, roughness, metallic), light (attenuation and participating media) and camera acquisition (tone mapping). It is based on 3 domains, physically based shading, physically based lights, and physically based camera.

### Post-Process 

A full-screen rendering pass that occurs often after a pass of rendering objects, based on a full screen shader execution reading the framebuffer and outputting a modified framebuffer.

### Pre-Multiplied Alpha Translucency

A blending mode that allows both alpha blending and additive blending results in the same render pass, based on an alpha mask. This blend mode applies a black cache based on the output alpha, then adds the RGB color on top of it.

### Progression Mask

A kind of texture containing a grayscale mask corresponding to the start to end animation every value corresponding to a time from start (black) to end (white) of the animation.

![show progressionmap]()

### Rasterization

Rasterization is the process where the GPU will try to determine which pixels need to be drawn on screen. During this stage, pixels are created from the triangle screen-space positions (calculated in vertex shader). Then culling can apply:

* For pixels that are out of screen
* For pixels that fail a front-face or a back-face culling test.
* For pixels that fail a depth test (if Z Tested)
* For pixels that fail a stencil test (if stencil culling is applied)

The resulting list of pixels is then executed by the **pixel (fragment) shader**

### Refraction

A material process where a translucent object with different density that its medium will display a distorted version of the background. In real-time rendering, refraction is often approximated from screen-space color, meaning that only visible pixels can be part of the refracted image.

### Render Target

A Graphics device object that can be drawn into by a Shader. Render targets can also be read, just like textures and use as source in other shaders. Common render targets in game engines are : the back-buffer, the G-Buffer.

### RGBMap (RGBAMap)

The process of encoding multiple data (masks, textures) within separate color/alpha channels of a single texture. This process enables saving texture fetches but can lead to compression artifacts (when using BC compression for instance)

### Ribbon

A ribbon is a kind of particle with multiple quad that follows motion across multiple frames. Ribbons are also often called trails. The definition can be unclear from one engine to another as some use one particle per ribbon/trail (with position history), other connect all particles from one emitter to build a ribbon/trail

### Rigid Body

A kind of dynamic, real-time body that holds properties for a physics simulation. Rigid bodies can be dynamically computed, static colliders or kinematic (animated) colliders. Most of the time, rigid bodies require a collider, and dynamic physical properties such as Weight, Friction and Bounciness.

### ROP (Raster Operations Pipeline)

The GPU unit responsible in both the rasterization process and the result of pixel shader of the depth testing, pixel blending and stencil testing.

### Shader

A GPU program that perform massive parallel computation on vertices, triangles, pixels (or even any data, for compute shaders).

### Soft Body

A dynamic body similar to a rigid body but which dynamic forces and collisions alter its shape and its behavior. Soft bodies add many more parameters for shape deformation in comparison to Rigid Bodies 

### Sorting

The process or ordering elements (particles) depending on a sorting factor. Examples are : depth sorting, age sorting.

### Sprite

An aligned planar geometry that is used to render a particle.  Most of the time this is a camera-facing quad, but other alignments (velocity, axis locked,...) or shapes (triangle, octagons,..) can exist.

### sRGB

A Texture color space used for actual color (and not mathematical information such as normals). Configuring a texture as sRGB will tell the engine that it has to perform an inverse-gamma correction before computing it in linear color space.

### Stencil

Stencil is a buffer where per-pixel masks can be stored. It is often used to reduce the amount of pixels to be computed in an intensive full-screen pass.

### Sub-UV

A portion of a [Texture Sheet](#texture-sheet). For the case of [flipbooks](#flipbook), it is a cell determined by the position of a row and column.

###Tangent Space

A computational space that represents every surface of the object with its tangent, normals and bitangent as a basis. It is often mixed with Texture Coordinate space.

###Temporal AntiAliasing

An Aliasing reduction method that relies on a smoothing over time of the image. To gather detail, the image is jittered every frame to try to catch detail inside a pixel history. Camera motion deforms the history by using re-projection methods. This method, although provides excellent results for reducing specular aliasing, requires adjusting all the rendering pipeline to match its needs (Motion Vectors, Jittered Matrices) and provides generally poor results and disgraceful artifacts for visual effects and complex animated materials.

### Texel

A pixel of the texture space. (**Tex**ture Pi**xel**)

### Texture Cache

The GPU memory area where textures from disk are loaded into to be read by shaders.

### Texture Coordinate

A vertex attribute used for Texture Mapping. It serves many purposes such as Texture Mapping, Detail mapping, LightMapping or even custom data storage.

### Texture Dependency

A GPU operation that relies on reading a texture to be able to read another texture. Common cases are Gradient Mapping and Texture UV Deforming.

### Texture Filtering

A Property of a Texture Sampler: this will enable the texture to be filtered either as Bi-Linear, Tri-Linear or Not at all (Point).

### Texture Sampler

A GPU Object that is used in shaders to read a texture based on UVs. It can be considered as a Texture Reader, with properties (Wrap Mode, Filtering, ...)

### Texture Sheet

The process of packing multiple textures into one texture (atlas) that will contain everything. To select a texture from the sheet, the shader has to compute its Sub-UV to determine the correct rectangle.

### Texture Unit (TU)

A part of a GPU hardware that is in charge of reading textures. Texture units gather pixels from the GPU memory to the GPU cache, then reads values.

### Tick

The operation of computing something every frame, for a period of time.

### Throughput

In parallel processing, the throughput is all the computation that can happen while a task is in latency state. For instance, when a Shader executes, sometimes it needs to read a texture that is not available yet. A request is made to read the pixel. Meanwhile, math operations can be computed as throughput before the latent operation (caching texture and reading pixel) happens.

### Trail

Synonym of [Ribbon](#ribbon)

### Tri-Planar Projection

A method of Texture mapping that involves projecting upon 3 axis-aligned planes (XY, YZ and ZX) a texture and blend these three texture projection based on the mesh normal. This method, besides being expensive enable consistent world Texel ratio and easy mapping without requiring complex UV editing

### Update

Synonym of [Tick](#tick)

###UV

See [Texture Coordinate](#texture-coordinate)

### Video Memory (VRAM)

A hardware random-access memory dedicated to GPU hardware. Often, this memory is faster than standard RAM to be able to process large amounts of geometric and texture data.

### Velocity

The momentum attribute of a moving element (particle, rigid body), describes the amount of displacement per time unit as a vector.

### World Space

A computational space based on the world position of Local objects that have been transformed to be placed into this space. It is often used for world-space operations and effects such as Tri-planar projection, Wind vertex deformation, Rain or snow masking on objects.

### Z-Buffer

A render target (or a component of it) that stores the depth, in projection space, of the pixels currently drawn.