---
title: "Micro benchmarking in Java with JMH"
summary: How Java Microbenchmark Harness works and how to use it in your current project.
date: 2022-07-12T11:36:09-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

[Java Microbenchmark Harness (JMH)](https://github.com/openjdk/jmh) is a Java harness for building, running, and analyzing nano/micro/milli/macro benchmarks written in Java and other languages targeting the JVM. [^1]

[^1]: https://github.com/openjdk/jmh

## What is benchmarking? [^2]

[^2]: https://medium.com/javarevisited/understanding-java-microbenchmark-harness-or-jmh-tool-5b9b90ccbe8d

Suppose you have a code snippet and want to test its execution time. What to do?

The easiest way for that is, find the current millisecond before the method and also find the current millisecond after the method. Then find the difference between both which is the execution time for that particular method.

And, if you want to make the result more accurate, run the method several times and need to find out the average of all execution.

## And what is micro benchmarking?

Its called "micro" because it uses the microseconds scale to measure the time execution. [^3]

[^3]: https://vimeo.com/78900556

However, its scale is configurable in JMH.

Stressing critical portions of code and obtaining metrics that are meaningful is actually difficult in the Java Virtual Machine (JVM) world, because the JVM is an adaptive virtual machine. As we see in [this article](https://www.oracle.com/technical-resources/articles/java/architect-benchmarking.html), the JVM does many optimizations that render the simplest benchmark irrelevant unless many precautions are taken. 

JMH was developed as part of the OpenJDK project and provides a very solid foundation for writing and running benchmarks whose results are not erroneous due to unwanted virtual machine optimizations. It does not prevent the JVM pitfalls but greatly helps mitigate them. [^4]


[^4]: https://www.oracle.com/technical-resources/articles/java/architect-benchmarking.html

## Using JMH in your project

Add [JMH Core](https://mvnrepository.com/artifact/org.openjdk.jmh/jmh-core) and [JMH Generators: Annotation Processors](https://mvnrepository.com/artifact/org.openjdk.jmh/jmh-generator-annprocess) dependencies on Maven.

```XML
<dependency>
    <groupId>org.openjdk.jmh</groupId>
    <artifactId>jmh-core</artifactId>
    <version>1.35</version>
</dependency>
<dependency>
    <groupId>org.openjdk.jmh</groupId>
    <artifactId>jmh-generator-annprocess</artifactId>
    <version>1.35</version>
    <scope>test</scope>
</dependency>
```

Update `maven-compiler-plugin`, adding `annotationProcessorPaths`, and `exec-maven-plugin`. [^5]

[^5]: https://stackoverflow.com/a/62983884

```XML
<plugin>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.8.0</version>
    <configuration>
        <source>1.8</source>
        <target>1.8</target>
        <annotationProcessorPaths>
            <path>
                <groupId>org.openjdk.jmh</groupId>
                <artifactId>jmh-generator-annprocess</artifactId>
                <version>1.35</version>
            </path>
        </annotationProcessorPaths>
    </configuration>
</plugin>
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <executions>
        <execution>
            <id>run-benchmarks</id>
            <phase>integration-test</phase>
            <goals>
                <goal>exec</goal>
            </goals>
            <configuration>
                <classpathScope>test</classpathScope>
                <executable>java</executable>
                <arguments>
                    <argument>-classpath</argument>
                    <classpath />
                    <argument>org.openjdk.jmh.Main</argument>
                    <argument>.*</argument>
                </arguments>
            </configuration>
        </execution>
    </executions>
</plugin>
```

Add the **`@Benchmark`** annotation to the methods on your project that you want to benchmark and run the code below to take the results. [^6]

```Java
Options opt = new OptionsBuilder()
        .include(".*")
        .forks(1)
        .build();

new Runner(opt).run();
```

[^6]: https://github.com/openjdk/jmh/blob/master/jmh-samples/src/main/java/org/openjdk/jmh/samples/JMHSample_01_HelloWorld.java