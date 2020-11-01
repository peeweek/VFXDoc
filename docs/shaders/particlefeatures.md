# Particle Shader Features

This shortform describes the most common features you can implement while authoring particle shaders.



### Color Mapping

GLSL is the OpenGL/Vulkan Language for Shaders

### Color Mapping

GLSL is the OpenGL/Vulkan Language for Shaders

### Sprite Deformation and Distortion

### Fake Directional Lighting

This one is a bit tricky, we will require to have a fake world direction and use a normal map to light our sprites. The main issue we currently have is how to determine the correct normal of the particle to use it in a NdotL operation. It it was only to be used without normal map, the particle normals are easy to fetch, especially if your particles are camera facing. 

In this case the a 

