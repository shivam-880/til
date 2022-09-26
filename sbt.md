## Are sbt library dependencies order dependent?
Yes, the order listed is the order used to resolve dependencies.

**Refer**: https://stackoverflow.com/questions/29135602/are-sbt-library-dependencies-order-dependent

## Debug lagom projects
To debug lagom projects, start `sbt` in debug mode like so, `sbt -jvm-debug 5005` and then add a remote debug configuration in your intellij project before launching the same.

## How to have SBT subproject with multiple Scala versions?
Refer this [stackoverflow post](https://stackoverflow.com/questions/27929272/how-to-have-sbt-subproject-with-multiple-scala-versions) and a [github project](https://github.com/iamsmkr/prime-grpc-scala-akka) to see this in action.

## Making eclipse aware of fresh dependencies in a sbt project
While eclipsifying a SBT project in order to open the project in Eclipse IDE all the library dependencies specified in the `build.sbt` are downloaded and registered in the generated eclipse `.classpath` file.

However every time you add new library dependencies in `build.sbt` it requires re-eclipsification of the SBT project i.e., you would need to re-run `sbt eclipse` so that freshly downloaded dependencies are also registered in `.classpath` file.

## Making SBT load locally published jars instead of reaching out to public maven repositories
**Refer**: [Proxy Repositories](https://www.scala-sbt.org/1.x/docs/Proxy-Repositories.html#sbt+Configuration), [sbt Launcher Configuration](https://www.scala-sbt.org/1.x/docs/Launcher-Configuration.html)

1. Create a local file with name as `~/.sbt/repositories` and with content in the below format (modify URL pattern to match yours)
    ```
    [repositories]
      local
      maven-central
      sbt-releases-repo: https://repo.typesafe.com/typesafe/ivy-releases/, [organization]/[module]/(scala_[scalaVersion]/)(sbt_[sbtVersion]/)[revision]/[type]s/[artifact](-[classifier]).[ext]
      sbt-plugins-repo: https://repo.scala-sbt.org/scalasbt/sbt-plugin-releases/, [organization]/[module]/(scala_[scalaVersion]/)(sbt_[sbtVersion]/)[revision]/[type]s/[artifact](-[classifier]).[ext]
      mulesoft-public: https://repository-master.mulesoft.org/nexus/content/repositories/public
    ```
 
2. Start sbt console with the following vm args:
    ```sh
    $ sbt -Dsbt.override.build.repos=true
    ```
  
    **Notes**: 
    - To make this work with Intellij, goto "Preferences/Build, Execute, Deployment/Build Tools/sbt" and add `-Dsbt.override.build.repos=true` add "VM parameters".
    - If the jar is not found in the local repository, it will be fetch from the public repository.

## Publish Artificats to Sonatype OSSRH
**1. Add plugins**
```scala
addSbtPlugin("org.xerial.sbt" % "sbt-sonatype" % "2.3")
addSbtPlugin("com.jsuereth" % "sbt-pgp" % "1.1.1")
```

**2. Configure `build.sbt`**
```scala
ThisBuild / organization := "com.shivamkapoor"
ThisBuild / organizationName := "shivamkapoor"
ThisBuild / organizationHomepage := Some(url("http://shivamkapoor.com/"))

ThisBuild / scmInfo := Some(
  ScmInfo(
    url("https://github.com/iamsmkr/sonatype"),
    "scm:git@github.com:iamsmkr/sonatype.git"
  )
)
ThisBuild / developers := List(
  Developer(
    id = "iamsmkr",
    name = "Shivam Kapoor",
    email = "mail@shivamkapoor.com",
    url = url("http://shivamkapoor.com")
  )
)

ThisBuild / description := "Greet API"
ThisBuild / licenses := Seq("MIT" -> url("https://opensource.org/licenses/MIT"))
ThisBuild / homepage := Some(url("https://github.com/iamsmkr/sonatype"))

// Remove all additional repository other than Maven Central from POM
ThisBuild / pomIncludeRepository := { _ => false }

ThisBuild / publishTo := {
  val nexus = "https://s01.oss.sonatype.org/"
  if (isSnapshot.value) Some("snapshots" at nexus + "content/repositories/snapshots")
  else Some("releases" at nexus + "service/local/staging/deploy/maven2")
}

ThisBuild / publishMavenStyle := true
```

**3. Create `version.sbt`**
```scala
version in ThisBuild := "0.1.0-SNAPSHOT"
```

**4. Create and publish PGP key to key server**
- Check that you have a recent version of GPG 
```
$ gpg --version
```

- Check if agent is gpg-agent running
```
$ ps -ef | grep gpg 
```

- Generate keys pair
```
$ gpg --gen-key
```

- List keys
```
$ gpg --list-keys
$ gpg --list-secret-keys
```

- Publish to key server
```
$ gpg --keyserver keyserver.ubuntu.com --send-keys D22873350AB7C24C3585D9561A189D3BDF75E779
```

- Add `default-key` to your `gpg.conf`
```
$ cat ~/.gnupg/gpg.conf
default-key D22873350AB7C24C3585D9561A189D3BDF75E779
```

- Export secret keys 
```
$ gpg -a --export-secret-keys > ~/.sbt/gpg/secring.asc
```

**5. Create Sonatype credentials file**
```
$ cat ~/.sbt/sonatype_credentials 
realm=Sonatype Nexus Repository Manager
host=s01.oss.sonatype.org
user=[username]
password=[password]
```

**6. Create `sonatype.sbt`**
```
$ cat ~/.sbt/1.0/sonatype.sbt
credentials += Credentials(Path.userHome / ".sbt" / "sonatype_credentials")
```

**7. Publish artifacts**
```
$ sbt publishSigned
```

## Specify dependencies in classpath
As part of `sbt package` command the paths of all the dependencies are compiled into a single file `target/streams/compile/dependencyClasspath/_global/streams/export` which could be referred to as classpath while running the built jar. 
```
$ sbt package
$ java -cp target/scala-2.13/ethereumcc_2.13-0.1.0-SNAPSHOT.jar:$(cat target/streams/compile/dependencyClasspath/_global/streams/export) com.pometry.ethereumcc.Runner
```

## Unmanaged Resources
There are scenarios when the jar we need in our project are not available on mvnrepositories. In that case, we can add those jars to our sbt projects as unmanaged jars. The jar can be stored in the `lib` directory under the root folder. 
```scala
unmanagedJars in Compile ++= Seq(
  baseDirectory.value / "lib/rules-core_2.11-0.1.jar"
)
```


![Unmanaged Resources](https://i.postimg.cc/jjYCCt6j/2021-01-26-20-30.png)


## Using locally maven published jar in a sbt project
In order use a locally published jar using `mvn install` in a sbt project add following resolver to your `built.sbt`.
```
resolvers += Resolver.mavenLocal
```

## Using sbt from behind proxy
In order to use SBT behind proxy server you need to set an *environment variable* named "**SBT_OPTS**" with following value.

``` 
-Dhttps.proxyHost=proxyserver -Dhttp.proxyPort=proxyserverport -Dhttp.proxyUser=username -Dhttp.proxyPassword=password
```

#### Refer
- [How to use sbt from behind proxy?](http://stackoverflow.com/questions/13803459/how-to-use-sbt-from-behind-proxy)
- [Trouble getting SBT working behind proxy](https://www.reddit.com/r/scala/comments/4phuxw/trouble_getting_sbt_working_behind_proxy/)
- [How to set the path and environment variables in Windows](http://www.computerhope.com/issues/ch000549.htm)

## What's SBT_OPTS/.sbtopts?
`JAVA_OPTS`/`SBT_OPTS` are environment variables which when set are passed to java or sbt runtime environment. These could be set either in bash profiles (i.e., across all sessions) or per bash session (i.e., settings are lost with session exit).

You could also define these options per project in `.jvmopts` or `.sbtopts` files under root directory. The precedence order of these alternatives is defined in the `sbt -h`:
```
# jvm options and output control
JAVA_OPTS           environment variable, if unset uses "-Dfile.encoding=UTF-8"
.jvmopts            if this file exists in the current directory, its contents
                    are appended to JAVA_OPTS
SBT_OPTS            environment variable, if unset uses ""
.sbtopts            if this file exists in the current directory, its contents
                    are prepended to the runner args
/etc/sbt/sbtopts    if this file exists, it is prepended to the runner args
```

Alternatives lower in the order take precedence.

You could also check what env variables are being passed to your sbt runtime environment using `sbt -d` command (this sets sbt log level to debug):
```
# Executing command line:
java
-Dfile.encoding=UTF-8
-Xms4G
-Xmx8G
-XX:ReservedCodeCacheSize=512M
-XX:MaxMetaspaceSize=2G
-Dsbt.script=/Users/rentsher/.sdkman/candidates/sbt/current/bin/sbt
-Dscala.ext.dirs=/Users/rentsher/.sbt/1.0/java9-rt-ext-adoptopenjdk_11_0_11
-jar
/Users/rentsher/.sdkman/candidates/sbt/1.6.1/bin/sbt-launch.jar
-debug
```
