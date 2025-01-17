.. _creating-new-textures-from-scratch:

Creating New Textures from Scratch
==================================

The PNMImage Class
------------------


This class is how Panda3D handles regular images (.gif, .jpg, and the like).
This class allows you to manipulate individual pixels of the image. You can
load existing images using the function
``read(fileName)`` where
``filename`` is the path to the
image file (in :ref:`Panda Filename Syntax <loading-models>`) wrapped in a
``Filename`` object. Or, you can
create a brand new image from scratch, by passing the x, y size to the
constructor. 

.. only:: python

    
    
    .. code-block:: python
    
        myImage = PNMImage()
        myImage.read(Filename("testImg.png"))
        myEmptyImage = PNMImage(256, 256)
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        PNMImage myImage;
        myImage.read(Filename("testImg.png"),PNMFileTypeRegistry::get_global_ptr()->get_type_from_extension("png"),true);
    
    


You can get the size of the image you have read using the
``getXSize()`` and
``getYSize()`` functions. Although you
cannot change the size of an image directly, you can rescale an image by
filtering it into a larger or smaller PNMImage:



.. code-block:: python

    fullSize = PNMImage(Filename("testImg.png"))
    reduced = PNMImage(256, 256)
    reduced.gaussianFilterFrom(1.0, fullSize)



You can get individual RGB values using the
``getRed(x,y), getGreen(x,y), getBlue(x,y)`` or
``getRedVal(x,y), getGreenVal(x,y), getBlueVal(x,y)`` where x and y tell what
pixel to look at (lower-left is 0,0 upper right is
``getXSize()-1, getYSize()-1`` The difference between
these functions is that the
``get*Val`` functions return a
number between 0 and 1 while the
``get*`` functions return their
value as an integer. For example, if your image uses 8-bit color calling
``getGreenVal`` on a green pixel will
return 255 and calling ``getGreen``
will return 1. You can also get all the RGB information at the same time using
``getXel(x,y)`` and
``getXelVal(x,y)`` which return a 3
component vector with red in the x, green in the y, and blue in the z.



.. code-block:: python

    # The pixel at 0,0 is red and we're using 8-bit color
    myImage.getRedVal(0,0) # Returns 255
    myImage.getRed(0,0) # Returns 1
    
    colors = myImage.getXelVal(0,0) # Returns (255,0,0)
    colorVal = myImage.getXel(0,0) # Returns (1,0,0)



The functions for setting pixel information are
``setRed(x,y,value), setGreen(x,y, value), setBlue(x,y, value)`` or
``setRedVal(x,y,value), setGreenVal(x,y, value), setBlueVal(x,y, value)``. Theres still the same
dichotomy as above when it comes to regular sets and using setvals. You can
also use ``setXel(x,y,colorVec)`` and
``setXelVal(x,y, colorVec)``. You can also fill an
image with a color by using
``fill(r,g,b)`` and
``fillVal(r,g,b)``.



.. code-block:: python

    myImage.setGreenVal(0, 0, 255) # If pixel (0,0) was red before, now it is yellow (255,255,0)
    myImage.setBlue(0, 0, 1) # Pixel (0,0) is now white
    
    gray = Vec3(0.5, 0.5, 0.5)
    
    # Both of these set the origin to gray
    myImage.setXelVal(0, 0, gray * 255)
    myImage.setXel(0, 0, gray)
    
    # Makes every pixel red
    myImage.fillVal(255, 0, 0)
    # Makes every pixel green
    myImage.fill(0, 1, 0)



There are also gets and sets for the alpha channel using the same interface as
above. However, if you use them on an image that doesn't have an alpha channel
you will cause a crash. To see if an image has an alpha channel use
``hasAlpha()`` which returns True if
there is an alpha channel and False otherwise. You can add an alpha channel
using ``addAlpha()``. You can also
remove it using ``removeAlpha()``.

You can also make an image grayscale by using
``makeGrayscale()``. You can now use sets
and gets for Gray too. Using
``getGray*`` on a color image just
returns the value in the blue channel. If you want to get the grayscale value
of a pixel regardless of whether the image is a grayscale or a color image,
you can use ``getBright(x,y)``, which
works equally well on color or on grayscale images. If you want to weight the
colors use ``getBright(x,y, r,g,b)`` where r,g,b
are the weights for their respective channels.

There are several other useful functions in the class this the API Reference
for more information.

Getting the Image of a Texture
------------------------------


The Panda ``Texture`` class does
not allow for pixel manipulation. However the
``PNMImage`` class below does.
Therefore, if you want to change the image in a
``Texture`` object you must call
its ``store(myImage)`` which saves the
image of the texture into
``myImage``.



.. code-block:: python

    myImage = PNMImage()
    myTexture = loader.loadTexture("myTex.jpg")
    
    # After this call, myImage now holds the same image as the texture
    myTexture.store(myImage)



Loading a PNMImage Into a Texture
---------------------------------


Once you have changed all the data in the image you can now load it into a
texture using the ``Texture``
objects ``load(myImage)`` function, where
``myImage`` is the PNMImage to make
the texture from. 

.. code-block:: python

    # Assume we already have myImage which is our modified PNMImage
    myTexture=Texture()
    
    # This texture now represents myImage
    myTexture.load(myImage)



Remember however, that most graphics cards require that the dimensions of
texture have to be a power of two.
``PNMImage`` does not have this
restriction and Panda will not automatically scale the image when you put it
into a texture.
