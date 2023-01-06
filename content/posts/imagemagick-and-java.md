---
title: "ImageMagick and Java"
summary: It could be as easy or as complicated as you wish.
date: 2023-01-05T09:18:52-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

[ImageMagick's official documentation](https://imagemagick.org/script/develop.php) suggests two libraries to work with Java.

## JMagick

Jmagick looked promising. It *provides an object-oriented Java interface to ImageMagick*. [^1]

[^1]: https://imagemagick.org/script/develop.php

But the linked website is down, and the Maven package has almost no usage [^2], and it is challenging to find any documentation or example. When you find it, it depends on many operational system specific files scattered in different directories. [^3] If you could make it work on your development machine [^4], you would have difficulty making it work on the production machine.

[^2]: https://mvnrepository.com/artifact/jmagick/jmagick

[^3]: https://www.roseindia.net/jmagick/index.shtml

[^4]: https://github.com/techblue/jmagick

## im4java

To use im4java, you only need ImageMagick installed on the host machine. im4java *generates the command line for the ImageMagick commands and passes the generated line to the selected IM-command (using the `java.lang.ProcessBuilder.start()` method)*. [^5]

[^5]: https://im4java.sourceforge.net/index.html

### To use im4java

- [Install ImageMagick on your machine](https://imagemagick.org/script/download.php)
- [Add im4java Maven dependency](https://mvnrepository.com/artifact/org.im4java/im4java)

Now a command line like this:
```bash
convert myimage.jpg -resize 800x600 myimage_small.jpg 
```

Could be written in Java like this: [^6]

[^6]: https://im4java.sourceforge.net/docs/dev-guide.html

```Java
// create command
ConvertCmd cmd = new ConvertCmd();

// create the operation, add images and operators/options
IMOperation op = new IMOperation();
op.addImage("myimage.jpg");
op.resize(800,600);
op.addImage("myimage_small.jpg");

// execute the operation
cmd.run(op);
```

It could be helpful when reusing code and working with conditionals. It also makes it easy to understand complex command line tasks.