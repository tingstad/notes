# Maven

Common commands.

## Tests

Do not run:

```
mvn clean install -DskipTests
```

Do not compile:

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

specific version:
```
mvn org.apache.maven.plugins:maven-dependency-plugin:3.2.0:analyze
```

Write effective POM:

```
mvn help:effective-pom -Doutput=effective.xml
```
