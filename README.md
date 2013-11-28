# Cascading Compatibility Test Suite

This project will test the binary compatibility of Cascading with differing releases of an underlying platform.

Cascading 2.5 is currently built and published against Apache Hadoop 1.2.x. So is fully tested with that Apache release.

This suite allows other distributions to be verified as being compatible with the current Cascading release as available
through Maven and through other tools apart of the [Cascading ecosystem](http://www.cascading.org/extensions/).

To see the current results, visit the [Cascading Compatibility](http://cascading.org/support/compatibility/) page.

See the FAQ, included in this project, for more information.

## Details

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

Any non-Apache Haddoop distribution may fork this project and add a sub-project specific to the vendor distribution. You
may want to send the test results to the Cascading project, so that your distribution will be listed on the
[Compatibility page](http://cascading.org/support/compatibility).

To add a sub-project, copy the example project to a new directory named after the new distribution. For example,

```bash
  > cp -R example-1.0 vendor-1.0
```

If your distribution is based on Hadoop2, use the `example-hadoop2-1.0` directory.

Update the `distribution.properties` file with all information about your distribution. Please make sure to fill in all
fields, if something does not apply to your distribution, leave it empty. The meaning of each property is explained
below:

```
    # the internal name of the distribution, used as the key
    distribution.name=vendor

    # the version of the distribution you are testing
    distribution.version=1.0.0

    # the name used for displaying on http://cascading.org/support/compatibility
    distribution.displayname=HadoopVendor

    # the homepage of your distribution
    distribution.url=http://vendor.example.com

    # dowload link of your distribution
    distribution.downloadurl=http://vendor.example.com/1.0/download

    # release notes of the version at hand
    distribution.releaseurl=http://vendor.example.com/1.0/releasenotes.html

    # maven repository of the distribution
    distribution.mavenrepo=http://maven.example.com/repository

    # does the distribution ship with the cascading SDK?
    distribution.includessdk=yes
```


Afterwards add your distribution to the `settings.gradle` to include the new directory
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

If your distribution is not deployed to a maven repository, please see the FAQ on how to run the tests without one.

If the tests have to run on a remote cluster, change the test settings at the bottom of your `build.gradle` file.

If you distribution supports both hadoop1 and hadoop2, you have to add 2 sub-projects to the test setup; one for each set of hadoop APIs.

## Sending cascading compatibility results

The build contains a task to send test results back to the cascading project. If you want to send the results of your
distribution upstream use the `uploadResults` tasks like this:

```bash
  > gradle :vendor-1.0:test uploadResults -i
```

If you have multiple versions of your distribution and you want to upload results for all of them (which you should),
add them to the same gradle run:

```bash
  > gradle :vendor-1.0:test :vendor-2.0:test :vendor-42:test  uploadResults -i
```

Please note, that the build will fail on the first test failure, if the `uploadResults` task is enabled. This is
intentional to prevent reporting failed tests.

Please also note, that in order to guarantee the correctness of the compatibility page, the Cascading project can not
list distributions, which did not follow this process.

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

