# Texture Formats

When importing a texture, your game engine does not process it as-is but instead generates a GPU compatible texture that will be sent to the Renderer in order to be displayed in 2D or 3D. The data contained in your texture will be slightly the same as the final data in the game.

## Input File Formats

##### Import Master

While working on your textures we have to find a process that ensures the image data will be kept at **maximum quality** until the final import into the game engine.  The final image you will import into the engine is your **import master**.

That means that we will be mostly working with lossless-quality files and a lot of project files. 

> One idea is to think that if the imported file is Compressed JPG rather than Uncompressed TGA the game data will be smaller. 
>
> This affirmation is **wrong** as what determines the imported image size will be the engine compression (or not).

Understanding this is crucial that in production you use only lossless file formats as your texture will most likely be compressed in the final build, you do not want them to be already compressed using another algorithm that has already degraded the quality.

##### Do's and Don't's

So basically,we do **NOT** use JPG, or GIF. but prefer instead using TGA files, EXR, TIF or BMP. PNG is debatable because of Photoshop's loss of data in the color channel for transparent pixels.

These formats are widely used and has proven to be efficient enough in production. The gain is worth the price.

> Another point (under eternal debate) is using a final export intermediate instead of importing directly a **PSD** file. 
>
> This is often viewed as another hurdle in the process by producing unnecessary files which sole purpose is to be imported. There also are some arguments in favor of using photoshop file for better color management, etc.

In the end, I would say It’s up to you, but my personal opinion on this is making export intermediates helps you a lot structuring your data. For instance you wil make sure by exporting a TGA from your PSD, that you want to export your alpha channel or not, every time you save your file. It can be seen as an hassle, but in the end you end up having full control over what you are doing.

If you end up making a lot of exports, do not let the process of doing everything by hand slow you down, and write scripts to automate your exports.

<u>Here is a list of lossless image file formats that you could use in production:</u>

| **File Format**                 | **Extension** | **Description**                                              | **Recommended**                       |
| ------------------------------- | ------------- | ------------------------------------------------------------ | ------------------------------------- |
| Targa                           | TGA           | Very simple and safe format8 bit per channel, RGB or RGBA, can be run-length encoded. | Yes                                   |
| Tagged Image File Format (TIFF) | TIF, TIFF     | Complex and rich file formatVarious kinds of compression, various kinds of bit depth. Harder to master because more error-prone.Warning: can be compressed lossy (jpg). | Yes (but risky, specific cases)       |
| OpenEXR                         | EXR           | Open standard for HDR imagery:Supports Float16 & Float32 precision.Can compress lossless and lossy (use RLE/ZIP for lossless) | Yes (uncompressed)                    |
| DirectDraw Surface              | DDS           | Final format (the engine should not reprocess the data)., access to mip-map generation, access to compression.Problems happens sometimes because of differences of implementation in some engines, so it's less recommended to use this file format. | Maybe (specific cases) NOT FOR MOBILE |

## Runtime Formats & Compression

When imported, the textures can be stored into a wide variety of GPU-Compatible texture formats, that can be compressed or not. Depending on your target hardware formats can be (or not) available, and compression can differ. This section will take DXT Compression as an example but has not a goal to be exhaustive. Mobile GPU also offer alternatives to DXT but these will not be covered here.

#### Uncompressed Formats

Uncompressed texture formats are the simplest expressions of the textures that your GPU can decode. They often are left out in preference of compressed formats that are sufficient for most of the usages. However, compression induces artifacts that cannot be affordable for some cases of specific textures (data encoding, progression masks, sharp and high-frequency noises).

##### Unsigned Integer Formats

Texture formats are quite straightforward, most of the time they consists on N channels of given bit depth. For examle A8R8G8B8 is made of a ARGB sequence where every channel is made of 8 bit of color. Depending on the needs the hardware can also provide data swizzling or encoding. Some lower precision color encoding R5G6B5 provides encoding data on 16 bit, given one more bit is allowed to the green channel (due its optical pregnancy).

Most of the time, texture formats use 8 bit of encoding, as it fits the standards of non-HDR displays that are able to display 24 bit color (8x3 channels).

On some HDR cases, some formats reduce alpha bits to increase precision for the RGB values, the A2R10G10B10 was quite famous on Xbox 360 for its efficiency for making low-precision HDR.

Also, depending on the usage, the channels can be named differently. For example A8R8G8B8 is used for storing alpha in the A channel whereas X8R8G8B8 does not intent to store alpha in the X channel.

##### Signed Formats

On the AXRXGXBX formats, data is most of the time encoded in unsigned integer values 8 bit will provide 256 values, 16bit will have 65535 values. Both of them will be ranged but do not have andy sign bit.

In parallel of that, there are signed formats often used in normal mapping or vector displacement mapping that use one bit for the sign. For example the V8U8 format is used in Unreal Engine 3 to store the U (tangent) and V (binormal) components of a normal map (centered on zero instead of 128, thanks to a sign bit), allowing uncompressed normal map in 16 bit, which is a good tradeoff if compression produces too many artifacts. This format, however, implies deducing the W (normal) component of the normal in the pixel shader.

##### Depth Buffer / Stencil 

Not really used in textures, but more often on render targets, the DXSY formats are used to encode depth and stencil operations. The D24S8 is quite famous for providing 8 stencil bit masks while conserving 24 bit of depth values per pixel.

##### Floating Point

While most of the formats (even those more than 8 bit) are encoded in integer values with a constant padding, there are some formats that enables to increase precision dramatically by using floating-point encoding instead of integer values.

Floating point data is really useful because it can either describe big number with low precision and really small numbers with high precision, such as HDR data.

Standard Floating Point is expressed in 32bit per channel using the R32F format for single channel surfaces or A32FR32FG32FB32F (otherly known as RGBAFloat in some engines) this precision is one of the best, and often is considered as overkill in favor of A16FR16FG16FB16F (RGBAHalf). This compression offers good range and precision at the expense of half of the texture bandwidth, which in this case is a huge gain (64 bits).

#### DXT / BC Compression

DXT is a compression widely used in DirectX but that’s actually based on BCx (Block Compression) specifications, otherly known as S3 Texture Compression. This format offers a wide range of lossy texture compressions with good performance and compatibility on PC GPUs. These are the formats commonly supported by GPUs and compatible with either DirectX and OpenGL.

## Block Compression Explained

DXT is based on Block Compression, an algorithm that divides the image into **blocks of 16 (4x4) pixels**, and stores information for each block : A palette, and pixel information.

<u>The palette is generated following these rules:</u>

- Every block of 4x4 pixels will contain a palette of **two colors** (or grayscale). These two colors are decided by the texture compressor and are generally the farthest colors in terms of hue, saturation and lightness.
- Additionally, two more colors are interpolated from the two colors (respectively at 33% and 66%) but not stored (so they will be *deduced*)

<u>The pixel information is generated following:</u>

- For each pixel of the block, we store an **index** to the palette.  Every pixel of the block is then defined by a palette index out of these 4 colors.



##### Different DXT Modes

Most of the engines use DXT1 & DXT5 as the base of compression for respectively RGB and RGBA textures, but some other compressions can be used. Here’s a table showing some of the most commonly used DXT formats:

| **Compression** | **Colors**                 | **Description**                                              |
| --------------- | -------------------------- | ------------------------------------------------------------ |
| DXT1 (BC1)      | RGB (+1Bit Alpha)          | The base compression for images without alpha. Can add a bit for alpha at the expense of losing one interpolated color for the block (see below).Compression ratio: 1:8 |
| DXT3 (BC2)      | RGBA (Low Precision Alpha) | This compression is similar to BC1 but it also adds another compressed surface for the alpha with 4 interpolated alpha values.Compression Ratio is 1:4 (1:8 for RGB + 1:8 for alpha)SInce BC3 is better suited for alpha, BC2 is not widely used nowadays. |
| DXT5 (BC3)      | RGBA                       | This compression is slightly the same as BC2 but instead of interpolating 4 gray values per block for alpha, it interpolates 8 gray values per block for alpha.Compression Ratio is 1:4 (1:8 for RGB + 1:8 for alpha) |
| BC4             | G                          | (DirectX 10+) This compressions behaves the same as a BC3 Alpha channel compression but is intended for storing only grayscale image.This helps a lot improving quality of compressing grayscale image compared to BC1 since we interpolate 8 gray values per block instead of 4. |
| 3Dc (BC5)       | XY                         | (DirectX 10+) This compression is meant to improve drastically the compression quality of tangent space normal maps and vector maps in general..As the normals can vary a lot from one pixel to another, the format stores the X and Y two separate alpha channel (each with 8 values per block).The Z value has to be derived in the pixel shader at an extra cost. |
| BC6             | RGB                        | (DirectX 11+) This compression allows to store RGB Data as half-precision floating point. Alpha channel cannot be stored this way. |
| BC7             | RGBA                       | (DirectX 11+) This compression is similar to the BC3 compression but is more configurable (you can add more bits to the color precision and less bits to the palette) so the compression takes a longer time to improve quality. |

## 