---
title: "Convert PDF to Tiff in Java with Apache PDFBox"
summary: One image file per PDF page and multiple pages image file
date: 2022-06-26T17:49:21-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

## Add [Apache PDFBox](https://mvnrepository.com/artifact/org.apache.pdfbox/pdfbox) and [Apache PDFBox Tools](https://mvnrepository.com/artifact/org.apache.pdfbox/pdfbox-tools) dependencies on Maven.

```XML
<dependency>
    <groupId>org.apache.pdfbox</groupId>
    <artifactId>pdfbox</artifactId>
    <version>2.0.26</version>
</dependency>
<dependency>
    <groupId>org.apache.pdfbox</groupId>
    <artifactId>pdfbox-tools</artifactId>
    <version>2.0.26</version>
</dependency>
```

## Convert each PDF page as a separated Tiff file

For the following pieces of code, you will need a `String` variable `pdfFilename` with the path and name of the PDF file[^1]. It will be used to load the PDF file and write the Tiff files.

[^1]: https://pdfbox.apache.org/2.0/migration.html#working-with-images

```Java
PDDocument document = PDDocument.load(new File(pdfFilename));
PDFRenderer pdfRenderer = new PDFRenderer(document);
int pageCounter = 0;
for (PDPage page : document.getPages())
{
    BufferedImage bim = pdfRenderer.renderImageWithDPI(pageCounter, 300, ImageType.RGB);
    ImageIOUtil.writeImage(bim, pdfFilename + "-" + (pageCounter++) + ".tiff", 300);
}
document.close();
```

-   Note that the page number parameter is zero based.
-   The `renderImageWithDPI` third parameter accepts the following values:
    -   `ImageType.RGB`, `ImageType.ARGB` and `ImageType.BGR` for coloured images;
    -   `ImageType.GRAY` for grayscale images;
    -   `ImageType.BINARY` for pure black and white images.
-   The suffix in `ImageIOUtil.writeImage` second parameter will be used as the file format.

## Convert a multi-page PDF into a multi-page Tiff

You can write multi-page images (in formats that support it, like Tiff) using the standard `ImageIO` API **starting from Java 9**[^2].

[^2]: https://stackoverflow.com/a/41038987

```Java
PDDocument document = PDDocument.load(new File(pdfFilename));
PDFRenderer pdfRenderer = new PDFRenderer(document);
FileImageOutputStream output = new FileImageOutputStream(new File(pdfFilename + ".tiff"));
ImageWriter writer = ImageIO.getImageWritersByFormatName("TIFF").next();
writer.setOutput(output);
writer.prepareWriteSequence(null);
int pageCounter = 0;
for (PDPage page : document.getPages())
{
    BufferedImage bim = pdfRenderer.renderImageWithDPI(pageCounter++, 300, ImageType.RGB);
    writer.writeToSequence(new IIOImage(bim, null, null), null);
}
writer.endWriteSequence();
writer.dispose();
output.close();
document.close();
```

-   Note that the page number parameter is zero based.
-   The `renderImageWithDPI` third parameter accepts the following values:
    -   `ImageType.RGB`, `ImageType.ARGB` and `ImageType.BGR` for coloured images;
    -   `ImageType.GRAY` for grayscale images;
    -   `ImageType.BINARY` for pure black and white images.