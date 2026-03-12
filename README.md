# WildFly OpenSSL - Natives


The WildFly OpenSSL project provides OpenSSL bindings for Java. As much as possible they attempt to use existing JSSE API's, so the
SSLContext should be usable as a drop in replacement for applications that are currently using JSSE.

The WildFly OpenSSL project was originally contained within a single project, however the project team started to notice that a lot
of the fixes were contained in the Java code only whilst the release process needed new native binaries for each release. Due to
different architectures and environments needed for the natives the native release process is more involved so this project was
split out so we could release the natives only when really needed.

This code was originally based on the Tomcat Native code, however it has been fairly extensively modified to more closely
align with JSSE and to support dynamic linking.

## Usage

This project is not intended to be directly consumed, artifacts in this repository can change without warning, users should
refer to the WildFly OpenSSL project for usage information.

## Building

The project is structured into a two stage build process to facilitate us being able to aggregate multiple native builds together
and push to Nexus as a single step.

The first step is to trigger the native build according to the environment you are running the build on.

 * `mvn clean install -Dnative-build`

This triggers the `package` module in the project which performs a native build after detecting the environment. The resulting native
library is then copied to the `target` directory at the root of the project.

The second stage is:

 * `mvn install`

This now uses the native libraries in the root `target` directory and performs the remainder of the build to assemble these into
maven artifacts that WildFly OpenSSL can depend upon.

To create a build for multiple architectures / environments you should run the `-Dnative-build` step on each of those environments
and copy the resulting binaries to the `target` directory of your local wildfly-openssl-natives checkout, when you run `mvn install`
these will then all be aggregated into the final build.

 - _If you omit the first invocation with `-Dnative-build`, only parent will be installed_
 - _If you add clean to second invocation, some new tests (since 2.3), which run against not installed library will fail_
 - _If you execute only first invocation, the wildfly-openssl-all will be skipped. See [pom.xml](blob/main/pom.xml) for more details_

### Environments

Presently the development of WildFly OpenSSL including the natives project predominantly occurs on Fedora Linux laptops, the
release process then uses GitHub actions and other CI infrastructure for the actual release.  The following set up information
was prepared for earlier iterations of this project so should be used with caution.

#### Windows

To do the Windows build you need to run the build from a visual studio native tools command prompt. If you want to build
the 32 bit natives you must use the 32 bit prompt (and have JAVA_HOME pointed to a 32 bit JVM), otherwise both the prompt
and the JVM must be 64 bit.

##### Configuring Your Environment

1. Visit the [OpenSSL Wiki](https://wiki.openssl.org/index.php/Binaries) and choose where to download OpenSSL
   from.
    * Install OpenSSL, ideally both 32 and 64 bit versions.
        * When prompted install the executables in the `C:\OpenSSL-32\bin` and `C:\OpenSSL-64\bin` directories
          respectively.
    * _Optional:_ Configure a `OPENSSL_32` and `OPENSSL_64` permanent environment variable.

1. Next ensure you have both a 32 and 64 bit JDK installed.
    * You can download OpenJDK from [Red Hat](https://developers.redhat.com/products/openjdk/download)
    * It seems to be easiest to download the zips.
        * For example unzip to `%USERPROFILE%\apps` and rename the directory to something simple like `java-1.8.0`.

1. Download and install [Visual Studio](https://visualstudio.microsoft.com/downloads/).
    * Make sure you install the native tools for the command prompt too.

##### Building 32-bit Natives

Navigate to the `x86 Native Tools Command Prompt for VS 2019` executable. Generally you can navigate to this through
the start menu. For Visual Studio 2019 Community the location is
`C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Visual Studio 2019\Visual Studio Tools\VC`.

Once the command prompt is open make sure you set your `JAVA_HOME` to the 32-bit JDK. Then update the `INCLUDE`
environment variable to include the OpenSSL headers.

Example:
```
cd %USERPROFILE%\projects\wildfly-openssl
set "JAVA_HOME=%USERPROFILE%\apps\java-1.8.0-32"
set "INCLUDE=%INCLUDE%;%OPENSSL_32%\include"
mvn clean install
```

##### Building 64-bit Natives

Navigate to the `x64 Native Tools Command Prompt for VS 2019` executable. Generally you can navigate to this through
the start menu. For Visual Studio 2019 Community the location is
`C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Visual Studio 2019\Visual Studio Tools\VC`.

Once the command prompt is open make sure you set your `JAVA_HOME` to the 64-bit JDK. Then update the `INCLUDE`
environment variable to include the OpenSSL headers.

Example:
```
cd %USERPROFILE%\projects\wildfly-openssl
set "JAVA_HOME=%USERPROFILE%\apps\java-1.8.0"
set "INCLUDE=%INCLUDE%;%OPENSSL_64%\include"
mvn clean install
```

Contributions
=============

All new features and enhancements should be submitted to the main branch only. Our [contribution guide](https://github.com/wildfly-security/wildfly-openssl-natives/blob/main/CONTRIBUTING.md) will take you through the steps for getting started with the OpenSSL bindings, and how to format and submit your first PR.

For more setup details, check out our [getting started guide](https://wildfly-security.github.io/wildfly-elytron/getting-started-for-developers/) for developers.
