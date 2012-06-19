# Cascading Compatibility Test Suite

This project will test the binary compatibility of Cascading with differing releases of an
underlying platform.

The only currently supported platform is Apache Hadoop.

## Detail

Cascading is built against a target set of dependencies and made available through the [Conjars.org](http://conjars.org/)
Maven repository.

In the case of Apache Hadoop, and likely any future platforms, there are multiple stable releases, many of which are
API backwards compatible, but not necessary binary backwards compatible.

A recompile of Cascading across each release should not be necessary. This test suite is intended to help
identify which platform release is not compatible with a current Cascading stable release.

Any non-Apache Haddoop distribution may fork this project and add a sub-project specific to the vendor distribution.

## Running Compatibility Tests

To execute all tests across all platforms

```bash
  > gradle test
```

## Other

### Running a single test

```bash
 > gradle -Dtest.single=CoGroupFieldedPipesPlatformTest :apache-0.20.2:test -i
```

Note that `-i` allows you to see the tests running and is quite useful when calling just the `test` target.

### Creating an IntelliJ module file

This will create an `.iml` file for each sub-project:

```bash
  > gradle ideaModule
```

