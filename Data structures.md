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

# Lecture 4
*form naked linked list to a usable list*
## Form basic SLList
```
public class IntNode {
	public int item;
	public IntNode next;

	public IntNode(int i, IntNode n) {
		item = i;
		next = n;
	}
}
```
```
public class SLList {
	private IntNode first;

	public SLList(int x) {
		first = new IntNode(x, null);
	}

	public void addFirst() {
		first = new IntNode(x, first);
	}

	public int getFirst() {
		return first.item;
	}
	
	public void addLast() {
	IntNode p = first;
	while (p.next != null) {
		p = p.next;
	}
	p.next = new IntNode(x, null);
}
```

## **Implementation**(*private*) vs. **Interface**(*public*)
- Hide implementation details from user of your classes
	- Less for user of class to understand
	- Safe for you to change private methods (implementation)
- Despite the term '*access control*'
	- Nothing to do with protection against hackers, spies, and other evil entities
## Calculate `size`

```
private int size(IntNode p) {
	if (p.next == null) {
		return 1;
	} else {
		return 1 + size(p.next);
	}
}
public int size() {
	return size(first);
}
```
To implement a recursive method in a class that is not itself recursive
- Create a private recursive helper method
- Have the public method call the private recursive helper method

**OR?**

```
public class SLList {
	...
	int size;
	...
}

public class addFirst(int x) {
	...
	size++;
	...
}
```
Faster! But takes a little bit more memory
Solution: Maintain a special size variable that caches the size of the list.
- Caching: putting aside data to speed up retrieval

TANSTAAFL: There's no such thing as free lunch.
- But spreading the work over each add call is a net win in almost any circumstance.

## Create an empty list
```
public SLList() {
	first = null;
	size = 0;
}
```
But? what if we create a empty list and then add last?
CRASH!
`first` is null, i.e. there's no Object `first` pointing, then we cannot call `first.next`!

**Solution**! Have sentinel node (*dummy head*)!
```
private IntNode sentinel;

public SLList(int x) {
	sentinal = new IntNode(0, null);
	sentinal.next = new IntNode(x,null);
	size = 1;
}
public SLList() {
	sentinal = new IntNode(0, null);
	size = 0;
}
```

# Lecture 5
*fix our SLList*
`addLast()` is pretty slower than `addFirst()`, how can we make `addLast()` faster?
**answer**: Doubly linked list
```
public class IntNode {
	public int item;
	public IntNode next;
	public IntNode prev;

	public IntNode(int i, IntNode n) {
		item = i;
		next = n;
	}
}
```
Since we have sentinal node at the begining of the list, which helps us to simplify the case
Then in DLList, we could have two sentinal nodes, `sentFront`, `sentBack`
**BUT!**
we could have **only one** sentinal node, whose's `next` is the first node of the list, `prev` is the last node of the list (**Pretty neat!** **DEEP BEAUTIFUL!**) circular list.

## Arrays
*also try java visualizer*
```
x = new int[3];
// creates array conatining 3 int boxes (32*3=96 bits in total)
```
Array are not premitive types, which means the variables are reference to the object;
weird function`System.arraycopy(to, start_idx, from, start_idx, copy_count)`
Arrays can be computed at runtime
```
int index = askUser();
System.out.println(arr[index]) // totaly fine
```
