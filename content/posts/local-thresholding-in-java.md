---
title: "Local Thresholding in Java"
summary: Implementing ImageJ Auto Local Threshold methods in any project.
date: 2024-03-07T14:00:00-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

By **local** here is meant that the threshold is computed for each pixel according to the image characteristings within a window of radius **r** (in pixel units) around it. The segmented phase is always shown as white (255). [^1]

[^1]: https://imagej.net/plugins/auto-local-threshold

For global thresholding rather than local, see [Global Thresholding in Java](/posts/global-thresholding-in-java).

[ImageJ Auto Local Threshold plugin](https://imagej.net/plugins/auto-local-threshold) is an open source Java implementation that brings us nine local thresholding methods for processing and analyzing scientific images, [converting them to black and white](/posts/converting-colors-in-java/).

To use ImageJ as a library in your Maven project, add the dependency: [^2]

[^2]: https://github.com/imagej/ImageJ?tab=readme-ov-file#using-imagej-as-a-dependency

```XML
<dependency>
    <groupId>net.imagej</groupId>
    <artifactId>ij</artifactId>
    <version>1.53j</version>
</dependency>
```
Copy the [Auto Local Threshold](https://imagej.net/plugins/auto-local-threshold) method of your choice from https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java to your project
-  [duplicateImage](https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java#L775)
    - required for all methods.
-  [Bernsen](https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java#L255)
-  [Contrast](https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java#L314)
-  [Mean](https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java#L356)
-  [Median](https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java#L396)
-  [MidGrey](https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java#L433)
-  [Niblack](https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java#L477)
-  [Otsu](https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java#L537)
-  [Phansalkar](https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java#L632)
-  [Sauvola](https://github.com/fiji/Auto_Local_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Local_Threshold.java#L715)

After the image is in memory as a `BufferedImage bufferedImage` object, you need to load it into an ImageJ `ImagePlus` object and pass it as a parameter to the chosen method.

```Java
ImagePlus imagePlus = new ImagePlus();
imagePlus.setImage(bufferedImage);
Bernsen(imagePlus, 15, 0, 0, true);
imagePlus.updateAndDraw();
imagePlus.getProcessor().setThreshold(255, 255, ImageProcessor.NO_LUT_UPDATE);
bufferedImage = imagePlus.getBufferedImage();
```
Now that your image has only white and black values, you can use a `TYPE_BYTE_BINARY` `BufferedImage`.

```Java
BufferedImage bwBufferedImage = new BufferedImage(bufferedImage.getWidth(), bufferedImage.getHeight(), BufferedImage.TYPE_BYTE_BINARY);
Graphics2D graphics2D = bwBufferedImage.createGraphics();
graphics2D.drawImage(bim, 0, 0, bufferedImage.getWidth(), bufferedImage.getHeight(), null);
graphics2D.dispose();
```