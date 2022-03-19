# Maven

Common commands.

For documentation of a command (e.g. `install`, `dependency:tree`) run:

```
mvn help:describe -Dcmd=
```

## Init

```
mvn -B archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app
```

## Tests

Do not run tests:

```
mvn clean install -DskipTests
```

Do not even compile tests:

```
mvn clean install -Dmaven.test.skip=true
```

https://maven.apache.org/surefire/maven-surefire-plugin/examples/skipping-tests.html

## Dependency analysis

Search for a dependency:

```
mvn dependency:tree -Dincludes="*:*name*"
```

See undeclared or unused dependencies:

```
mvn dependency:analyze
```

Using specific version of dependency plugin:
```
mvn org.apache.maven.plugins:maven-dependency-plugin:3.2.0:analyze
```

Write effective POM:

```
mvn help:effective-pom -Doutput=effective.xml
```

Evaluate expression:

```
mvn org.apache.maven.plugins:maven-help-plugin:3.2.0:evaluate \
    -N -q -DforceStdout -f pom.xml -Dexpression=project.artifactId
```

## Upload artifact

```
mvn deploy:deploy-file \
    -Durl=http://nexus/repository/releases/ \
    -DrepositoryId=releases \
    -DpomFile=artifact-1.0.pom \
    -Dfile=artifact-1.0.pom #or .jar
```

## Install pom file

```
mvn -s settings.xml jar:jar install:install
```
