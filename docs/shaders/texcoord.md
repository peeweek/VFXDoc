# Texture Coordinates

Reading a texture in Shaders is just a matter of sending coordinates (UV) to display the texture at the correct place on our mesh, or particle.

![](./img/uv-read.png)

So if we want to display a texture, we will need **texture coordinates** to display them correctly. Most of the time, these coordinates comes from the mesh setup in our favorite DCC.

![](./img/uv-tiling.png)

## Scrolling UVs

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



## Texture Coordinate Deformation

A common, more sophisticated effect involves adding the result of a deforming texture to the texture coordinates before reading the final texture. It is called Texture Coordinate Deformation (or UV Deformation).

![GIF of Texture Coordinate Deformation result]()

In order to implement such deformation, we need to understand a bit what we can do with our UVs. In the previous example, we added a function of time and a direction to subtract  to the pixels. So it was applied in a uniform manner to all pixels.

Now, let's ask ourselves: What If I could apply a different offset, for each pixel? We would not have a continuous scrolling in one direction, but could have a scrolling that flows in **every** direction. 

But we are not yet there. Let's put the infinite scrolling on hold, pause the time and look at this :

![Screenshot of normal map deforming UVs]()

In this example, My UVs have been subtracted by not a single, uniform value, but instead by the result of a distortion map (taken from a normal map). The values are then applied to the UVs, and these distorted UVs will sample the texture with more or less strength.

![GIF showing the strength of the deformation]() 

Finally, a common use case for deformation, is to scroll the deformation map, and apply it to the UVs. It will make the deformation animated. You can also scroll the UVs for both the Deformation Map and the Color map at different speeds to create some parallax effect.

### Deformation Continued : Flow Mapping

Now that we know how to perform UV deformation, let's go back at our question: *Can I scroll infinitely every pixel in any direction?*

The simple answer is : Yes! .... but not so easy!

If we go back to our previous example, when we apply the deformation intensity to the deformation map, the intensity acts as our initial "time", and the pixels of the texture act as our initial "direction".

![GIF Showing time plugged for deformation]()

Here, we used the time variable instead of a parameter, the deformation will start at zero, then grow infinitely.... and after a quick value, it becomes so stretched that it becomes unusable. But something that was interesting was the beginning of the movement, when it was *not so deformed*.

Now, instead of running our time value between 0 and infinity, let's cycle it between -1 ad 1. To do so, we modulo the time by 2, creating cycles between 0 and 2, then we subtract 1 to change the range to [-1..1]

![GIF Showing time cycled between -1 and 1]()

Now, the deformation is way better, we get inverse deformation when time is at -1, no deformation at 0 then forward deformation at 1. But the problem is now that it **does not cycle**.

#### Making it loop

The flow mapping workflow involves a process where two sequences of deformation ranges [-1 .. 1] are blended and alternate so the gap in the cycle becomes hidden. To do so, we offset the time by half a period so when the first sequence is at -1 (or 1), the other is at 0. Then we lerp between the two sequences to hide the gap. 

![TimelineFigure showing the two sequences]()

When applied to our graph, it doubles the sampling of the color map (but not the deformation map), which makes it a bit more complex.

![Screenshot of the graph]()

In that graph, we have at the two the two sequences, cycled in the -1...1 range, but the second one is offset of 1s just before remapping. So the texture is sampled two times for each sequence.

Also, we used a triangle wave function to interpolate between the two sequences, it has a period of two, so it alternates every second from A to B, then B to A. For convenience in the master graph, we used an utility node. Here are the contents of that function. 

![Screenshot of the Triangle Wave function]()

Finally, we get this infinite scrolling effect. And by adjusting the intensity of the flow, we can tweak how it affects the temporal tiling. In order to enlighten the alternating two sequences, the example on the right uses two different color maps.

> **Can I tweak the speed of blending between the two sequences? The intensity of the flow?** 
>
> Absolutely! For simplicity convenience, this example was made in the -1...1 range. But the flow mapping can be tweaked in two ways:
>
> 1) By dividing input time value by the period you want to use to blend between the two sequences.
> 2) By applying an intensity multiplier to the values you read from the deformation map.

> **How to simplify this graph? ** 
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





## UV to Gradient Color Mapping

Color mapping is the use of a grayscale texture as coordinates to read inside a gradient texture. Basically to a black to white gradient we correspond a more complex gradient.

![](./img/gradient-map.gif)

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