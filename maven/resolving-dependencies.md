# Resolving Dependencies

`mvn compile` will resolve all the compile time dependencies.<br/>
`mvn test` will resolve all the compile time as well as test time dependencies.

`mvn package` or `mvn install` will resolve all the dependencies.

However, if we want to only download dependencies without doing anything else, we can run `mvn dependency:resolve`.
