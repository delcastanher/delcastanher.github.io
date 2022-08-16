---
title: "Using OpenCV in Java with Maven"
summary: How to easily use OpenCV in Java with JavaCV.
date: 2022-08-16T10:16:05-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

You can use OpenCV directly in Java, as brainily taught in [Tutorials Point](https://www.tutorialspoint.com/java_dip/introduction_to_opencv.htm), updating some versions and paying attention to changelogs.

But when working on a Maven project, it is better to declare dependencies in Maven. And that is where [JavaCV](https://github.com/bytedeco/javacv) enters.

## Add [JavaCV dependencies](https://mvnrepository.com/artifact/org.bytedeco/javacv) on Maven.

```XML
<dependency>
    <groupId>org.bytedeco</groupId>
    <artifactId>javacv</artifactId>
    <version>1.5.7</version>
</dependency>
```

If you don't have OpenCV installed and configured on the host machine, add [JavaCV Platform](https://mvnrepository.com/artifact/org.bytedeco/javacv-platform) as well.

```XML
<dependency>
    <groupId>org.bytedeco</groupId>
    <artifactId>javacv-platform</artifactId>
    <version>1.5.7</version>
</dependency>
```

## OpenCV GrayScale Conversion

From https://www.tutorialspoint.com/java_dip/grayscale_conversion_opencv.htm to JavaCV.

```Java
import org.bytedeco.opencv.opencv_core.Mat;

import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;

import static org.bytedeco.javacv.Java2DFrameUtils.toBufferedImage;
import static org.bytedeco.javacv.Java2DFrameUtils.toMat;
import static org.bytedeco.opencv.global.opencv_core.CV_8UC1;
import static org.bytedeco.opencv.global.opencv_imgproc.COLOR_RGB2GRAY;
import static org.bytedeco.opencv.global.opencv_imgproc.cvtColor;

public class Main {
    public static void main(String[] args) {

        try {
            File input = new File("digital_image_processing.jpg");
            BufferedImage image = ImageIO.read(input);

            Mat mat = toMat(image);

            Mat mat1 = new Mat(image.getHeight(),image.getWidth(), CV_8UC1);
            cvtColor(mat, mat1, COLOR_RGB2GRAY);

            BufferedImage image1 = toBufferedImage(mat1);

            File ouptut = new File("grayscale.jpg");
            ImageIO.write(image1, "jpg", ouptut);

        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

```

To see the Mat object as a matrix of values, run:

```Java
mat.createIndexer();
```

## OpenCV Color Space Conversion

From https://www.tutorialspoint.com/java_dip/color_space_conversion.htm to JavaCV.

```Java
import org.bytedeco.opencv.opencv_core.Mat;

import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;

import static org.bytedeco.javacv.Java2DFrameUtils.toBufferedImage;
import static org.bytedeco.javacv.Java2DFrameUtils.toMat;
import static org.bytedeco.opencv.global.opencv_core.CV_8UC3;
import static org.bytedeco.opencv.global.opencv_imgproc.COLOR_RGB2HSV;
import static org.bytedeco.opencv.global.opencv_imgproc.cvtColor;

public class Main {
    public static void main(String[] args) {

        try {
            File input = new File("digital_image_processing.jpg");
            BufferedImage image = ImageIO.read(input);
            Mat mat = toMat(image);

            Mat mat1 = new Mat(image.getHeight(), image.getWidth(), CV_8UC3);
            cvtColor(mat, mat1, COLOR_RGB2HSV);

            BufferedImage image1 = toBufferedImage(mat1);

            File ouptut = new File("hsv.jpg");
            ImageIO.write(image1, "jpg", ouptut);

        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

## Basic Thresholding

From https://www.tutorialspoint.com/java_dip/basic_thresholding.htm to JavaCV.

```Java
import org.bytedeco.opencv.opencv_core.Mat;

import static org.bytedeco.opencv.global.opencv_imgcodecs.*;
import static org.bytedeco.opencv.global.opencv_imgproc.THRESH_TOZERO;
import static org.bytedeco.opencv.global.opencv_imgproc.threshold;

public class Main {
    public static void main(String[] args) {

        Mat source = imread("digital_image_processing.jpg", IMREAD_COLOR);
        Mat destination = new Mat(source.rows(), source.cols(), source.type());

        destination = source;
        threshold(source, destination, 127, 255, THRESH_TOZERO);
        imwrite("ThreshZero.jpg", destination);

    }
}
```