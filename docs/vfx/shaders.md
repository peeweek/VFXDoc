# Shaders and Materials

This page presents an overview of the Shaders and Materials. It is not aimed to be exhaustive as there is a [detailed overview about shaders](../shaders/overview.md). 

In this document, you will find basic information about how to use shaders in general, and what can be achieved with them, in an artist's perspective.

## Programmable rendering

In layman's terms, shaders are programmable rendering functions. They enable users to customize the rendering by providing functions to blend, animate, mask and compute all sorts of things. All modern engines and hardware use shaders in a form or another, with more complexity or more user-friendliness. 

### Text vs. Graph

Authoring shaders can take many forms in terms of tools, depending on the engine you are working with, but the two main ways to author them are : text-based (the old-fashioned way) and using a node-based shader editor (most useful for artists).

These two methods have their pros and cons but the learning curve of text-based shader editing is way more daunting than a graphical approach, but instead gives a better understanding and more possibilities than a node-based graph.

The main advantage of using a node-based shader editor lies in the preview and the experimental context it enables for the user. Most of the node-based shader tools that know success enable per-node preview and this is precisely where the power come from. Prototyping made easy. A single tree can be reused and modified, and all the intermediate steps update accordingly. This is a must to learn how shading works, because it's visual, and visual is the way artists understand more how stuff works.

However, in production node-based tools have proven poor maintenance, especially if the shaders are the fruit of N iterations of trial and error. Refactoring and understanding a tentacular graph can become nearly impossible when in production.



## What you can do

Shaders enable a lot of possibilities by using different paradigms that can be reused, mixed and combined together to create new paradigms. This section covers generic aspects of what can be done. For more detailed explanation, see the [Shader Math](../shaders/math.md) section and the [Shader Utility](../shaders/utility.md) section for more code or graph snippets.

### Pixel effects

#### Advanced Material Detail and Composition

RME master shader

Shaders are mainly used in a game engine artist pipeline to establish a model for all rendering by making generic templates for artists. Unity provides a standard, generic-purpose shader while unreal lets the user make its own template for the project, using a material graph. Nowadays, Unity tends to follow this same paradigm, to ensure production needs are fullfilled.

These shaders are often complex and make uses of switches to toggle on or off some features, in order to save performance.

#### Animated Scrolling and Flow

LIS ocean / shore shader

#### Advanced Masking and material animation

LIS rain mask

#### Effect blending and composition

Fire / 

###Vertex Effects

#### Wind, Noise and procedural animation

Flex and shiver

#### Transforms and Texture Baking

Houdini tools, pivot baker, animation textures



### Other

#### Tesselation and Geometry Scattering

Compute Shaders / Geometry Shaders

#### Procedural Mask Generation

Unity Custom Texture / Unreal Blueprint + Render to texture

#### Complex Systems / Other

keijiro stuff

