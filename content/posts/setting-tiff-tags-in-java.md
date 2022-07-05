---
title: "Setting Tiff tags in Java"
summary: Create or modify any Tiff header tags when saving it in Java.
date: 2022-07-05T10:02:33-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

Setting Tiff tags when saving a Tiff file in Java is tricky. PDFBox does this, putting some basic tags on the exported file [^1], but it does not give you the option to change or add tags.

[^1]: https://github.com/apache/pdfbox/blob/trunk/tools/src/main/java/org/apache/pdfbox/tools/imageio/TIFFUtil.java

To achieve the same results as the following codes, you need some Java objects:
-   An [`ImageWriter writer`](/posts/convert-pdf-to-tiff-in-java-with-apache-pdfbox/#convert-a-multi-page-pdf-into-a-multi-page-tiff);
-   An [`ImageWriteParam param`](/posts/tiff-compression-in-java/);
-   A [`BufferedImage bim`](/posts/convert-pdf-to-tiff-in-java-with-apache-pdfbox/#convert-a-multi-page-pdf-into-a-multi-page-tiff).

With these in your hands, create an `IIOMetadata` object.

```Java {linenos=true}
IIOMetadata metadata = writer.getDefaultImageMetadata(new ImageTypeSpecifier(bim), param);
String metaDataFormat = metadata.getNativeMetadataFormatName();
IIOMetadataNode root = new IIOMetadataNode(metaDataFormat);
IIOMetadataNode ifd = new IIOMetadataNode("TIFFIFD");
root.appendChild(ifd);
ifd.appendChild(createAsciiField(305, "Software", "MyApp"));
metadata.mergeTree(metaDataFormat, root);
```

The sixth line is where one specific tag is being created. It uses `createAsciiField()`, one of the methods from [PDFBox's `TIFFUtil` class](https://github.com/apache/pdfbox/blob/trunk/tools/src/main/java/org/apache/pdfbox/tools/imageio/TIFFUtil.java).

```Java
private static IIOMetadataNode createShortField(int tiffTagNumber, String name, int val) {
    IIOMetadataNode field = new IIOMetadataNode("TIFFField");
    field.setAttribute("number", Integer.toString(tiffTagNumber));
    field.setAttribute("name", name);
    IIOMetadataNode arrayNode = new IIOMetadataNode("TIFFShorts");
    field.appendChild(arrayNode);
    IIOMetadataNode valueNode = new IIOMetadataNode("TIFFShort");
    arrayNode.appendChild(valueNode);
    valueNode.setAttribute("value", Integer.toString(val));
    return field;
}

private static IIOMetadataNode createAsciiField(int number, String name, String val) {
    IIOMetadataNode field = new IIOMetadataNode("TIFFField");
    field.setAttribute("number", Integer.toString(number));
    field.setAttribute("name", name);
    IIOMetadataNode arrayNode = new IIOMetadataNode("TIFFAsciis");
    field.appendChild(arrayNode);
    IIOMetadataNode valueNode = new IIOMetadataNode("TIFFAscii");
    arrayNode.appendChild(valueNode);
    valueNode.setAttribute("value", val);
    return field;
}

private static IIOMetadataNode createLongField(int number, String name, long val) {
    IIOMetadataNode field = new IIOMetadataNode("TIFFField");
    field.setAttribute("number", Integer.toString(number));
    field.setAttribute("name", name);
    IIOMetadataNode arrayNode = new IIOMetadataNode("TIFFLongs");
    field.appendChild(arrayNode);
    IIOMetadataNode valueNode = new IIOMetadataNode("TIFFLong");
    arrayNode.appendChild(valueNode);
    valueNode.setAttribute("value", Long.toString(val));
    return field;
}

private static IIOMetadataNode createRationalField(int number, String name, int numerator, int denominator) {
    IIOMetadataNode field = new IIOMetadataNode("TIFFField");
    field.setAttribute("number", Integer.toString(number));
    field.setAttribute("name", name);
    IIOMetadataNode arrayNode = new IIOMetadataNode("TIFFRationals");
    field.appendChild(arrayNode);
    IIOMetadataNode valueNode = new IIOMetadataNode("TIFFRational");
    arrayNode.appendChild(valueNode);
    valueNode.setAttribute("value", numerator + "/" + denominator);
    return field;
}
```

There is one method for [each Tiff tag type](https://www.loc.gov/preservation/digital/formats/content/tiff_tags.shtml).

Let's take the **DocumentName** Tiff tag as an example. Suppose you want to add this tag to your Tiff file. As you can see on https://www.awaresystems.be/imaging/tiff/tifftags/documentname.html it has:
-   Type: **ASCII**
    -   So you need to use the `createAsciiField()` method
-   Code: **269**
    -   This is the first `createAsciiField()` parameter
-   Name: **DocumentName**
    -   This is the second `createAsciiField()` parameter
-   The third `createAsciiField()` parameter is the value you want inside the tag. In this case, the value inside the **DocumentName** tag.

```Java
ifd.appendChild(createAsciiField(269, "DocumentName", "My Document Name"));
```

You can use the logic above for all other tags and repeat the sixth line for each tag chosen before the `metadata.mergeTree(metaDataFormat, root);` call.

Once the `IIOMetadata metadata` object is created, you can pass it as the third parameter of the [`IIOImage`](https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/javax/imageio/IIOImage.html) constructor, that in turn, is the first parameter of [`javax.imageio.ImageWriter.write()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/javax/imageio/ImageWriter.html#write(javax.imageio.metadata.IIOMetadata,javax.imageio.IIOImage,javax.imageio.ImageWriteParam)) and [`javax.imageio.ImageWriter.writeToSequence()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/javax/imageio/ImageWriter.html#writeToSequence(javax.imageio.IIOImage,javax.imageio.ImageWriteParam)).

In our [Convert a multi-page PDF into a multi-page Tiff](/posts/convert-pdf-to-tiff-in-java-with-apache-pdfbox/) example, the `writeToSequence()` call now should be:

```Java
writer.writeToSequence(new IIOImage(bim, null, metadata), null);
```