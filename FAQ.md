# Frequently Asked Questions

## Why is this compatibility test needed?

Cascading is compiled against the latest stable release of Apache Hadoop and made available through
a Maven repository hosted on [Conjars](http://conjars.org/) as a convenience to developers.

Many developers of open-source project and internal proprietary projects use Maven to pull down Cascading,
its dependencies, and any project dependencies. Tools like Ant, Ivy, Maven, Leiningen, SBT, and Gradle use
Maven repositories for builds.

Because Cascading is built against the latest stable Apache Hadoop distribution, it will not run reliably on
a Hadoop distribution (other than Apache) that may have changed public Hadoop API calls in an incompatible way.

## What do the compatibility tests test?

They verify that a pre-built version Cascading, built against the latest stable Apache Hadoop distribution, is
'binary compatible' with another custom Hadoop build.

If a Java API changes, it may be source compatible with a consumer of that API, if the consumer (Cascading) is
recompiled against the new API signature.

But often a compatible change handled by a recompile isn't binary compatible. That is, compiling against one version
of the API may succeed, but executing the compiled signature against a slightly modified API may fail.

This is most commonly seen if the return type of a method is changed.

## Where do I find a listing of compatible Hadoop builds?

See the [Compatibility](http://www.cascading.org/support/compatibility/) page for a current listing.

## How do I add a new sub-project to be tested?

After forking this project on GitHub, review the section titles "Testing a different distribution" in the
README file.

## How do I have my Hadoop distribution listed on the compatibility page?

Execute the `uploadResults` task after your tests, as explained in the README file.

## Is a Maven repository required?

No.

This is the simplest way to have Gradle find your Hadoop build. But you can update the test CLASSPATH to
include jars from a local directory, or download a tar/zip file and unpack it into the CLASSPATH.

Suppose you want to run the tests against the jars of an apache hadoop release
tarball, which has been unpacked in `/home/hadoop`, then your `build.gradle`
file should look something like this:

```
def hadoopVersion = distProps[ "distribution.version" ]

// get default hadoop configuration
apply from: "${rootDir}/settings/hadoop-settings.gradle"

dependencies {
      runtime fileTree(dir: "/home/hadoop", include: "*.jar")
      runtime fileTree(dir: "/home/hadoop/lib", include: "*.jar").exclude("*junit*")
}
```
It is important to exclude the `junit` jar file, that ships with hadoop,
otherwise the tests will fail.

## What Hadoop tests does this test execute?

None, yet it relies on the `hadoop-test` jar to be present on the CLASSPATH to
properly function.

The point of this test is to run Cascading tests on top of the new Hadoop build in question. Not to run the
new Hadoop build in question tests.

Cascading is the foundation for a number of open-source and proprietary projects. If the Cascading tests pass,
it is assumed any projects built on Cascading will work, within limits of course.

## How does the compatibility test relate to the Cascading SDK

The [Cascading SDK](http://www.cascading.org/sdk/) is a downloadable archive of many pre-built Cascading
applications.

The compatibility test does not need the SDK to be installed, you can clone the compatibility test
directly from GitHub and run the tests.

If the Cascading compatibility tests do pass on a custom Hadoop build, then its is generally safe to assume
the tools distributed in the SDK will also run without errors.

The compatibility test is included in the SDK as a convenience.

## What do you mean by 'Apache Hadoop', how is that different from the Hadoop I use?

The Apache foundation is the home and legal copyright holder for the Hadoop project. Both the source code and binary
releases are hosted on the Apache Hadoop project page.

Per the Apache License, any organization can compile and redistribute Hadoop for nearly any purpose, without
any significant limitations (see a lawyer for details).

The Hadoop release downloaded from the Apache Hadoop project page is the Apache Hadoop distribution. No other
organization providing a release is entitled to call their Hadoop download Apache, per the rules of the
Apache foundation.

Cascading is compiled, tested, and distributed for the latest stable version of Apache Hadoop. Pre-built binaries of
Cascading will not run on any other distribution unless the Hadoop distribution in question is source and binary
compatible with the Apache Hadoop release.

## Many tests are failing with problems in MiniDFSCluster. What can I do?

This is a permission problem and can be solved by setting the umask in your
current sesssion to `0022` like so:

```bash
    > umask 0022
```

Afterwards you will be able to run the tests as described in the README file:

```bash
    > gradle :vendor-1.0:tests uploadResults -i
```
