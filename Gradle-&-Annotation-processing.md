Annotation processing takes a few steps to setup correctly in gradle.

**Which plugin to use?**

For Android projects use [android-apt](https://bitbucket.org/hvisser/android-apt):

```gradle
buildscript {
    dependencies {
        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
    }
}

apply plugin: 'com.neenbedankt.android-apt'
```

For Java projects use [gradle-apt-plugin](https://github.com/tbroyer/gradle-apt-plugin):

```gradle
buildscript {
    repositories {
        maven {
            url "https://plugins.gradle.org/m2/"
        }
    }
    dependencies {
        classpath "net.ltgt.gradle:gradle-apt-plugin:0.5"
    }
}

apply plugin: `net.ltgt.apt`
```

Note the need for plugins for proper annotation processing may get addressed in a future version of gradle, as described [here](https://github.com/gradle/gradle/blob/master/design-docs/java-annotation-processing.md)

**Why not use the `provided` scope?**

You can use `provided` scope on an annotation processor dependency in gradle like so:

```gradle
provided 'io.requery:requery-processor:1.0-SNAPSHOT'
```

This works and the processor will run and generate source files. However there are problems:

1. In IntelliJ the generated files are not visible to the IDE and appear red, even though the code compiles.

2. The dependencies of the processor are now put on to your project classpath which is not desired.

