# Running Maven Applications

## Without arguments

```
$ mvn exec:java -D exec.mainClass=com.codingkapoor.smfdependencygraph.Builder
```

## With arguments

```
$ mvn exec:java -D exec.mainClass=com.codingkapoor.smfdependencygraph.Builder -Dexec.args=input.txt
```
