# Fading Workflows

Fading is a concept in Visual Effects that enables artists to control the opacity of their effects (or part of them) based on various criterion. This page contains an overview of the techniques used

## Particle Fading

### Over Life Fading

Over Life particle fading enables the control of the opacity of the entire particle based on its age (often expressed as *relative age* which is the percentage ratio of its life `age/lifetime`). Particle system software often enable users to define this value either by using curves or alpha gradients. It is then the responsibility of the particle system to sample these curves/gradients every frame, for each particle and set the alpha value.

#### Special Case : Pre-Multiplied Alpha blend mode

In the case of pre-multiplied alpha blend mode. The color and the alpha of the particle are not correlated anymore. In order to entirely fade the particle you will need to :

* Fade the RGB channel to pure black to remove the additive part of the blending
* Fade the Alpha channel to zero to remove the matte part of the blending

### Depth Fading (Soft Particles) 

Depth fading is one of the most used tricks in order to intersect big particles into opaque geometry. This trick can be achieved because particles do not write into depth buffer and thus can 'read' the value of the first opaque pixel behind them.

**To achieve that, we require two things:** 

* First, the depth of the currently drawn pixel (our particle)
* Then, the depth of the pixel 'behind' our currently drawn pixel. To be more precise it is the depth of the first opaque pixel, that we can get by reading the **depth buffer**

The computation is then really easy: we want that our pixel opacity goes to zero as the two depths become equal, and goes to one if the spacing increase.

The formula is quite simple :

```c
float spacing = sceneDepth - pixelDepth;
float fade = saturate(spacing/fadeDistance); //we use saturate to clamp to 0..1 range
```

You can then use your fade factor as you like.

You can also extend this function by modulating the fade distance according to another factor, for instance the alpha channel of your sprite texture.

### Camera Fading

Camera fading is a feature that makes a particle **fade its alpha as it comes close to the camera**. It is often implemented in particle systems directly but it can easily be computed in shaders.

**To achieve fading we require**:

* Either the Particle or Pixel World Position and the Camera World Position
* Or the current Pixel Depth in view or clip space.

The formula is quite simple :

```c
// float3 particle_pos is the particle position in world space
// float3 camera_pos is the camera position in world space
// float min_distance is the user set min fade distance (invisible)
// float max_distance is the user set max fade distance (fully visible)

float dist = distance(particle_pos, camera_pos);
float fade = saturate((dist - min_distance) / (max_distance - min_distance))
if(fade == 0.0f)
    visible = false; // Clip the particle
// Note : Setting max_distance < min_distance will fade the particle as it gets away from the camera
```

This camera fading formula implements a fading distance where the particle will fade between a minimum distance and a maximum distance. It is often a good opportunity to clip the geometry of the particle when it is fully faded out, as it is not supposed to be visible anymore.

**Optimization : Clipping a fully faded Particle/Object**

Clipping a particle can be achieved in vertex shader, by setting all the vertex positions at zero, thus leading in a zero-size geometry. In order to achieve that clipping, you need to ensure that all the vertices will be affected equally by the test.  When implemeting clipping is is a good idea to perform the depth test (or camera distance test) based on the pivot point of the particle instead of every vertex.