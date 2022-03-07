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
	    -Dcom.sun.management.jmxremote.port=9800 \
	    -Dcom.sun.management.jmxremote.rmi.port=9800 \
	    -jar jmx-tester-1.0.0.jar
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
