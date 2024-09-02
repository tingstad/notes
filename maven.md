# Maven

Some useful commands.

For documentation of a command (e.g. `install`, `dependency:tree`) run:

```shell
mvn help:describe -Dcmd=
```

## Init

```shell
mvn -B archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app
```

## Tests

Do not run tests:

```shell
mvn clean install -DskipTests
```

Do not even compile tests ([doc](https://maven.apache.org/surefire/maven-surefire-plugin/examples/skipping-tests.html
)):

```shell
mvn clean install -Dmaven.test.skip=true
```

## Release

```shell
mvn release:clean && mvn release:prepare && mvn release:perform
```

## Set version
```shell
mvn versions:set -DgenerateBackupPoms=false -DnewVersion="$newVersion"
```

## Dependency analysis

Search for a dependency:

```shell
mvn dependency:tree -Dincludes="*:*name*"
```

See undeclared or unused dependencies:

```shell
mvn dependency:analyze
```

Using specific version of dependency plugin:
```shell
mvn org.apache.maven.plugins:maven-dependency-plugin:3.3.0:analyze
```

Write effective POM:

```shell
mvn help:effective-pom -Doutput=effective.xml
```

Evaluate expression:

```shell
mvn org.apache.maven.plugins:maven-help-plugin:3.2.0:evaluate \
    -N -q -DforceStdout -f pom.xml -Dexpression=project.artifactId
```

Multi-module graph:

```shell
echo "" > raw.dot
mvn dependency:tree -DoutputType=dot -DoutputFile="$PWD"/raw.dot -DappendOutput
grep ' ->' raw.dot | sort | uniq | ( echo "digraph mygraph {"; cat; echo "}" ) > graph.dot
```

## Upload artifact

```shell
mvn deploy:deploy-file \
    -Durl=http://nexus/repository/releases/ \
    -DrepositoryId=releases \
    -DpomFile=artifact-1.0.pom \
    -Dfile=artifact-1.0.pom #or .jar
```

## Install artifact to local repository

```shell
mvn org.apache.maven.plugins:maven-dependency-plugin:3.8.0:get -Dartifact=group:name:1.0
```

## Install pom file

(Without compiling)
```shell
mvn -s settings.xml jar:jar install:install
```
