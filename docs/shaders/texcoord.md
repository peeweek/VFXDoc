# Texture Coordinates

Reading a texture in Shaders is just a matter of sending coordinates (UV) to display the texture at the correct place on our mesh, or particle.

![](D:/git/VFXDoc/docs/shaders/img/uv-read.png)

So if we want to display a texture, we will need **texture coordinates** to display them correctly. Most of the time, these coordinates comes from the mesh setup in our favorite DCC.

![](D:/git/VFXDoc/docs/shaders/img/uv-tiling.png)

## Scrolling UVs

Some thing that is pretty cool in shaders is you can modify these coordinates before reading the texture : one simple example is texture scrolling : after reading the UVs, we add an offset based on the current game time to apply a scrolling effect.

![](D:/git/VFXDoc/docs/shaders/img/uv-scroll.gif)

## Texture Coordinate Deformation

A more sophisticated effect involves adding the result of a deforming texture to the texture coordinates before reading the final texture. 

(Screenshot and Gif)

## UV to Gradient Color Mapping

Color mapping is the use of a grayscale texture as coordinates to read inside a gradient texture. Basically to a black to white gradient we correspond a more complex gradient.

![](D:/git/VFXDoc/docs/shaders/img/gradient-map.gif)

#### Authoring assets

The method involves authoring gradient maps that are used as lookup tables. The general setup for a color-mapped process is as following:

* Temperature Maps imported as grayscale and linear (non-sRGB)
* Gradient Maps imported as sRGB with no texture wrapping (clamp outside 0..1 UV range)

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
| - Unified Color mapping enables consistent colors for fire all across the game.<br />-  Fire Color fades more accurately compared to an already mapped flame.<br />- Enables the use of a single temperature map through multiple lookup tables. | - Use of a Lookup table Induces a texture dependency and degrade performance in some cases.<br /><br />-Requires a custom shader to enable the feature.<br />-Range is limited to the extents of the texture. HDR needs a separate scale channel (divider) or the use of FP16/FP32 textures. |


## 