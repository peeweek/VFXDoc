# Alpha Erosion

Alpha erosion is a process of eroding opaque (or at least non-transparent) pixels based on different criteria.  The outcome of this is a control over the alpha channel that differs from a simple fade as all the pixels do not fade equally but instead are eroded progressively until all pixels are transparent.

## Principles

Alpha erosion is based on determining an **erosion threshold** in an **alpha mask** and animate this threshold based on various criteria so the shape dissolves itself into nothing.

#### Equation and Algorithm

The base equation for erosion is :

```C
if(pixel.a < threshold)
{
    pixel.a = 0.0;
}
else
{
    pixel.a = 1.0;
}
```

#### Step() function

instead of using this long comparison, we can use the `step(threshold, alpha);` [HLSL function](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-step) that will return 0 for any value below the `threshold`, and 1 for any value greater or equal to the `threshold`

so, this algorithm can be simplified down to :

```c
pixel.a = step(threshold, pixel.a); // pixel.a = (pixel.a >= thredhold)? 1 : 0;
```

Step function is a hard-stepping function that will output fully opaque or fully transparent pixels. No feather will be present so the erosion will be subject to **Aliasing**.

#### Smoothstep() function

In addition to the `step(threshold, alpha)` function exists a `smoothstep(min, max, alpha)` [HLSL Function](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-smoothstep), that will erode not only based on a hard threshold but instead using a feathered threshold that will interpolate alpha between a min Threshold and a max Threshold.



## Erosion Methods and Processes

### Hard Erosion

### Soft Erosion

### Erosion Maps

### Progression Maps

