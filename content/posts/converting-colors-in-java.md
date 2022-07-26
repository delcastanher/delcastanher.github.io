---
title: "Converting colors in Java"
summary: How to convert RGB, CYMK, Grayscale, and binary Black and White.
date: 2022-07-24T22:30:55-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

A digital image is a pixel matrix, with each pixel storing 32 bits of information. 8 bits for storing a number between 0 and 255 for Alpha (transparency information), 8 bits for Red, 8 for Green, and 8 for Blue. All with a value between 0 and 255. [^1]

[^1]: https://dyclassroom.com/image-processing-project/how-to-get-and-set-pixel-value-in-java

To convert to **grayscale**, for each pixel, you take these Red, Green and Blue values, calculate the average, and use this average result as the value, exactly the same value, for even Red, Green and Blue. [^2]

[^2]: https://dyclassroom.com/image-processing-project/how-to-convert-a-color-image-into-grayscale-image-in-java

Converting to pure **black and white** is similar, but you use the average result to decide when to store 0 (black) if the value is greater than 127 (that is the result for `255/2`) or to store 255 (white) if less than 127. [^3] The result can be suboptimal, particularly if the page background is of uneven darkness. [^4]

[^3]: https://stackoverflow.com/a/17482430

[^4]: https://tesseract-ocr.github.io/tessdoc/ImproveQuality.html#binarisation

The normalized color coordinate transformations used for the default CMYK color space are defined as follows: [^5]

[^5]: https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/javax/imageio/metadata/doc-files/tiff_metadata.html#ColorSpacesRead

**Linear RGB to CMYK**
```Mathematica
K = min{1 - R, 1 - G, 1 - B}
if(K != 1) {
    C = (1 - R - K)/(1 - K)
    M = (1 - G - K)/(1 - K)
    Y = (1 - B - K)/(1 - K)
} else {
    C = M = Y = 0
}
```

**CMYK to linear RGB**
```Mathematica
R = (1 - K)*(1 - C)
G = (1 - K)*(1 - M)
B = (1 - K)*(1 - Y)
```

**In Java**, you can do all of this using `ImageIO` [^6] or a library such as [Marvin Image Processing Framework](http://marvinproject.sourceforge.net/en/index.html).

[^6]: https://memorynotfound.com/convert-image-black-white-java/