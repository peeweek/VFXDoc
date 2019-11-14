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

Geometric Light shafts are pieces of geo

## GlowSpheres / FogSpheres

## Volumetric Sprites

## Raymarching





