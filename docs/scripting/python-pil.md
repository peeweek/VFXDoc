# Python Image Library (PIL)

PIL (and its fork PILLOW) is the one of the historic python image library that is used to batch process images using simple commands.

Its full documentation can be found here : https://effbot.org/imagingbook/ or https://pillow.readthedocs.io/en/stable/

# Installing and using PIL

### Install via PIP

PIL can be installed via PIP.

`python3 -m pip install Pillow`

For all platforms see : https://pillow.readthedocs.io/en/stable/installation.html

### Using in Python Scripts

Then PIP Modules and classes can be imported in your python scripts

```python
from PIL import Image, ImageFilter  # imports the library
```

## Quick Sheet

### Opening and Saving Images

Open an image using the `Image.open` function.
* Returns an image object
Opening an image using the `save` function.
* Use the save function on a image object

```python
from PIL import Image

source = Image.open("./image.bmp")
source.save("./image-compressed.png")
```

### Getting Image Size

Use the size property of an Image object. Size is an array of ints where index 0 is image width, and index 1 is height

```python
from PIL import Image

source = Image.open("./image.bmp")
print("Image size is {}x{}".format(source.size[0], source.size[1]))
```

### Cropping / Extracting Parts of an Image

Cropping an image is achieved using the crop function that takes a tuple of 4 values that defines a **cropping box** which values are respectively:

* x minimum coordinate of the cropping box
* y minimum coordinate of the cropping box
* x maximum coordinate of the cropping box (x+box_width)
* y maximum coordinate of the cropping box (y+box_height)

crop returns a new image object, leaving the source unchanged.

```python
from PIL import Image

source = Image.open("./image.bmp")
# crops a rectangle of 32x32 located at position 100,100
box = (100,100,132,132)
cropped = source.crop(box) 
```

