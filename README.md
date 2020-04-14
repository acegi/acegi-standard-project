![Maven Build and Deployment](https://github.com/acegi/acegi-standard-project/workflows/Maven%20Build%20and%20Deployment/badge.svg)
[![Maven Central](https://img.shields.io/maven-central/v/au.com.acegi/acegi-standard-project.svg?maxAge=3600)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22au.com.acegi%22%20AND%20a%3A%22acegi-standard-project%22)
[![License](https://img.shields.io/hexpm/l/plug.svg?maxAge=2592000)](http://www.apache.org/licenses/LICENSE-2.0.txt)

# Acegi Standard Project

Acegi Standard Project is a super POM and resources file bundle that is used by
several open source projects maintained by Acegi Technology. It integrates build
pipeline services (eg GitHub Actions) and plugins for test coverage (JaCoCo,
Codecov), build quality (reproducible builds, dependency updates, dependency
usage, dependency duplication), code quality (PMD, Checkstyle, SpotBugs, source
duplication, XML formatting), licenses (headers, third party usage summaries),
JAR packaging (manifest metadata, assembly), plugin sites, releases (OSS
Sonatype) etc.

## Performing A Release

Project inheriting from Acegi Standard Project are configured with a continuous
integration pipeline that attempts a snapshot artifact deployment on every Git
push. Any person with commit access to these projects can initiate a versioned
release by editing the relevant project POM versions (ie discard the `-SNAPSHOT`
suffix) and creating a tag. Alternately, a project maintainer can locally
execute `mvn -Prelease-tag release:clean release:prepare` to initiate a
versioned release.

## License

This project is licensed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
