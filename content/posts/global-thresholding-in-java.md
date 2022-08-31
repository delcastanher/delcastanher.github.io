---
title: "Global Thresholding in Java"
summary: Implementing ImageJ Auto Threshold methods in any project.
date: 2022-08-30T09:09:40-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

A global thresholding technique is one which makes use of a single threshold value for the whole image, whereas local thresholding technique makes use of unique threshold values for the partitioned subimages obtained from the whole image. [^1]

[^1]: https://www.sciencedirect.com/topics/engineering/global-thresholding

[ImageJ Auto Threshold plugin](https://imagej.net/plugins/auto-threshold) is an open source Java implementation that brings us seventeen global thresholding methods for processing and analyzing scientific images, [converting them to black and white](/posts/converting-colors-in-java/).

To use them in your project, you will need the histogram of your `BufferedImage bufferedImage` object, an array with 256 positions (one for each 8 bits color) with the counting of the color occurrence on the original image. [^2]

[^2]: https://github.com/imagej/imagej2/blob/7ffb50366de028f4c6e423b3b5fe7fedcb4d678b/src/ij/process/ByteProcessor.java#L1171

```Java
byte[] pixels = ((DataBufferByte) bufferedImage.getRaster().getDataBuffer()).getData();

int[] histogram = new int[256];
for (int y = 0; y < (bufferedImage.getHeight()); y++) {
    int i = y * bufferedImage.getWidth();
    for (int x = 0; x < (bufferedImage.getWidth()); x++) {
        int v = pixels[i++] & 0xff;
        histogram[v]++;
    }
}
```

Copy the [Auto Threshold](https://imagej.net/plugins/auto-threshold) method of your choice from https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java to your project and pass your `histogram` as a parameter.
-  [IJDefault](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L409)
-  [Huang](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L447)
-  [Huang2](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L533)
-  [Intermodes](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L603)
   -   [`bimodalTest`](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L586) method needed
-  [IsoData](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L653)
-  [Li](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L715)
-  [MaxEntropy](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L797)
-  [Mean](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L882)
-  [MinErrorI](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L897)
   -   [`A`](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L945), [`B`](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L952), and [`C`](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L959) methods needed
-  [Minimum](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L966)
   -   [`bimodalTest`](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L586) method needed
-  [Moments](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L1017)
-  [Otsu](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L1069)
-  [Percentile](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L1125)
   -   [`partialSum`](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L1155) method needed
-  [RenyiEntropy](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L1163)
-  [Shanbhag](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L1364)
-  [Triangle](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L1447)
-  [Yen](https://github.com/fiji/Auto_Threshold/blob/master/src/main/java/fiji/threshold/Auto_Threshold.java#L1550)

```Java
int threshold = IJDefault(histogram);
```

Once you run the method, it will return a `threshold` value, which you can use on your `BufferedImage bufferedImage` object, to decide which value is white and which is black.

```Java
for (int y = 0; y < bufferedImage.getHeight(); y++) {
    for (int x = 0; x < bufferedImage.getWidth(); x++) {
        int i = y * bufferedImage.getWidth() + x;
        if ((pixels[i] & 0xff) > threshold)
            bufferedImage.setRGB(x, y, Color.WHITE.getRGB());
        else
            bufferedImage.setRGB(x, y, Color.BLACK.getRGB());
    }
}
```

Now that your image has only white and black values, you can use a `TYPE_BYTE_BINARY` `BufferedImage`.

```Java
BufferedImage bwBufferedImage = new BufferedImage(bufferedImage.getWidth(), bufferedImage.getHeight(), BufferedImage.TYPE_BYTE_BINARY);
Graphics2D graphics2D = bwBufferedImage.createGraphics();
graphics2D.drawImage(bim, 0, 0, bufferedImage.getWidth(), bufferedImage.getHeight(), null);
graphics2D.dispose();
```