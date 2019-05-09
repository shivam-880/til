# Reload your configuration files reactively 

## 1. Poll config file for changes
Conventional way would be to poll checksum of the configuation file every configured period of time, check if the checksum has changed and based on the results reload the file.

## 2. Using `better-files-akka` library in Scala

https://github.com/pathikrit/better-files

``` scala
import akka.actor.{ActorRef, ActorSystem}
import better.files.File
import better.files.FileWatcher._
import java.nio.file.{StandardWatchEventKinds => EventType}

trait ConfWatcher {
  def confDir: String
  implicit def actorSystem: ActorSystem

  private val confPath = confDir + "application.conf"
  private val appConfFile = File(confPath)
  private var appConfLastModified = appConfFile.lastModifiedTime

  val watcher: ActorRef = appConfFile.newWatcher(recursive = false)

  watcher ! on(EventType.ENTRY_MODIFY) { file =>
    if (appConfLastModified.compareTo(file.lastModifiedTime) < 0) {
      // TODO
      appConfLastModified = file.lastModifiedTime
    }
  }

}
```

**Note:** This snippet of code also address multiple events [issue](https://github.com/pathikrit/better-files/issues/313).
