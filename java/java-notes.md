
# Setting Up VS Code

- Download JDK zip file and extract in `C:/` drive or `/opt`.
- store java bin in JAVA_HOME evn: ` export JAVA_HOME='C:\project\jdk-17'`

JVM is platform dependent but the Java application you build is independent of OS by using same JVM. Note that JVM needs byte code and therefore we need to compile the application for byte code conversion using `javac`. The `javac` expects file to be compiled to have `main` method which will be called `javac`. `jre` is the runtime environment.

> Java is `WORA`, `Write Once, Run Anywhere`.

# Data Type

## Primitive
- Integer: `int`, `short`, `long`, `byte`
	- suffix `l` to value for long type
- Float: `float`, `double`(default)
	- suffix `f` to value for float type
- Character: `char` (single quotes)
- Boolean: values: `true`, `false`, Type: `boolean`

## Array

### Jagged Array
2D array with columns are unknown. The rows needs to be fix but columns can be any number of element.

```java
int num[][] = new int[3][];
```
Later you need to set the columns as
```java
int num[0] = new int[2];
int num[1] = new int[5];
int num[2] = new int[21];
```
### Enhanced Loop for arrays
```java
int  num[] =  new  int[]{1,2,3};
for(int  n  :  num){
	System.out.println(n); //1 2 3
}
```
## Strings
Double Quote. It is not primitive but a class.

```java
String name = "Sanjay Maurya";
```
Using object notation as String is class.
```java
String name = new String("Sanjay Maurya");
```
String in Java is immutable. Java maintains string constant pools to reuse the same string literal without need of creating new memory location if literal is same. If you re-assign the string to same variable with some modification, the previous string constant is not used will be marked for garbage collection.

To make string mutable, we use string buffer or string builder.

```java
StringBuffer name = new StringBuffer("Sanjay");
//name.capacity() //characters that can strong
name.append(" Maurya");
System.out.println(name); //Sanjay Maurya
String finalName = name.toString(); //convert back to immutable.
System.out.println(finalName ); //Sanjay Maurya
```

