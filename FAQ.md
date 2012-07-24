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

Send an email to support@cascading.org.

## Is a Maven repository required?

No.

This is the simplest way to have Gradle find your Hadoop build. But you can update the test CLASSPATH to
include jars from a local directory, or download a tar/zip file and unpack it into the CLASSPATH.

See the Gradle documentation for more details.

## What Hadoop tests does this test execute?

None.

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