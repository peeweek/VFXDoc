# Shader Code Snippets and Utilities

A Series of code snippets for use in your shaders.

## Math Utilities

##### Sawtooth Function (0..1)

```c
float Saw01(float x) {
	return abs(2*frac(abs(x))-1);
}
```

##### Sawtooth Function (-1..1)

```c
float Saw(float x) {
	return 2*(abs(2*frac(abs(x))-1)-0.5);
}
```

##### Square Function (0..1)

```c
float Square01(float x) {
	return ceil(frac(x)-0.5f);
}
```

##### Square Function (-1..1)

```c
float Square(float x) {
	return 2*(ceil(frac(x)-0.5f))-0.5;
}
```

##### SoftThreshold

````c
float SoftTreshold(float x, float threshold, float size)
{
  	return saturate((x-threshold) / max(size,0.00001f));
}
````

## Trigonometry

##### Rotate around Axis

```c
float3 Rotate(float3 pivot, float3 position, float3 rotationAxis, float angle)
{
	rotationAxis = normalize(rotationAxis);
	float3 cpa = pivot + rotationAxis * dot(rotationAxis, position - pivot);
	return cpa + ((position - cpa) * cos(angle) + cross(rotationAxis, (position - cpa)) * sin(angle));
}
```





## Color

##### Desaturate

````c
float Desaturate(float3 Color) 
{
	// Correct luminance factors
	float3 w = Color*float3(0.2126f,0.7152f,0.0722f);
	return w.r + w.g + w.b;
}

````



## Texture Sampling

##### TriPlanar Projection

```c
float4 TriplanarProjection(sampler2D Texture, float3 WorldPixelRatio, float3 WorldNormal, float3 WorldPosition) {

	WorldPosition /= WorldPixelRatio;
	fixed4 projX = tex2D(Texture, WorldPosition.yz) * abs(WorldNormal.x);
	fixed4 projY = tex2D(Texture, WorldPosition.xz) * abs(WorldNormal.y);
	fixed4 projZ = tex2D(Texture, WorldPosition.xy) * abs(WorldNormal.z);
	return projX + projY + projZ;

}
```



## Texture Coordinate Utilities

##### Panner (Constant Scroll Rate)

````c
float2 Panner(float2 TexCoord, float2 ConstScrollRate, float Time) {
	return(TexCoord + frac(ConstScrollRate * Time));
} 
````

##### Rotator (Constant RotationRate)

````c
float2 Rotator(float2 TexCoord, float2 Center, float Angle) {
		
		float2 AngleCoords = float2(sin(Angle%(2*PI)),cos(Angle%(2*PI)));
		TexCoord -= Center;
		return Center +
				float2(
					TexCoord.x*AngleCoords.y - TexCoord.y * AngleCoords.x,
				 	TexCoord.x*AngleCoords.x + TexCoord.y * AngleCoords.y
				 	);
}
````

##### Rectangular to Polar Conversion

```c
float2 RectangularToPolar(float2 TexCoord) {
	return float2(
			sqrt(dot(TexCoord,TexCoord)),
			0.5f+(atan2(TexCoord.y,TexCoord.x)/PI)						
	);
}
```

##### Refit to ZeroOne

````c
float2 UVRefitTo01(float2 TexCoord, float2 MinCoord, float2 MaxCoord) {
	return (TexCoord / (MaxCoord - MinCoord)) - MinCoord;
}
````

##### Refit to MinMax

```c
float2 UVRefitToMinMax(float2 TexCoord, float MinCoord, float MaxCoord) {
	return (TexCoord - MinCoord) * (MaxCoord-MinCoord);
}
```

##### Planar Parallax

```c
float2 PlanarParallax(float2 TexCoord, float3 TangentSpaceCameraVector, float ParallaxFactor) {
	return TexCoord.xy + (TangentSpaceCameraVector.xy*(-ParallaxFactor/TangentSpaceCameraVector.z));
}
```

## Flipbooks

##### Flipbook UV computation (no blending)

```c
float2 FlipBookUV(float2 TexCoord, int NumU, int NumV, float FrameRate, float Time) {

	float2 NumUV = float2((float)NumU, (float)NumV);
	float T = (Time*FrameRate)%(float)(NumU*NumV);;
	float CF = floor(T);

	return  (TexCoord + float2(CF%NumU, NumV-floor(CF/NumU)))/NumUV;
}
```

##### Flipbook UV computation (linear blending)

```c
struct FlipBookBlendData {
	float2 CurrentImageTexCoord;
	float2 NextImageTexCoord;
	float Ratio;
};

FlipBookBlendData FlipBookUVBlend(float2 TexCoord, int NumU, int NumV, float FrameRate, float Time) {

	FlipBookBlendData o;
	
	float2 NumUV = float2((float)NumU, (float)NumV);
	float T = (Time*FrameRate)%(NumUV.x * NumUV.y);
	float CF = floor(T);
	float NF = ceil(T);
	o.Ratio = frac(T);
	o.CurrentImageTexCoord = (TexCoord + float2(CF%NumU, NumV-floor(CF/NumU)))/NumUV;
	o.NextImageTexCoord  = (TexCoord + float2(NF%NumU, NumV-floor(NF/NumU)))/NumUV;
	return o;
}
```

##### Flipbook Sampling (no blending)

````c
float4 FlipBook(sampler2D FlipBookTexture, float2 TexCoord, int NumU, int NumV, float FrameRate, float Time) {

	return tex2D(FlipBookTexture, FlipBookUV(TexCoord, NumU, NumV, FrameRate,Time)).rgba;

}
````

##### Flipbook Sampling (linear blending)

```c
fixed4 FlipBookBlend(sampler2D FlipBookTexture, float2 TexCoord, int NumU, int NumV, float FrameRate, float Time) {
	
	FlipBookBlendData d = FlipBookUVBlend(TexCoord, NumU, NumV, FrameRate,Time);

	return lerp(tex2D(FlipBookTexture, d.CurrentImageTexCoord).rgba, tex2D(FlipBookTexture, d.NextImageTexCoord).rgba, d.Ratio);

}
```

