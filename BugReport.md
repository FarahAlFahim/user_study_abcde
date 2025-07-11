## Title: Unable to open bolt page of storm ui

## Description
There is a stacktrace corresponding to this in nimbus.log showing `IndexOutOfBound` error:

## Stack Trace:
```
java.lang.ArrayIndexOutOfBoundsException: -2  
    at java.util.ArrayList.elementData(ArrayList.java:418),  
    at java.util.ArrayList.get(ArrayList.java:431),  
    at org.apache.storm.daemon.nimbus.Nimbus.getComponentPageInfo(Nimbus.java:3606),  
    at org.apache.storm.generated.Nimbus$Processor$getComponentPageInfo.getResult(Nimbus.java:4097),  
    at org.apache.storm.generated.Nimbus$Processor$getComponentPageInfo.getResult(Nimbus.java:4081)  
```

The problem is that we expect the index to be positive, but since it is a mod of hashcode it can be negative.  
```
int taskIndex = TupleUtils.listHashCode(Arrays.asList(componentId)) % tasks.size();  
int taskId = tasks.get(taskIndex);
```