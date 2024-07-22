# Lecture 1
## Overview
61B features:
- Write code that runs efficiently
	- Good algorithms
	- Good data structures
- Writing code efficiently
	- Designing, building, testing, and debugging large programs
	- Use of programming tools
		- git, IntelliJ, JUnit
	- Java
- The most popular topics for job interview questions in software engerning
	- Hash tables, binary search, ...
- Some really cool math
	- Asymptotic analysis
	- Resizing arrays...

## Phases
- Phase 1 (weeks 1-4): intro to Java and Data Structures
	- All coding work is solo
	- Moves VERY Fast
- Phase 2 (weeks 5-10): Data Structures
	- All coding work is solo
	- Moves moderately
- Phase 3 (week 12-14): Algorithms
	- Coding works is entriely dedicated to final project, done in pairs
	- Slower pace

## Java Basis
- Functions must be decleared as a part of a class in Java. A function that is part of a class is called a "method". So in Java, all functions are methods
- to define a function in Java, we use `public static`. We will see alternate ways of defining functions later
- All parameters of a function must have a decleared type. Functions in Java return only one value.
- Java is an object oriented langurage
	- Every Java file must conatin a class declaration
	- All code lives inside a class, even helper functions, global constants, etc.
	- To run a Java program, you typically define a main method using `public static void main(string[] args)`
- Java is statically typed
	- All variables, parameters, and methods must have a decleared type
	- That type can never change
	- Expression also have a type
	- The compiler checks that all the types in your program are compatible before the program ever runs

# Lecture 2
*basic java class tutorial*

# Lecture 3
## Walrus mystery (*reference or copy*)
```
Walrus a = new Walrus(1000,8.3);
Walrus b;
b = a;
b.weight = 5;
System.out.println(a);
System.out.println(b);
```
```
weight: 5, tusk size: 8.30
weight: 5, tusk size: 8.30
```


```
int x = 5;
int y;
y = x;
x = 2;
System.out.println("x is" + x);
System.out.println("y is" + y);
```
```
x is 2
y is 5
```
**WHY?**

- When you declear a variable...
	- Your conputer sets aside exactly enough bits to hold a thing of that type (*e.g. 32 bits for int, 64 bits for double*)
	- Java createsman internal table that maps each variable name to a location
	- Java does not write anything into the reserved boxes
- Given variables `y` and `x`:
	- `y = x`  **copies** all the  bits from `x` to `y`
- There are 8 primitive types in Java
	- `byte`, `short`, `int`, `long`, `float`, `double`, `boolean`, `char`
- Everything else, including **arrays**, is a reference type
- When we instanciate an Object...
	- Java first allocates a box of bits for each instance variable of the class and fills them with a default value (*e.g. `0`, `null`*)
	- The constructor then usually fills every such box with some other value
	- and can think of `new` as returing the address of the newly created object.
	- Java allocates exactly a box of size 64 bits, no matter what type of object.
		- `null`(all zero)
		- The 64 bit "address" of a specific instace of that class (returned by `new`)
- That is to say that the variables of a reference type are the **Reference** of the instance **Object**

## The golden rule
- `b = a` copies all the bits from `a` to `b`
- Passing parameters copies the bits

## Build lists!
```
public class IntList {
	public int first;
	public IntList rest;
	public static void main(String[] args) {
		IntList L = new Intlist();
		L.first = 5;
		L.rest = null;

		L.rest = new IntList();
		L.rest.first = 10;

		L.rest.rest = new IntList();
		L.rest.rest.first = 15;
	}
}
```
![[Pasted image 20240722214654.png]]
Not neat!
How abt...
```
...
public IntList(int f, IntList r) {
	first = f;
	rest = r;
}

public static main(String[] args) {
	IntList L = new IntList(15, null);
	L = new Intlist(10, L);
	L = new IntList(5, L);
}
```
![[Pasted image 20240722215445.png]]
Constract backward!

to make users' lives easier, we could add more functions to automatly calculate.
```
/*a recursive way*/
public int size() {
	if (rest==null) {
		return 1;
	} else {
		1 + rest.size();
	}
}

/*a iterative way*/
public int size() {
	IntList p = this;
	int totalSize = 0;
	while (p != null) {
		p = p.rest;
		totalSize++;
	}
	return totalSize;
}

/**/
public int get(int index) {
	if (index == 1) {
		return this;
	} else {
		return rest.get(index - 1)
	}
}
```
