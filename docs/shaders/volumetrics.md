# Volumetric Effects

## Geometric Fog Sheets

Fog Sheets are simple curtains of transparent geometry that are mainly used to separate foreground from backgrounds and emphasize on depth on certain perspective. 

They are used most often to simulate volumetric effects at a cheap cost by relying on combined features for emissive, unlit meshes instead of computing actual dynamic, volumetric lighting.

<u>These can Help:</u>

* Enhance the readability of an image by blending the background into a less contrasted color.
* Emphasize on a certain object in the depth of a scene by making its silhouette stand out.
* Emphasize on light sources that are not directly visible to enlighten paths that are difficult to discover for the player.
* Augment the sensation of claustrophobia and short range misperception by adding animated smoke and fog patterns.
* Provide fake and performance-efficient animated lighting to suggest background animation (for example, a road at night with vehicles)

<u>Fog Sheets are mainly used with any combination of shader features such as:</u>

* Procedural Masks or Texture Masks to define the overall fading shape.

* Distance Fading to make it disappear as the player comes close.

* Depth Fading to avoid hard-edge intersection with opaque geometry

* Texture Scrolling, Deformation and/or Blending used to generate animated smoke, steam or fog patterns. 

* Animated emission added to the base emission to simulate moving lights through the fog.

* Inverse-Fresnel fading to attenuate the planar nature of the geometry as the player roams around it.

* Billboard Alignment to ensure the geometry is facing the player at all times

## Geometric Light Shafts

Geometric Light shafts are pieces of geometry that simulate light shafts. They are a pretty cheap replacement to a global volumetric fog solution and can provide more artist-friendly tweaks. Still, this is a static solution which integration is pretty manual.

<u>Light Shafts are mainly used for :</u>

* Emphasize on the lighting direction and intensity on certain darkened areas, such as interiors, forests, ...
* Animated Lightshafts can be used as radial explosion emphasis during the first frames in order to animate a bloom effect through a fog.

<u>Geometric Light Shafts can be of different shapes:</u>

* Planar Light Shafts are mainly used for backgrounds and fixed camera points of view.
* Volume (Cone, Cylinder, Pyramid, Box) can be used directly on the critical path of the player by implementing more complex behavior.

## Simple GlowSpheres / FogSpheres

Glow Spheres and FogSpheres are pseudo-volumetric geometry proxies that blend into the environment and fake the absorption of light by the fog around light sources. They are made to be simple enough to create good effect without too much expensive features, such as dynamic volumetric shadows, sync with lights.

The base concept of a glowsphere uses a [N dot V]() operation to make the sphere fade around its grazing angles so it looks like a radial gradient whatever angle youŕe looking at it. 

![Base NdotV look]()

Then, depending on the needs, the following shader features can be implemented.

- [Depth Fading]() : make the Glow sphere blend into surrounding geometry. It is similar to Soft Particle fading, but applied to the sphere. Please note that however this feature does not take shadowing into account.

- [Vertex Painted Fading]() : In the case where a glowsphere is too intense in some areas and/or overlaps a zone that is in a shadowed area, a good idea is to vertex paint it to allow modulation in certain areas.

- [Distance Fading]() : If the player's camera is able to traverse the sphere sufrace, the illusion vanishes as the volumetric effect will clip instantly. In order to ease the transition, distance fading can be used to fade as the player comes close. However, if the player is inside and the sphere cull mode is set to Back, there will be no fog inside. It can be then interesting to use cull mode to None for these, as it will also display the backface as pseudo-volumetric. However please note that with just simple Glowspheres, the interior's pixels will lack the look of fog (see Advanced Glowspheres for more info)

- [Texturing]() : in order to give more atmosphere to your glowspheres, it can be important to apply some variation to the fake density. This variation can be either applied to the density as a multiplier of alpha, or as an exponent to the N dot V gradient. The latter option `alpha = pow(abs(dot(normal,view)),lerp(minExp, maxExp, density))`  instead of fading all  alpha uniformly, will give less spherical shape. However please note that

- [Optimizing with half-resolution transparency]() : If your engine supports it, Glowspheres are pretty friendly with half resolution transparency. As they generally render low frequency features, the difference in visuals when set in low resolution transparency makes it a win-win situation to ease the framerate.

### Advanced Glowspheres

While simple glowspheres are one of the easiest use case to implement in games, they often come with their constraints, which are that they are too simple to be handled as real-volumetric objects. However, there is another, less-simple implementation available to render pseudo-volumetrics, solving the "inside" effect of these spheres.



## Volumetric Sprites

## Raymarching
