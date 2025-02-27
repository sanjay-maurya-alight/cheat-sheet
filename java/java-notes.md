
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
`StringBuffer` is _thread safe_ where `StringBuilder` is not.

# Static
You can access static using following. It doesn't create copies
- Object.staticVariable
- ClassName.staticVariable
- Object.staticMethod()
- ClassName.staticMethod()

> You can't call instance variable in static method as static method don't reference to object. Instead you can pass class object and access instance members using passed object. 

# Constructor
Classname and constructor method are same.

There can be multiple constructor. A constructor without any parameters is called default where as with parameter is called parameterized constructor.

# Naming Convention

> Camel casing

- Class Name, interface: starts with capital
- Method/variable: starts with small
- Constants: all capital letters

# Inheritance
Use keyword `extends`.
Java doesn't support multiple inheritance that is inheritance from more than one class at the same time to avoid ambiguity.

```java
class A{
	public A(){
		System.out.println("Default A");
	}

	public A(int age){
		System.out.println("Paramterized constructor A");
	}
}

class B extends A{
	public B(){
		System.out.println("Default B");
	}
	public B(String name){
		System.out.println("Paramterized constructor B");
	}
}
```
In main class:
```java
B obj1 = new B();
```
Here, default constructor for A and B are called. For below example:
```java
B  obj2  =  new  B("Sanjay");
```
Here default constructor of A and parameterized constructor of B are called. In both the cases, parameterized constructor of A is never called.

Note that, every constructor has first call as `super()` method by default. So our example should look like as:
```java
class A{
	public A(){
		super();
		System.out.println("Default A");
	}

	public A(int age){
		super();
		System.out.println("Paramterized constructor A");
	}
}

class B extends A{
	public B(){
		super();
		System.out.println("Default B");
	}
	public B(String name){
		super();
		System.out.println("Paramterized constructor B");
	}
}
```
The above code run as usual as previously. However we can see we get opportunity to call parameterized constructor A by calling some value in `super()` constructor as below.
```java
class A{
	public A(){
		super();
		System.out.println("Default A");
	}

	public A(int age){
		super();
		System.out.println("Paramterized constructor A");
	}
}

class B extends A{
	public B(){
		super();
		System.out.println("Default B");
	}
	public B(String name){
		super(10); //<- call parameterized constructor of A
		System.out.println("Paramterized constructor B");
	}
}
```
You can play with `super()` and make any constructor of parent.

The keyword `this` refers to current class. You can use `this` to refer properties or methods in the same class avoid local name clashes. By calling `this()`, you are calling current class's constructor.

`Object` is top most class in java and it is extended by any other class implicitly.

# Packages
To organize the codes, we place codes in folders or packages.

Create a folder named `tools` and place a java class file there and mark this file as package `tools` as follows:
```java
package tools;

public class Calc {
	public void showAdd(int a, int b){
		System.out.println(a+b);
	}
}
```
To import this file in another file, we use import statement at the top of Java file and use as usual as shown below.
```java
import  tools.Calc;

//.... in main method
public  static  void  main(String  c[]) {
	Calc  calcu  =  new  Calc();
	calcu.showAdd(3,3);
}
```
Note: the package `java.lang` is imported by default.

To import all classes in a package, we use `*` instead of individual class name.
```java
import  tools.*;
```
Note that this `*` loads the all file in folder. It can't be used for loading folders. Therefore, if a class `Calc` exists in `tools/ > other/ > Calc.java` , we can't use below to load `Calc` class file as below:
```java
import  tools.*;
```
Instead we will write as 
```java
import  tools.other.*;
```
# Access Modifier
By default, a class member gets `default` access modifier meaning the member is public within the *same package* only and is private outside. The other modifiers are: `public`, `protected` and `private`.
