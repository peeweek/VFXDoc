# Meshes in Visual Effects

Geometric data is part of every artist job and visual effects are also part of it. With the job we are lead to model specific shapes for our purpose or use already made meshes (by other teams) in order to append our own data to setup effects on these. This section covers the geometry and find echoes in the Shaders section of this documentation.

## Particle Meshes

Particle meshes are a great deal when it comes to make effect as they provide real volume compared to sprites, at the expense of being more expensive in some way, and less expensive in other ways. Knowing when to use particle meshes is critical when dealing with production. 

## Static Meshes

#### Volumetric proxies

Volumetric meshes are an old-school method that enables local densification of the medium (fog) based on simple mesh primitives in scene that will act as proxies for fog. While this method is really simple, it can induce a lot of overdraw.  

##### Glowspheres

Glowspheres are volumetric proxies that help emphasize on light sources in a dense (foggy, dusty) atmosphere. They are generally defined by an additive color and  a N-dot-V attenuation (inverse Fresnel). The inverse Fresnel fades the sphere opacity at grazing angle and can be controlled either using a gradient map or an analytical function (power, smoothstep).

Additional Features can include:

* Camera Distance fading
* Depth Fading
* Texture projection and animation

##### LightShafts

Lightshaft meshes are simulating light entries in a darkened environment. These meshes are often used to emphasize cavities or large openings in buildings. Depending on the complexity needed, these mesh can go from simple, world-oriented quads, to billboardized quads, up to fake volumetric frustums or cones.

Simple quads will only render a color using a mask and will fade using a N-dot-V inverse fresnel so the quad is visible only when facing forwards.

Billboardized quads will rotate around the light direction but will probably be an issue while looking directly at the light.

Fake volumetric meshes are more advanced and require a lot more computations in order to render correctly. While this look like the same shader as a glowsphere, The shader needs to increase exponentially while looking directly at the light source.

##### Fog Curtains

Fog curtains are meshes

#### Fake Decals

#### Light / Shadow quads

#### Heat Planes

## Adding your touch to an existing mesh

#### Method by duplication

## Animated Meshes

