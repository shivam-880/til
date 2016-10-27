# Using SBT from behind proxy

In order to use SBT behind proxy server you need to set an *environment variable* named "**SBT_OPTS**" with following value.

``` 
-Dhttps.proxyHost=proxyserver -Dhttp.proxyPort=proxyserverport -Dhttp.proxyUser=username -Dhttp.proxyPassword=password
```

## Refer
- [How to use sbt from behind proxy?](http://stackoverflow.com/questions/13803459/how-to-use-sbt-from-behind-proxy)
- [Trouble getting SBT working behind proxy](https://www.reddit.com/r/scala/comments/4phuxw/trouble_getting_sbt_working_behind_proxy/)
- [How to set the path and environment variables in Windows](http://www.computerhope.com/issues/ch000549.htm)
