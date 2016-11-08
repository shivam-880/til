# Making Eclipse aware of fresh dependencies in a SBT project

While eclipsifying a SBT project in order to open the project in Eclipse IDE all the library dependencies specified in the `build.sbt` are downloaded and registered in the generated eclipse `.classpath` file.

However every time you add new library dependencies in `build.sbt` it requires re-eclipsification of the SBT project i.e., you would need to re-run `sbt eclipse` so that freshly downloaded dependencies are also registered in `.classpath` file.
