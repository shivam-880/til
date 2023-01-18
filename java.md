## Calculate time difference (millisecond precision)
[Try it on replit](https://replit.com/@iamsmkr/Time-Difference-Millisecond-Precision?v=1)

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

class Main {  
  public static void main(String args[]) throws ParseException { 
    String time1 = "15:23:51.981";
    String time2 = "15:39:42.847";
    
    SimpleDateFormat format = new SimpleDateFormat("HH:mm:ss.SSS");
    
    Date date1 = format.parse(time1);
    Date date2 = format.parse(time2);
    long difference = date2.getTime() - date1.getTime();
    System.out.println(difference);
  } 
}
```

## Create heap dump
```
$ jmap -dump:live,file=dump1.hprof <pid>
```

## Create thread dump
```
$ jstack <pid> > /tmp/threaddump.txt
```

**Note**: Use [fastthread.io](https://fastthread.io) to open thread dump file.

## Reading properties from properties file

Basically if our properties are not locale specific using `ResourceBundle` would be semantically incorrect. We should instead use `Properties` class in standard Java library.

``` java
public class PropertyReader {

	private Properties prop = new Properties();

	public PropertyReader(String fileName) throws IOException {

		try (InputStream is = getClass().getClassLoader().getResourceAsStream(fileName)) {

			if (is == null)
				throw new FileNotFoundException(fileName);

			prop.load(is);
		} 
	}

	public String getProperty(String key) {
		return prop.getProperty(key);
	}

}
```

#### Refer
- [Properties vs Resource Bundle](http://stackoverflow.com/questions/6978415/properties-vs-resource-bundle)
- [Why not use ResourceBundle instead of Properties?](http://stackoverflow.com/questions/14883000/why-not-use-resourcebundle-instead-of-properties)


## Reading resources from classpath

Resource file present in the classpath, such as `src/main/resources`, can be read as depicted below:

``` java
public InputStream getResource(String fileName) {
  InputStream is = (fileName != null) ? new FileInputStream(fileName) : getClass().getResourceAsStream("/resource.txt");
}

Scanner scanner = new Scanner(getResource(null));
BufferedReader br = new BufferedReader(new InputStreamReader(getResource(null)));
```

**Note:** In this snippet, if user provides resource file name as argument to the `getResource()` method that file will be read otherwise default resource file present in the classpath will be read.

## Remote JMX Monitoring over SSH
1. To JMX monitor an application we need to pass couple of properties while running the application like so:
	```
	java \
	    -Djava.rmi.server.hostname=localhost \
	    -Dcom.sun.management.jmxremote=true \
	    -Dcom.sun.management.jmxremote.ssl=false \
	    -Dcom.sun.management.jmxremote.authenticate=false \
	    -Dcom.sun.management.jmxremote.port=9900 \
	    -Dcom.sun.management.jmxremote.rmi.port=9900 \
	    -jar jmx-tester-1.0.0.jar
	```
	
	**.jvmopts**
	```
	-J-Xms12G
	-J-Xmx32G
	-J-XX:ReservedCodeCacheSize=512M
	-J-XX:MaxMetaspaceSize=2G
	-J-Djava.rmi.server.hostname=localhost
	-J-Dcom.sun.management.jmxremote=true
	-J-Dcom.sun.management.jmxremote.ssl=false
	-J-Dcom.sun.management.jmxremote.authenticate=false
	-J-Dcom.sun.management.jmxremote.port=9900
	-J-Dcom.sun.management.jmxremote.rmi.port=9900
	```
	
	**.sbtopts**
	```
	-Xms12G
	-Xmx32G
	-XX:ReservedCodeCacheSize=512M
	-XX:MaxMetaspaceSize=2G
	-Djava.rmi.server.hostname=localhost
	-Dcom.sun.management.jmxremote=true
	-Dcom.sun.management.jmxremote.ssl=false
	-Dcom.sun.management.jmxremote.authenticate=false
	-Dcom.sun.management.jmxremote.port=9900
	-Dcom.sun.management.jmxremote.rmi.port=9900
	```

2. Create standard SSH tunnel
	```
	$ ssh -i aws_ethereum.pem -f -N -L 9800:localhost:9800 ubuntu@ec2-3-82-189-52.compute-1.amazonaws.com
	```

	**Note**: `jmxremote.port` can be equal to `jmxremote.rmi.port` so you can use only one port to create SSH tunnel.

3. Add local JMX connection

	[![Screenshot-2022-03-07-at-4-33-22-PM.png](https://i.postimg.cc/K8SyPNbJ/Screenshot-2022-03-07-at-4-33-22-PM.png)](https://postimg.cc/fJKFZYJ9)
	

**Refer**
- [jmx-tester](https://github.com/cluther/jmx-tester)
- [How to monitor a remote JVM over ssh using JVisualVM](http://issamben.com/how-to-monitor-remote-jvm-over-ssh/)
- [VisualVM on a Remote EC2 Instance Over an SSH Tunnel](http://msnider.github.io/blog/2014/01/07/visualvm-on-a-remote-ec2-instance-over-an-ssh-tunnel/)
- [VisualVM with AWS EC2](https://www.youtube.com/watch?v=e8gMt0wixls&ab_channel=YaakovDiament)
- [VisualVM: Monitoring Remote JVM Over SSH (JMX Or Not)](https://dzone.com/articles/visualvm-monitoring-remote-jvm)

## Running java applications

#### Without arguments
```
$ java -cp target/kaeru-jump-1.0-SNAPSHOT.jar com.codingkapoor.kaerujump.KaeruJumpAlgo
```

#### With arguments
```
$ java -cp target/kaeru-jump-1.0-SNAPSHOT.jar com.codingkapoor.kaerujump.KaeruJumpAlgo ".\input1.txt"
```

#### As background process
```bash
$ nohup bash -c "(java -Xms32G -Xmx64G -XX:ReservedCodeCacheSize=512M -XX:MaxMetaspaceSize=100M -XX:+UseShenandoahGC -XX:ShenandoahGCHeuristics=compact -verbose:gc \
    -cp target/scala-2.13/trm_2.13-0.1.0-SNAPSHOT.jar:$(cat target/streams/compile/dependencyClasspath/_global/streams/export) \
    -Djava.rmi.server.hostname=localhost \
    -Dcom.sun.management.jmxremote=true \
    -Dcom.sun.management.jmxremote.ssl=false \
    -Dcom.sun.management.jmxremote.authenticate=false \
    -Dcom.sun.management.jmxremote.port=9800 \
    -Dcom.sun.management.jmxremote.rmi.port=9800 \
    com.raphtory.trm.BatchLoader)" &; tail -f nohup.out
```
