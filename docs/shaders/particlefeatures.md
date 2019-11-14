# Particle Shader Features

This shortform describes the most common features you can implement while authoring particle shaders.

### Depth Fading (a.k.a Soft Particles) 

Depth fading is one of the most used tricks in order to intersect big particles into opaque geometry. This trick can be achieved because particles do not write into depth buffer and thus can 'read' the value of the first opaque pixel behind them.

**To achieve that, we require two things:** 

* First, the depth of the currently drawn pixel (our particle)
* Then, the depth of the pixel 'behind' our currently drawn pixel. To be more precise it is the depth of the first opaque pixel.

The computation is then really easy: we want that our pixel opacity goes to zero as the two depths become equal, and goes to one if the spacing increase.

The formula is quite simple :

```c
float spacing = sceneDepth - pixelDepth;
float fade = saturate(spacing/fadeDistance); //we use saturate to clamp to 0..1 range
```

You can then use your fade factor as you like.

You can also extend this function by modulating the fade distance according to another factor, for instance the alpha channel of your sprite texture.

### Camera Fading

GLSL is the OpenGL/Vulkan Language for Shaders

### Color Mapping

GLSL is the OpenGL/Vulkan Language for Shaders

### Color Mapping

GLSL is the OpenGL/Vulkan Language for Shaders

### Sprite Deformation and Distortion

### Fake Directional Lighting

This one is a bit tricky, we will require to have a fake world direction and use a normal map to light our sprites. The main issue we currently have is how to determine the correct normal of the particle to use it in a NdotL operation. It it was only to be used without normal map, the particle normals are easy to fetch, especially if your particles are camera facing. 

In this case the a 

