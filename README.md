[![Build Status](https://travis-ci.org/acegi/xml-format-maven-plugin.svg?branch=master)](https://travis-ci.org/acegi/acegi-standard-project)
[![Dependency Status](https://www.versioneye.com/user/projects/579705114fe918004d2bf7d2/badge.svg?style=flat-square)](https://www.versioneye.com/user/projects/579705114fe918004d2bf7d2)
[![Maven Central](https://img.shields.io/maven-central/v/au.com.acegi/acegi-standard-project.svg?maxAge=3600)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22au.com.acegi%22%20AND%20a%3A%22acegi-standard-project%22)
[![License](https://img.shields.io/hexpm/l/plug.svg?maxAge=2592000)](http://www.apache.org/licenses/LICENSE-2.0.txt)

# Acegi Standard Project

Acegi Standard Project delivers conventions and configurations to simplify the
long-term development of Java projects. It consists of detailed instructions, a
fully-configured Maven parent POM, and file resources referenced by that POM.

Acegi Standard Project can be used by both open and closed source projects.
While end user customisation is very easy, the default configuration offers:

* Maven project practises (configuration, dependency use etc)
* Java practises (style, conventions, bug detection etc)
* Licensing (automatic file header management, bundle inclusion etc)
* XML formatting (automatic)

Optional integration with a number of popular hosted services is also offered:

* GitHub (ie GitHub-aware POM entries, and optional GitHub Pages use)
* Travis CI automation
* Codecov coverage reporting
* Maven Central deployment

### Usage

1. Copy the `test/src/it/acegi-internal` files to your project's root directory
2. If this is your first project, follow the "Organisation Setup" section below
3. Complete the remaining "Project Setup" section below

### Project Setup

#### Git

Change into your new project's directory. It is assumed you already have a
Git repository (if not, simply `git init`).

If using GitHub, create a repository via the GitHub web interface.

#### POM

Edit the `pom.xml`, amending placeholders and removing elements not required.

Most plugins inherit configuration from the `acegi-standard-project` POM. The
`acegi-standard-project` POM takes care to avoid activating any plugin, but it
does provide reasonable configuration defaults.

The main options to customise the build are:

* Remove the `<plugin>` entry if it relates to functionality you don't want
* Edit a `<properties>` element to reflect common configuration customisations
* Provide a new `<plugin>` configuration for more significant changes

Some plugins require configuration files. These are packaged in the
`acegi-standard-resouces` JAR, which is added to relevant plugin classpaths and
also unpacked into `target/acegi-standard-project` during `generate-sources`
phase. If you don't like the configuration of these files, make a copy (eg to
`src/main/config`) and modify the relevant `<property>` element to refer to your
changed file.

It is suggested you add a "hello world" class to `src/main/java/your/package`.
Then run `mvn clean verify` to ensure success. This will also allow you to
progressively add new services and test them in the order shown below.

#### Codecov

If using Codecov, the only step required is to create a project via its web
interface. Use the applicable "import from Git hosting provider" link.

#### Travis

If you are not using Travis, you can remove the `.travis*` files that you may
have copied from `test/src/it/acegi-internal`.

If you are using Travis:

1. Create a project in the Travis web interface (import the project from GitHub)
2. Configure some or all of the following environment variables in the Travis
   web interface: `CENTRAL_USERNAME`, `CENTRAL_PASSWORD`, `GITHUB_PASSWORD`,
   `GPG_KEY`, `GPG_PASSPHRASE` and `GPG_KEY_NAME` (see the "Organisation Setup"
   section for details)
3. Edit `.travis.yml`, removing the GPG line if you're not using Maven Central,
   and removing the Codecov line if you're not using Codecov
4. Add the `.travis.yml` and `.travis-settings.xml` to your Git repo

Run a `git push` and in theory you should see Travis build (and submit coverage
details to Codecov if you left that enabled).

### Organisation Setup

Open source projects (or closed source projects wishing to use these services)
will need keys and various services. The following needs only be completed once
per organisation. You can share the resulting environment variables across all
your projects (eg set them up in Travis, or in a `.bashrc` so they can be
referenced by a machine's local `settings.xml`.

#### GitHub Personal Access Token

GitHub configuration is only necessary for projects wishing to deploy a
Maven-generated site to GitHub Pages. This is generally not used except by
plugin projects.

Any GitHub user with commit rights to the projects' repository can use their
personal access token for the Maven site updates. To change the effective GitHub
personal access token, login to GitHub and create a new personal access token
with two scopes only, `public_repo, user:email`. Then add the newly-created
GitHub personal access token to the `GITHUB_PASSWORD` environment variable.

**Compromised key management:** Login to GitHub, select the personal access
token and click "Delete".

#### Sonatype JIRA and OSSRH

For open source projects intending to deploy release artifacts to Maven Central,
the [OSSRH Guide](http://central.sonatype.org/pages/ossrh-guide.html) offers
instructions on creating a OSS Sonatype JIRA account and claiming a `groupId`.

Once approved, login to [OSS Sonatype](https://oss.sonatype.org/). In the top
right-hand corner, select your username, "Profile", "User Token", "Create User
Token". Add this token information to your `CENTRAL_USERNAME` and
`CENTRAL_PASSWORD` enviornment variables.

**Compromised key management:** Login to OSS Sonatype, select the user token and
click "Reset User Token".

#### GPG Key

Create a GPG key the usual way (`gpg --gen-key`). You may use `unset DISPLAY`
to force text-mode passphrase entry, which can be useful if you do not want any
GPG passphrase on the created key (this can be convenient at times, but is
obviously less secure).

Once created, use `gpg -send-keys KEY_ID` to publish the public key.

You should set the `GPG_PASSPHRASE` and `GPG_KEY_NAME` with the passphrase and
key ID respectively.

If using Travis, the simplest option to transfer the private key to Travis
without requiring complicated per-repository encryption steps is to store a
Base64-encoded (without any line wrapping) version of the ASCII-armoured key in
a secure environment variable called `GPG_KEY`. This one-liner will produce the
required environment variable content so it is compatible with the default
`.travis.yml`:

``` bash
gpg --export-secret-keys -a KEY_ID | base64 -w 0
```

**Compromised key management:** Use `gpg --list-secret-keys` to find the key ID.
Then `gpg --edit-key KEY_ID`, `revuid`, `save`, `quit`. Finally, publish the
newly-revoked status via `gpg --send-keys KEY_ID`.

## Performing A Release

If you used the default Travis CI configuration, a project maintainer can
release by invoking `mvn -Prelease-tag release:clean release:prepare`. This will
update the POMs to a formal release version number, Git tag, and increment the
version number for ongoing development. Travis will perform the actual release.

A developer (or Travis if setup appropriately) can also use
`mvn -Pdeploy deploy` to deploy snapshot versions.

## Snapshots

Travis CI automatically publishes snapshot releases to the
[OSS Sonatype Snapshots Repository](https://oss.sonatype.org/content/repositories/snapshots/au/com/acegi/acegi-standard-project).

## License

This project is licensed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
