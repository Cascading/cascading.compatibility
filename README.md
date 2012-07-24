# Cascading Compatibility Test Suite

This project will test the binary compatibility of Cascading with differing releases of an underlying platform.

Cascading 2.0 is current built and published against Apache Hadoop 1.0.x. So is fully tested with that Apache release.

This suite allows other distributions to be verified as being compatible with the current Cascading release as available
through Maven and through other tools apart of the [Cascading ecosystem](http://www.cascading.org/extensions/).


## Detail

Cascading is built against a target set of dependencies and made available through the [Conjars.org](http://conjars.org/)
Maven repository.

In the case of Apache Hadoop, and likely any future platforms, there are multiple stable releases, many of which are
API backwards compatible, but not necessary binary backwards compatible.

A recompile of Cascading across each release should not be necessary. This test suite is intended to help
identify which platform release is not compatible with a current Cascading stable release.

## Running Compatibility Tests

To execute all tests across all platforms, install Gradle 1.0 and execute:

```bash
  > gradle test
```

## Testing a different distribution

Any non-Apache Haddoop distribution may fork this project and add a sub-project specific to the vendor distribution.

To add a sub-project, copy one of the current Apache sub-projects to a new directory named after the new
distribution. For example,

```bash
  > cp -R apache-0.20.2 vendor-1.0
```

Update the `setting.gradle` to include the new directory.

```bash
  > echo "include 'vendor-1.0'" >> settings.gradle
```

Then make the necessary changes in the `vendor-1.0/build.gradle` file. For example, pointing to a different
Maven repository, and updating the [Gradle Maven dependencies](http://gradle.org/docs/current/userguide/artifact_dependencies_tutorial.html).

The line loading the `hadoop-settings.gradle` settings should not be changed.

When updated, to run the tests, call:

```bash
  > gradle :vendor-1.0:test -i
```


## Running a single test

```bash
  > gradle -Dtest.single=CoGroupFieldedPipesPlatformTest :apache-0.20.2:test -i
```

Note that `-i` allows you to see the tests running and is quite useful when calling just the `test` target.

## Creating an IntelliJ module file

This will create an `.iml` file for each sub-project:

```bash
  > gradle ideaModule
```

