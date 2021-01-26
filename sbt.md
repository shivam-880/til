## Making eclipse aware of fresh dependencies in a sbt project
While eclipsifying a SBT project in order to open the project in Eclipse IDE all the library dependencies specified in the `build.sbt` are downloaded and registered in the generated eclipse `.classpath` file.

However every time you add new library dependencies in `build.sbt` it requires re-eclipsification of the SBT project i.e., you would need to re-run `sbt eclipse` so that freshly downloaded dependencies are also registered in `.classpath` file.

## Debug lagom projects
To debug lagom projects, start `sbt` in debug mode like so, `sbt -jvm-debug 5005` and then add a remote debug configuration in your intellij project before launching the same.

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
