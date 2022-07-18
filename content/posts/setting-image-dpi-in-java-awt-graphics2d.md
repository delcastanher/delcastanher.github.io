---
title: "Setting image DPI in Java AWT Graphics2D"
summary: Graphics2D does not have an option to choose images DPI directly.
date: 2022-07-18T16:45:48-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

When using [Java AWT Graphics2D](https://docs.oracle.com/javase/10/docs/api/java/awt/Graphics2D.html) for creating images in memory, the default image DPI (Dots Per Inch) value is 72. And it has no direct way of choosing or changing this.

> _The scaling of the default transform is set to identity for those devices that are close to 72 dpi, such as screen devices._ [^1]

[^1]: https://docs.oracle.com/javase/10/docs/api/java/awt/Graphics2D.html

To set an image DPI other than 72, you need to find the relation between the desired DPI and 72 and use it combined with Graphics2D [`scale`](https://docs.oracle.com/javase/10/docs/api/java/awt/Graphics2D.html#scale(double,double)) method. [^2]

[^2]: https://community.oracle.com/tech/developers/discussion/comment/5454467/#Comment_5454467

Apache PDFBox does the first part on method `renderImageWithDPI` of [PDFRenderer class](https://github.com/apache/pdfbox/blob/trunk/pdfbox/src/main/java/org/apache/pdfbox/rendering/PDFRenderer.java).

```Java
public BufferedImage renderImageWithDPI(int pageIndex, float dpi) throws IOException
{
    return renderImage(pageIndex, dpi / 72f, ImageType.RGB);
}

public BufferedImage renderImageWithDPI(int pageIndex, float dpi, ImageType imageType) throws IOException
{
    return renderImage(pageIndex, dpi / 72f, imageType);
}
```

The second part is on `graphics.scale(scaleX, scaleY);` line of the `transform` method.

```Java
private void transform(Graphics2D graphics, int rotationAngle, PDRectangle cropBox, float scaleX, float scaleY)
{
    graphics.scale(scaleX, scaleY);

    if (rotationAngle != 0)
    {
        float translateX = 0;
        float translateY = 0;
        switch (rotationAngle)
        {
            case 90:
                translateX = cropBox.getHeight();
                break;
            case 270:
                translateY = cropBox.getWidth();
                break;
            case 180:
                translateX = cropBox.getWidth();
                translateY = cropBox.getHeight();
                break;
            default:
                break;
        }
        graphics.translate(translateX, translateY);
        graphics.rotate(Math.toRadians(rotationAngle));
    }
}
```