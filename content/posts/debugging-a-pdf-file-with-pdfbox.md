---
title: "Debugging a Pdf File With Pdfbox"
summary: Understanding some log messages PDFBox throws for certain files.
date: 2024-02-07T12:42:08-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

When using PDFBox to handle PDF files in Java, you are faced with specific PDF messages, like _"Operator cm has too few operands"_, _"Missing XObject"_, and font related problems.

Instead of using your IDE to debug each document page as objects and properties in the memory, you can use PDBox Command-Line Tools and check the same values graphically. This application will take an existing PDF document and allows to analyze and inspect the internal structure.

## Apache PDFBox 2.0
- Download [pdfbox-app-2.0.30.jar](https://dlcdn.apache.org/pdfbox/2.0.30/pdfbox-app-2.0.30.jar)
- Run `java -jar pdfbox-app-2.y.z.jar PDFDebugger [inputfile]` [^1]

[^1]: https://pdfbox.apache.org/2.0/commandline.html#pdfdebugger

## Apache PDFBox 3.0
- Download [pdfbox-app-3.0.1.jar](https://dlcdn.apache.org/pdfbox/3.0.1/pdfbox-app-3.0.1.jar)
- Run `java -jar pdfbox-app-3.y.z.jar debug [inputfile]` [^2]

[^2]: https://pdfbox.apache.org/3.0/commandline.html#pdfdebugger