---
title: "The Count 2 Tiff Tag in Java"
summary: How to set Tiff tags that receive two short values in Java.
date: 2022-07-08T11:12:35-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

To set Tiff tags in Java, you need to specify the tag's code, name, and value, as explained [here](/posts/setting-tiff-tags-in-java/).

But certain tags hold a peculiarity of having a "Count 2" attribute, which means that one tag can receive two values:
-   [PageNumber](http://www.awaresystems.be/imaging/tiff/tifftags/pagenumber.html)
-   [HalftoneHints](https://www.awaresystems.be/imaging/tiff/tifftags/halftonehints.html)
-   [DotRange](https://www.awaresystems.be/imaging/tiff/tifftags/dotrange.html)
-   [YCbCrSubsampling](http://www.awaresystems.be/imaging/tiff/tifftags/ycbcrsubsampling.html)

Setting these tags in Java requires this code in addition to [PDFBox codes](https://github.com/apache/pdfbox/blob/trunk/tools/src/main/java/org/apache/pdfbox/tools/imageio/TIFFUtil.java).

```Java
private static IIOMetadataNode createShortField(int tiffTagNumber, String name, int val1, int val2) {
    IIOMetadataNode field = new IIOMetadataNode("TIFFField");
    field.setAttribute("number", Integer.toString(tiffTagNumber));
    field.setAttribute("name", name);
    IIOMetadataNode arrayNode = new IIOMetadataNode("TIFFShorts");
    field.appendChild(arrayNode);
    IIOMetadataNode valueNode1 = new IIOMetadataNode("TIFFShort");
    arrayNode.appendChild(valueNode1);
    valueNode1.setAttribute("value", Integer.toString(val1));
    IIOMetadataNode valueNode2 = new IIOMetadataNode("TIFFShort");
    arrayNode.appendChild(valueNode2);
    valueNode2.setAttribute("value", Integer.toString(val2));
    return field;
}
```