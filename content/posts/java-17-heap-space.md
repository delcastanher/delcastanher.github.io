---
title: "Java 17 Heap Space"
summary: "What to do in case of java.lang.OutOfMemoryError: Java heap space on Java 17"
date: 2023-07-11T12:31:29-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

The `java.lang.OutOfMemoryError` error can also be thrown by native library code when a native allocation cannot be satisfied (for example, if swap space is low).

The detail message **Java heap space** indicates that an object could not be allocated in the Java heap. This error does not necessarily imply a memory leak. The problem can be as simple as a configuration issue, where the specified heap size (or the default size, if it is not specified) is insufficient for the application. [^1]

[^1]: https://docs.oracle.com/en/java/javase/17/troubleshoot/troubleshooting-memory-leaks.html#GUID-19F6D28E-75A1-4480-9879-D0932B2F305B

Heap memory is the run time data area from which the memory for all java class instances and arrays is allocated. The heap is created when the Java Virtual Machine starts up and may increase or decrease in size while the application runs. [^2]

[^2]: https://deeptimittalblogger.medium.com/memory-for-java-application-how-much-is-enough-ebc7dfa2fb03

These are heap size default selections: Initial heap size of 1/64 of physical memory; Maximum heap size of 1/4 of physical memory. [^3]

[^3]: https://docs.oracle.com/en/java/javase/17/gctuning/ergonomics.html#GUID-DA88B6A6-AF89-4423-95A6-BBCBD9FAE781

The minimum and maximum heap sizes that the garbage collector can use can be set using `-Xms=<nnn>` and `-Xmx=<mmm>` for minimum and maximum heap size respectively. [^4]

[^4]: https://docs.oracle.com/en/java/javase/17/gctuning/ergonomics.html#GUID-83551BA5-ADEA-4E2E-B60A-3A953DA8FD02

If the heap grows to its maximum size and the throughput goal isn't being met, then the maximum heap size is too small for the throughput goal. Set the maximum heap size to a value that's close to the total physical memory on the platform, but doesn't cause swapping of the application. Execute the application again. If the throughput goal still isn't met, then the goal for the application time is too high for the available memory on the platform. [^5]

[^5]: https://docs.oracle.com/en/java/javase/17/gctuning/ergonomics.html#GUID-034BAF7C-2F2E-483D-8606-0BF2B8710BC9