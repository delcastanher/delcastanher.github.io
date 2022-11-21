---
title: "Understanding Image Dimensions and Resolution"
summary: What are dimension and resolution, and how they are related.
date: 2022-11-18T14:25:14-03:00
tags: ["Digital image"]
author: "Ricardo Delcastanher"
---

Suppose you have an image on a **physical Letter paper** and want to make it into a **Digital image**.

## Scanning an image

A paper is a paper, a thing from the physical world. A Digital image is information, a pixels matrix inside a computer.
One of the first choices you will need to make is the **resolution**, which is the number of pixels within one inch of an image. [^1]

[^1]: https://www.sony.com/electronics/support/articles/00027623

Let's imagine you choose the resolution of 300 pixels per inch. So, an 8.5 x 11 inches Letter paper starts with an empty 2550 x 3300 pixels matrix.

Now, you begin scanning every inch of the digital image, discovering the color of that specific portion of the physical image, and storing its color RGB values inside the corresponding pixel.

And, once you are kind, you share this information of 300 pixels per inch on a specific header if the image format allows. If the digital image consumer wants to know the best quality when printing, to be nearest to the original, the information is there.

## Printing the digital image

But, if your consumer needs to print on a Letter size paper, but with 100 pixels per inch, for instance?

Well, he has the data: 2550 x 3300 pixels with RGB color information matrix; and he knows how many inches it should become: 8.5 x 11 inches. The difference here is that he needs 100 pixels per inch, although you provided him with the information that 300 would be the best fit. But, cmon, this was just a tip you gave him. The information is the pixel matrix, and he can do what he needs with it.

Now the consumer needs to transform a 2550 x 3300 pixels matrix to the dimension of 850 x 1110 before printing. This means he needs to transform groups of 9 pixels of RGB color information (3 x 3) into one single RGB color information for the new matrix.

It is a tough decision. Nine different RGB colors from the original matrix will become one only RGB color on the new one. Fortunately, there are a lot of known algorithms that could help us with this task. [ImageMagick calls them *Filters*](https://imagemagick.org/Usage/filter/).

Some algorithms are good when downscaling, others when upscaling, but **the new image is definitely not the same as the original**.

## The best approach

Despite being true that converting a physical image to a digital image with high DPI results in a better quality digital image, it is even better to know what resolution the digital image will be used in the end and using it since from the beginning.

**Using the correct DPI information during the whole process leads to fewer conversions and an output near the original image.**