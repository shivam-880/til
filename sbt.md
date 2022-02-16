## Making eclipse aware of fresh dependencies in a sbt project
While eclipsifying a SBT project in order to open the project in Eclipse IDE all the library dependencies specified in the `build.sbt` are downloaded and registered in the generated eclipse `.classpath` file.

However every time you add new library dependencies in `build.sbt` it requires re-eclipsification of the SBT project i.e., you would need to re-run `sbt eclipse` so that freshly downloaded dependencies are also registered in `.classpath` file.

## Debug lagom projects
To debug lagom projects, start `sbt` in debug mode like so, `sbt -jvm-debug 5005` and then add a remote debug configuration in your intellij project before launching the same.

## How to have SBT subproject with multiple Scala versions?
Refer this [stackoverflow post](https://stackoverflow.com/questions/27929272/how-to-have-sbt-subproject-with-multiple-scala-versions) and a [github project](https://github.com/iamsmkr/prime-grpc-scala-akka) to see this in action.

## Using sbt from behind proxy
In order to use SBT behind proxy server you need to set an *environment variable* named "**SBT_OPTS**" with following value.

``` 
-Dhttps.proxyHost=proxyserver -Dhttp.proxyPort=proxyserverport -Dhttp.proxyUser=username -Dhttp.proxyPassword=password
```

#### Refer
- [How to use sbt from behind proxy?](http://stackoverflow.com/questions/13803459/how-to-use-sbt-from-behind-proxy)
- [Trouble getting SBT working behind proxy](https://www.reddit.com/r/scala/comments/4phuxw/trouble_getting_sbt_working_behind_proxy/)
- [How to set the path and environment variables in Windows](http://www.computerhope.com/issues/ch000549.htm)

## Unmanaged Resources
There are scenarios when the jar we need in our project are not available on mvnrepositories. In that case, we can add those jars to our sbt projects as unmanaged jars. The jar can be stored in the `lib` directory under the root folder. 
```scala
unmanagedJars in Compile ++= Seq(
  baseDirectory.value / "lib/rules-core_2.11-0.1.jar"
)
```

[![2021-01-26-20-30.png](https://i.postimg.cc/jjYCCt6j/2021-01-26-20-30.png)](https://postimg.cc/sMK3NFNF)

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

## Using wildcards with scala.sys.process._ in Scala
Refer: https://stackoverflow.com/questions/71132425/using-wildcards-with-scala-sys-process-in-scala
