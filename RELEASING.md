# Release Process

The release process for WildFly OpenSSL Natives is inverted compared to the rest of the `wildfly-security` projects.
For most projects we tag locally, upload to Maven / Nexus and only push our tags once the release is complete, for
WildFly OpenSSL Natives we tag and push first and then publish the release.

The reason for this inversion is when we push the tag to GitHub, actions will be run to create the native libraries for:

 * Linux
 * MacOS
 * Windows

We will also take the tag and use CI infrastructure to create an s390 build.

The README of this project contains more information about the hierarchy of the build but once we have the natives from
each of these builds we copy them to the `target` directory at the root of the project and can then procees to call
`mvn deploy` to publish the release to Nexus.

The basic process is:

_Before running the release you should run `mvn clean` to ensure the `target` directory is empty._

 * Perform the tag and push it to github as normal.

Gather the native libraries built by the GitHub Action and place them in the `target` directory.
Use ci.wildfly.org to run the s390 build and copy the library to `target` with the others.

 * Run `mvn deploy`, this will publish the release to the `wildfly-staging` repo in Nexus.

As per the other projects deployed to Nexus, at this stage verify there are no errors in the staging repository that could
indicate the release will fail to synchronise to Maven Central.

 * Assuming everything is Ok now run `mvn nxrm3:staging-move`, this will move the release from `wildfly-staging` to `wildfly-security`.

The tags were pushed to GitHub at the start of the process so the only remaining work is to clean up Jira and ensure the version in the
project's poms is set for the next round of development.

