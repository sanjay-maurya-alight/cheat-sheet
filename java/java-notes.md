
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

## Dynamic Method Dispatch
You can re-assign same class object with different class types or class objects as long as they are related by inheritance. For example:
```java
class A extends Object{
	public void show(){
		System.out.println("AShow");
	}
}

class B extends A{
	public void show(){
		System.out.println("BShow");
	}
}
public class C {
	public void show(){
		System.out.println("CShow");
	}
}
```
Note here class C is not related to A and B. In main method we can write as:
```java
A obj = new B();
obj.show(); //BShow

obj = new A();
obj.show(); // AShow
obj = new B();
obj.show(); //BShow

obj = new C(); //error
obj.show();
```
We started out creating object of parent type and assigned child class instance. Then on `obj` variable is reused in other assignment in classes which are related. The class C is not related and hence throws error when instance is assigned to same object `obj`.

## Final keyword
The keyword `final` is used for class name, methods and properties. It is used for creating constant properties when applied to properties.
```java
final int age = 15; //creates a constant
age = 20; //error as constant
```
When a class is made `final`, that class then can't be sub-classed.
```java
final class A{}
class B extends A{} // error
```
When a method is `final`, that method can not be overridden in child class.
```java
class A{
	public final void show(){
		System.out.println("From class A");
	}
}
class B extends A{
	public void show(){ // error, can be overridden in child class
		System.out.println("From class A");
	}
}
```
- `final` property: create constant property
- `final` method: - stop overriding
- `final` class: stop sub class

# Typecasting
```java
datatype variable = (dataType2) value
```
We can typecast using user defined object. We have two cases either object will be down casted to get features of child class or up casted to get features of parent class.


# Wrapper Class
Classes around primitive data types to get additional features which are not there with primitive types such as parsing string to number.

# Abstract Class

An abstract class can't be instantiate and forces to implement abstract methods if any in child class. The child will  become abstract if even child class is not implementing the abstract methods. The class must be abstract if one of the method is abstract where as an abstract class need not contain abstract method.

An abstract method is method declaration without curly braces.
```java
abstract public void drive();
```
# Inner Class
A class within another class.

```java
class A{
	
	private int val = 10;

	public void showValue(){
		System.out.println(this.val);
	}

	class B{
		public void showNewValue(){
			System.out.println("New Value");
		}	
	}
}

//in main method
A a = new A();
A.B b = a.new B();
a.showValue();
b.showNewValue();
```
Another way to define anonymous inner class as:
```java
A a = new A(){
	public void showValue(){
		System.out.println("add this");
	}
};
```
Here instead of extending, we are overriding the method during runtime.

For abstract class, we can instantiate using anonymous inner class by providing definition of abstract methods in the curly braces.
```java
abstract class A{
	abstract void showDetails();
}

A a = new A(){
	public showDetails(){
		System.out.println("Showing from abstract class");
	}
};
//in main method
a.showDetails();
```
# Interfaces
All methods are by default `abstract public`, Interface is not a class and hence can't be instantiated but can be used for defining data type of variable. If we are creating class for just abstract methods, we should refer `interface` instead.

We use `implements` keyword for implementing interface. An interface can extend another interface and a class can implement multiple interfaces.

You have to define all abstract methods in `interface` or mark your class as `abstract`.
```java
interface C {
	void showA();
	void showB();
}

abstract public class A implements C {}
```
If you declare properties within the interface, by default they will become `final` and `static`. And thus you can refer them just using class name where interface is implemented. 

Note
>class can extend another class \
> class can implement an interface \
> interface can extend another interface

## Enum
List of objects that can be reused in the code:
```java
enum Status {Running, Pending, Stopped};
```
Here the values within the curly braces are objects. They are instances of user defined `enum` class Status. So the `enum` is storing its own objects.

You can define `enum` as data type to store listed values only.

```java
Status codes = Status.Pending
```
Moreover:
```java
Status  codes[] =  Status.values(); //get all values
System.out.println(codes[0]); //get first value in the array
```
We can use the same in `if` and `switch` conditions:
```java
enum Status {Running, Pending, Stopped};
class Server{
	Status getServerStatus(){
		return Status.Pending;
	}	
}

//main method:
Server server = new Server();
Status s = server.getServerStatus(); //some user defined function
if(s == Status.Pending){
	System.out.println("Process not started!");
}else if(s == Status.Running){
	System.out.println("Process still running!");
}else if(s == Status.Stopped){
	System.out.println("Process has stopped!");
}
```
Same `if` condition can be written with smaller codes as:
```java
switch (s) {
	case Pending:
		System.out.println("Process not started!");
		break;
	case Running:
	System.out.println("Process still running!");
		break;
	case Stopped:
		System.out.println("Process has stopped!");
		break;
	default:
		break;
}
```
Note that type `Status` is referred itself. We are not using value `Pending` as `Status.Pending` for example in switch cases.

`Enum` is like class but can't be extended. Let's check on enum like class.

Following declaration is enum containing objects of its own with value passed to each object.

You can guess what would be called when we pass parameter to class constructor while object creation. It is the constructor.
```java
enum  Laptop{
	MacBook, Dell, HP, Surface;
}
```
Here if we pass value to those objects, the constructor needs to incorporate the value passed to it otherwise it is an error.
```java
enum Laptop{
	MacBook(100), Dell(200), HP(300), Surface(400)

	Integer cost=0;
	Laptop(Integer cost){
		this.cost = cost;
	}
}
```
In main method, we can write as:
```java
Laptop lap = Laptop.HP;
System.out.println(lap + " - " + lap.cost);
```
What if one object doesn't need value, we can then define additionally default constructor in the enum as:
```java
enum Laptop{
	MacBook(100), Dell, HP(300), Surface(400);

	Integer cost=0;
	Laptop(Integer cost){
		this.cost = cost;
	}
	private Laptop() {
		this.cost = 500;
	}
}
```
So now, the if no value passed to object, the price property would be set to 500. So in our main method we can write:
```java
for (Laptop lap : Laptop.values()) {
	System.out.println(lap + " - " + lap.cost);	
}
```
The outcome would be:
```console
MacBook - 100
Dell - 500
HP - 300
Surface - 400
```

# Lambda Expression
- Functional Interface: has one method with optionally `@FunctionInterface` applied to force to have one method
- Normal Interface: multiple methods
- Marker Interface: no methods

The lambda expression let us define method of the function interface during runtime. For example
```java
@FunctionalInterface
public interface A {
	void showing(boolean isShowing);
}
```
We can define the method by declaring a class. The lambda expression is another way which doesn't require you to define class if there is one method implementation.
```java
class Hello {
	public static void main(final String c[]) {
		A obj = isShowing -> System.out.println("Showing " + isShowing);
		obj.showing(true);
	}
}
```
Output:
```
Showing true
```
One can use braces if there are multiple statements for the expression.

Notice that the during the object creation, we didn't specify the type of variable for the method.
Below is the example of Lambda expression with return value. For single statement, `return` is not required.
```java
@FunctionalInterface
public interface A {
	int add(int i, int j);
}
class Hello {
	public static void main(final String c[]) {
		A obj = (i,j) -> i + j;
		System.out.println(obj.add(5,4));
	}
}
```
Output:
```java
9
```
# Exception
```java
try{ //try() <- for resources
	//some code to evaluate
}catch(Exception e){
	 // optional block, can be removed if not required
	//some information when exception arises
}finally{
	//always gets execited
}
```
The `Exception` is parent class of many other exception.
```java
class Hello {
	public static void main(final String c[]) {
		try {
			final double data = 4/0;
			System.out.println(data);
		} catch (Exception e) {
			System.out.println("Something went wrong. " + e.getMessage());
		}
	}
}
```
You may chain multiple exception in one statement. The `Exception` should be last.

Note that try can accept declaration which creates resources and those resources can be closed automatically if the resource creator class has auto closable implemented.
```java
public  static  void  main(final  String  c[]) {

try (BufferedReader  br  =  new  BufferedReader(new  InputStreamReader(System.in))) {

} catch (IOException  e) { }

}
```

## Throwing an exception

```java
throw new ArithmeticException();
```
You can optionally pass message into the exception class.

## Custom Exception
For example:
```java
class AException extends Exception{
	AException(String message){
		super(message);
	}
}
class Hello {
	public static void main(final String c[]) {
		try {
			throw new AException("My Favorite exception");
		} catch (AException e) {
			System.out.println(e); //AException: My Favorite exception
		}
	}
}
```
## Exception bubbling Up
In PHP we have exception bubble up which means an exception occurred somewhere is bubbled up to the calling method or property of the object. However in Java we don't have bubbling enabled by default and therefore whenever exception occurs, it has to be handled there otherwise code will not go ahead. When class method extends `throws Exception`, it means any exception occurred in that class should be bubbled up in case not handled in that class.

For example:
```java
class A {
	public void add(){
		try {
			double a = 4/0;	
			System.out.println("Answer: " + a);
		} catch (Exception e) {
			System.out.println("An error occured");
		}
	}
}

class Hello {
	public static void main(final String c[]) {
		A obj = new A();
		obj.add();
	}
}
```
Here you would note the class A is handling the exception and there calling method of class A doesn't know what happened if code stopped there. For example
```java
class A{
	public void add() throws Exception{
		double a = 4/0;	
		System.out.println("Answer: " + a);
	}
}

class Hello {
	public static void main(final String c[]) {
		A obj = new A();
		try {
			obj.add();	
		} catch (Exception e) {
			System.out.println("From Main: " + e);
		}
	}
}
```
Note here the `try/catch` block is missing in class A and exception occurring from class A is checked in main method. Please note that you may still write the `try/catch` block and the local `try/catch` will be executed instead in that case.

## User Inputs
Similar to `System.out`, we have `System.in` for taking input. The `System.in` has methods. One of them is `read()` method which accepts one character and returns ASCII value of passed input.
```java
class Hello {
	public static void main(final String c[]) {
		System.out.print("Enter a value: ");
		try {
			System.in.read();	
		} catch (Exception e) {
			System.out.println("Unable to get input.12");
		}
	}
}
```
> Note that `System.in` is input stream from `InputStream` class and can passed to method call which require input data from user from console.

However we want to read entire content. Java provides two classes for the same `BufferedReader` and `Scanner`. They both belongs to `java.io` package. The `BufferedReader` provides `readLine()` method for reading entire line from the input stream.
```java
BufferedReader  bf  =  new  BufferedReader();
```
The above statement is not a complete as `BufferedReader` needs `Reader` class as input. For input stream as reader we use `InputStreamReader` class. That is
```java
InputStreamReader  in  =  new  InputStreamReader();
BufferedReader  bf  =  new  BufferedReader(in);
```
Here again the `InputStreamReader` needs input stream which is `System.in`. The other streams for example are: File, Print, etc,.
```java
InputStreamReader  in  =  new  InputStreamReader(System.in);
BufferedReader  bf  =  new  BufferedReader(in);
```
Below is complete code:
```java
class Hello {
	public static void main(final String c[]) {
		System.out.print("Enter a value: ");
		
		InputStreamReader in = new InputStreamReader(System.in);
		BufferedReader bf = new BufferedReader(in);
		
		try {
			System.out.println(bf.readLine());
			bf.close();	//close the resource point
		} catch (Exception e) {
			System.out.println("Unable to close the stream input");
		}
	}
}
```
The `Scanner` code simplifies above to below:
```java
class Hello {
	public static void main(final String c[]) {
		System.out.print("Enter a value: ");
		Scanner sc = new Scanner(System.in);
		System.out.println(sc.next());
		sc.close();
	}
}
```

## Threads
Small unit of process that can be completed independently of other task. They share the resources and therefore can run the at the same time. Therefore threads allows multi-tasking.

Refer thread related videos back from the list.

# Collection API

`Collection` and `Map` are interfaces for creating any user defined collection API. However, we should first check the in built collections. The collections are part of `util` package. 
- All collections of interface `Collection`: `java.util.Collection` 
- All collections of interface `Map`: `java.util.Map`

Ref:  [geeksforgeeks](https://www.geeksforgeeks.org/collections-in-java-2/)

Following are interfaces on collection interface. They have implemented own classes. Partial examples shown below. Refer link for complete list.
- `java.util.Collection` interfaces
	- `List` interface: ****ordered collections of elements****
		- `ArrayList` class
		- `LinkedList` class
		- `Vector` class
		- `Stack` class
	- `Queue` interface: ****ordered; stores and processes the data in FIFO(First In First Out)****
		- `PriorityQueue` class
		- `Deque` class
	- `Deque` interface: ****ordered;double ended queue - FIFO and LIFO both****
	- `Set` interface: ****Unordered and non duplicate collection****
	- `SortedSet` interface: ****Ordered and non duplicate collection****
- `java.util.Map` interfaces
	- `Map` interface: ****Key/value pair****
		- `HashMap` class: ***unordered keys***
		- `SortedMap` class: ***keys in a sorted order***
		- `NavigableMap` class: ***Uses key range for navigation***

The collection API works with object by default and not primitive type. To work with primitive in collection API, we use generics to specify class corresponding to data type. For example:
```java
Collection num = new ArrayList();
num.add(2);
num.add(3);
num.add(8);
```
Here we are storing integers instead of objects. So in this case we need to specify type if we are not storing objects.
```java
Collection<Integer> num = new ArrayList<Integer>();
num.add(2);
num.add(3);
num.add(8);
```
Now way you can loop over collection as following:
```java
for(Integer n : num){
	System.out.println(n);
}
```
Note `Integer` is a integer class for handling integer and is part of `java.lang`. Note that `Collection` is very generic interface and therefore you might not get all unique features or methods to work on data.

## Comparable
If you implement a class with `Comparable`, you can directly use your object and compare in in the code. For example:
```java
class A implements Comparable<A> {

	private int age=0;
	private String name="";

	A(String name, int age){
		this.name = name;
		this.age = age;
	}
	public int compareTo(A human) {
		return this.age >= human.age ? 1 : -1;
	}
	public String toString(){
		return "Name: " + this.name + " - Age: " + this.age;
	}
}

class Hello {
	public static void main(final String c[]) {

		List<A> person = new ArrayList<A>();

		person.add(new A("Aa", 21));
		person.add(new A("Ab", 1));
		person.add(new A("Ac", 10));
		person.add(new A("Ad", 43));

		Collections.sort(person);

		System.out.println(person);
		//[Name: Ab - Age: 1, Name: Ac - Age: 10, Name: Aa - Age: 21, Name: Ad - Age: 43]
	}
}
```


