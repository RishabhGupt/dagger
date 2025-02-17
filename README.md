# Dagger

[![Maven Central][mavenbadge-svg]][mavencentral]

A fast dependency injector for Java and Android.

Dagger is a compile-time framework for dependency injection. It uses no
reflection or runtime bytecode generation, does all its analysis at
compile-time, and generates plain Java source code.

Dagger is actively maintained by the same team that works on [Guava]. Snapshot
releases are auto-deployed to Sonatype's central Maven repository on every clean
build with the version `HEAD-SNAPSHOT`. The current version builds upon previous
work done at [Square][square].

## Documentation

You can [find the dagger documentation here][website] which has extended usage
instructions and other useful information. More detailed information can be
found in the [API documentation][latestapi].

You can also learn more from [the original proposal][proposal],
[this talk by Greg Kick][gaktalk], and on the dagger-discuss@googlegroups.com
mailing list.

## Installation

### Bazel

If you build with `bazel`, follow the [`bazel` documentation for referencing
external projects][bazel-external-deps] to include Dagger in your build.

Given the following `WORKSPACE` definition, you can reference dagger via
`@com_google_dagger//:dagger_with_compiler` in your deps.

```python
http_archive(
    name = "com_google_dagger",
    urls = ["https://github.com/google/dagger/archive/dagger-<version>.zip"],
)
```

### Other build systems

You will need to include the `dagger-2.x.jar` in your application's runtime.
In order to activate code generation and generate implementations to manage
your graph you will need to include `dagger-compiler-2.x.jar` in your build
at compile time.

#### Maven

In a Maven project, include the `dagger` artifact in the dependencies section
of your `pom.xml` and the `dagger-compiler` artifact as an
`annotationProcessorPaths` value of the `maven-compiler-plugin`:

```xml
<dependencies>
  <dependency>
    <groupId>com.google.dagger</groupId>
    <artifactId>dagger</artifactId>
    <version>2.x</version>
  </dependency>
</dependencies>
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.6.1</version>
      <configuration>
        <annotationProcessorPaths>
          <path>
            <groupId>com.google.dagger</groupId>
            <artifactId>dagger-compiler</artifactId>
            <version>2.x</version>
          </path>
        </annotationProcessorPaths>
      </configuration>
    </plugin>
  </plugins>
</build>
```

If you are using a version of the `maven-compiler-plugin` lower than `3.5`, add
the `dagger-compiler` artifact with the `provided` scope:

```xml
<dependencies>
  <dependency>
    <groupId>com.google.dagger</groupId>
    <artifactId>dagger</artifactId>
    <version>2.x</version>
  </dependency>
  <dependency>
    <groupId>com.google.dagger</groupId>
    <artifactId>dagger-compiler</artifactId>
    <version>2.x</version>
    <scope>provided</scope>
  </dependency>
</dependencies>
```

If you use the beta `dagger-producers` extension (which supplies
parallelizable execution graphs), then add this to your maven configuration:

```xml
<dependencies>
  <dependency>
    <groupId>com.google.dagger</groupId>
    <artifactId>dagger-producers</artifactId>
    <version>2.x</version>
  </dependency>
</dependencies>
```

#### Java Gradle
```groovy
// Add plugin https://plugins.gradle.org/plugin/net.ltgt.apt
plugins {
  id "net.ltgt.apt" version "0.10"
}

// Add Dagger dependencies
dependencies {
  compile 'com.google.dagger:dagger:2.x'
  apt 'com.google.dagger:dagger-compiler:2.x'
}
```

#### Android Gradle
```groovy
// Add Dagger dependencies
dependencies {
  compile 'com.google.dagger:dagger:2.x'
  annotationProcessor 'com.google.dagger:dagger-compiler:2.x'
}
```

If you're using classes in `dagger.android` you'll also want to include:

```groovy
compile 'com.google.dagger:dagger-android:2.x'
compile 'com.google.dagger:dagger-android-support:2.x' // if you use the support libraries
annotationProcessor 'com.google.dagger:dagger-android-processor:2.x'
```

If you're using a version of the Android gradle plugin below `2.2`, see
https://bitbucket.org/hvisser/android-apt.

If you're using the [Android Databinding library][databinding], you may want to
increase the number of errors that `javac` will print. When Dagger prints an
error, databinding compilation will halt and sometimes print more than 100
errors, which is the default amount for `javac`. For more information, see
[Issue 306](https://github.com/google/dagger/issues/306).

```groovy
gradle.projectsEvaluated {
  tasks.withType(JavaCompile) {
    options.compilerArgs << "-Xmaxerrs" << "500" // or whatever number you want
  }
}
```

### Resources

*   [Documentation][website]
*   [Javadocs][latestapi]
*   [GitHub Issues]


If you do not use maven, gradle, ivy, or other build systems that consume
maven-style binary artifacts, they can be downloaded directly via the
[Maven Central Repository][mavencentral].

Developer snapshots are available from Sonatype's
[snapshot repository][dagger-snap], and are built on a clean build of
the GitHub project's master branch.

## Building Dagger

See [the CONTRIBUTING.md docs][Building Dagger].

## License

    Copyright 2012 The Dagger Authors

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

[`bazel`]: https://bazel.build
[bazel-external-deps]: https://docs.bazel.build/versions/master/external.html#depending-on-other-bazel-projects
[Building Dagger]: CONTRIBUTING.md#building-dagger
[dagger-snap]: https://oss.sonatype.org/content/repositories/snapshots/com/google/dagger/
[databinding]: https://developer.android.com/topic/libraries/data-binding/
[gaktalk]: https://www.youtube.com/watch?v=oK_XtfXPkqw
[GitHub Issues]: https://github.com/google/dagger/issues
[Guava]: https://github.com/google/guava
[latestapi]: https://dagger.dev/api/latest/
[mavenbadge-svg]: https://maven-badges.herokuapp.com/maven-central/com.google.dagger/dagger/badge.svg
[mavencentral]: https://search.maven.org/artifact/com.google.dagger/dagger
[project]: http://github.com/google/dagger/
[proposal]: https://github.com/square/dagger/issues/366
[square]: http://github.com/square/dagger/
[website]: https://dagger.dev
