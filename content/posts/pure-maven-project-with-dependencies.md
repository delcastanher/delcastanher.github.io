---
title: "Pure Maven Java project with dependencies"
summary: For when you just want to test a Maven dependency in pure Java.
date: 2022-06-25T08:45:27-03:00
tags: ["Java"]
author: "Ricardo Delcastanher"
---

You are looking for Java libraries to solve a particular demand, and you need to test them without polluting your current project. Still, you don't want to configure a Java framework too.

## Step 1: Create the simplest Maven project[^1]

[^1]: https://maven.apache.org/guides/getting-started/index.html#how-do-i-make-my-first-maven-project

```Bash
mvn -B archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4
cd my-app
```

## Step 2: [Add the dependency](https://maven.apache.org/guides/getting-started/index.html#how-do-i-use-external-dependencies) you want to *test drive*

And program the needed code.

## Step 3: [Add the plugin](https://maven.apache.org/guides/getting-started/index.html#how-do-i-use-plugins) to `pom.xml` file[^2]

[^2]: https://stackoverflow.com/questions/574594/how-can-i-create-an-executable-jar-with-dependencies-using-maven

```XML
<plugin>
    <artifactId>maven-assembly-plugin</artifactId>
    <configuration>
    <archive>
        <manifest>
        <mainClass>fully.qualified.MainClass</mainClass>
        </manifest>
    </archive>
    <descriptorRefs>
        <descriptorRef>jar-with-dependencies</descriptorRef>
    </descriptorRefs>
    </configuration>
</plugin>
```

## Step 4: Compile

```Bash
mvn clean compile assembly:single
```

## Step 5: Run

```Bash
java -cp target/my-app-1.0-SNAPSHOT-jar-with-dependencies.jar com.mycompany.app.App
```