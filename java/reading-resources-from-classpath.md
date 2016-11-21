# Reading Resources From Classpath

Resource file present in the classpath, such as `src/main/resources`, can be read as depicted below:

``` java
public InputStream getResource(String fileName) {
  InputStream is = (fileName != null) ? new FileInputStream(fileName) : getClass().getResourceAsStream("/resource.txt");
}

Scanner scanner = new Scanner(getResource(null));
BufferedReader br = new BufferedReader(new InputStreamReader(getResource(null)));
```

**Note:** In this snippet, if user provides resource file name as argument to the `getResource()` method that file will be read otherwise default resource file present in the classpath will be read.
