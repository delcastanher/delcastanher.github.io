---
title: "Tiff Compression in Java"
summary: Choosing the Tiff compression on writing between options available in Java.
date: 2022-07-01T12:13:38-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

When [writing a Tiff file using the standard ImageIO API in Java](/posts/convert-pdf-to-tiff-in-java-with-apache-pdfbox/#convert-a-multi-page-pdf-into-a-multi-page-tiff), you must initialize a `TIFFImageWriter` variable.

```Java
ImageWriter writer = ImageIO.getImageWritersByFormatName("TIFF").next();
```

The compression type is set at the `TIFFImageWriteParam` object,  returned from `TIFFImageWriter`.

```Java
ImageWriteParam param = writer.getDefaultWriteParam();
```

You can list the available compression types as an array of Strings with the following method. [^1]

[^1]: https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/javax/imageio/ImageWriteParam.html#getCompressionTypes()

```Java
param.getCompressionTypes();
```

The available compression types for Tiff are:
-   CCITT RLE
-   CCITT T.4
-   CCITT T.6
-   LZW
-   JPEG
-   ZLib
-   PackBits
-   Deflate
-   Exif JPEG

If you need [CCITT Group 3](http://fileformats.archiveteam.org/wiki/CCITT_Group_3), you can choose **CCITT T.4**. For [CCITT Group 4](http://fileformats.archiveteam.org/wiki/CCITT_Group_4), use **CCITT T.6**. Please pay attention to the other names a compression type is also known.

To set CCITT Group 3 compression type:

```Java
param.setCompressionMode(ImageWriteParam.MODE_EXPLICIT);
param.setCompressionType("CCITT T.4");
```

Now, pass the `TIFFImageWriter` created as the third parameter when using [`javax.imageio.ImageWriter.write()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/javax/imageio/ImageWriter.html#write(javax.imageio.metadata.IIOMetadata,javax.imageio.IIOImage,javax.imageio.ImageWriteParam)) for single page Tiff, or as the second parameter when using [`javax.imageio.ImageWriter.writeToSequence()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/javax/imageio/ImageWriter.html#writeToSequence(javax.imageio.IIOImage,javax.imageio.ImageWriteParam)) while creating multi-pages Tiff.