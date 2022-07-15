---
title: "Use of Java AWT RenderingHints by PDFBox"
summary: How Apache PDFBox uses RenderingHints when rendering PDF to image.
date: 2022-07-15T10:35:30-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

[PDFBox](https://pdfbox.apache.org/) uses [Java AWT RenderingHints](https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/java/awt/RenderingHints.html) before converting a PDF page to a [`BufferedImage`](https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/java/awt/image/BufferedImage.html) object. Of the eleven options `RenderingHints` offers, PDFBox uses only three, as follows, and can be checked at [PDFBox's `PDFRenderer` class](https://github.com/apache/pdfbox/blob/trunk/pdfbox/src/main/java/org/apache/pdfbox/rendering/PDFRenderer.java).

## `KEY_INTERPOLATION`
The INTERPOLATION hint controls how image pixels are filtered or resampled during an image rendering operation.

PDBox uses the **`VALUE_INTERPOLATION_NEAREST_NEIGHBOR`**, the color sample of the nearest neighboring integer coordinate sample in the image is used.

## `KEY_RENDERING`

The RENDERING hint is a general hint that provides a high level recommendation as to whether to bias algorithm choices more for speed or quality when evaluating tradeoffs.

PDFBox uses the **`VALUE_RENDER_QUALITY`**, rendering algorithms are chosen with a preference for output quality.

## `KEY_ANTIALIASING`

The ANTIALIASING hint controls whether or not the geometry rendering methods of a Graphics2D object will attempt to reduce aliasing artifacts along the edges of shapes. A typical antialiasing algorithm works by blending the existing colors of the pixels along the boundary of a shape with the requested fill paint according to the estimated partial pixel coverage of the shape.

PDFBox uses the **`VALUE_ANTIALIAS_OFF`**, rendering is done without antialiasing.

### For knowledge, the other `RenderingHints` keys available are:
-   `KEY_DITHERING`
-   `KEY_TEXT_ANTIALIASING`
-   `KEY_TEXT_LCD_CONTRAST`
-   `KEY_FRACTIONALMETRICS`
-   `KEY_ALPHA_INTERPOLATION`
-   `KEY_COLOR_RENDERING`
-   `KEY_STROKE_CONTROL`
-   `KEY_RESOLUTION_VARIANT`