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

# Lecture 6
*testing and debugging*
Write a method: giving some points, find the shortest way between two points
Formalization:
- find **input** and **output**
- **input:** an array of **2D** intiger points
- **output:** double
- **exception:** if there are 0/1 point, return 0;
- **example input:**
	- $(0,0), (7,7), (13,37), (10,11)$
	- `public double Distance(point p1, point p2)`
How to write a test?
```
public class TestClosest() {
	public static void testClosest() {
		Point[] input = {
			new Point(0,0);
			new Point(3,4);
			new Point(8,2);
		};
		double expected = 5.0;
		double actual = Closest.findclosest(input);
		
		if (!expected.equals(actural)) {
			System.out.println("Incorrect resultL expected: '" + expected + "', but got '" + actural + "'");
		}
		return;
	}
}
```
this is called **unit test**

- Unit test
	- test desighed to catch bugs
	- small input size so you can run the test manually
- Comprehensive Test
- Edge Case Tests
- Integration Tests

**Develop Philosophy**
- ADD (**A**utograder **D**riven **D**evelopment)
	- slow, unsafe
	- NOT EFFICIENT!
- TDD (**T**est **D**riven **D**evelopment)
	- fast feedback
	- design a easy way to get feedback before you start executing

# Lecture 7
*build a list using array*
use an array in the back instead of linked list
**array resizing:**
- create a slightly larger array, and copy the old array to the new array **OR?**
- we can double the size of the array each time they are fulfiled.

# Lecture 8
*interface & implementation inheritance*
if we want a function to have different argument, there are many ways to deal with it:
- **overloading**, copy the same function and then change the argument and logic.
	- code is visually identical
	- won't work for future data type
	- harder to maintain
- **use hypermyn**
	- define a hypermyn using **interface** keyword
	- using **implememts** keyword to declear a hypomyn for an interface

**Overloading vs. Overriding**
If a "subclass" has a method with exact same signature as in the "superclass", we say the subsclass **overrides** the method

methods with same method name but different arguments are **overloaded** methods

Speciftying the capabilities of a subclass using the `implements` key

If the "supclass" waht to set a default implementation for method, use `default` keyword.

If you don't like the method you inherited, you can override it.

# Lecture 9
*Extends, Casting, Higher Order Functions*
Lsat time: 
**Interface inherience** `interface`keyword
**Implementation inherience** `default`keyword

new keyword `extends`, allows you to inherit from a class
`SLList -> RotateSLList` by `public class RotateSLList<Item> extends SLList<Item>`

keyword`super`, allows you to access the parent class's method
when you call the child class's constructer, java will autometically call `super()`

everything in java extends `Object`
**dangerous**: `extends` should only be used for "**is-a**" relationship (SLList is a list)
you shouldn't use `extends` for a "**has-a**" relationship (cats has paws)

related idea: ***Module***
hide implementation, show interfaces

**Implementation Inheritance Breaks Encapsulation**

cast `(Poodle) maxDog(d1, d2)`, a cast told the compiler to be less cautious

# Lecture 10
*more inheritance!*
## Higher Order Function
function that takes function as parameter
*an idea in FP*
```
public class TenX {
	public int apply(int x) {
		return 10 * x;
	}
}

public class HoFDemo {
	public static int doTwice(TenX f, int x){
		return f.apply(f.apply(x));
	}

	public static void main(String[] args) {
		System.oyt.print(doTwice(new TenX(), 2))
	}
}
```
if we have other int unary function, we could create a `IntUnaryFunction` interface and implements it in different classes and pass the instances of `IntUnaryFunction` as paramter

## Subtype Polymorphism
*providing a single interface to entites of different types*

**HoF apporach:**
```
def print_larger(x, y, compare, stringify):
	if compare(x,y):
		return stringify(x)
	return stringify(y)
```

**subtype polymorphism apporach:**
```
def print_larger(x, y):
	if x.largerThan(y):
		return x.str()
	return y.str()
```

implement `max()` for all array
`public static OurComparable max(OurComparable x, OurComparable y)`

```
public interface OurComaprable {
	public int compareTo(Object obj);
}
```

*trick!*
wirte:
```
rteurn size - otherDog.size;
```
rather than:
```
if (size > otherDog.size) {
	return 1;
} else if (size = otherDog.size) {
	return 0;
} else {
	return -1;
}
```

in java, we have `interface comparable<T>`

## Comparators
if we want to compare Objects by different factor, we could use **comparator**
```
public class NameComparator implements Compartor<Dog> {
	public int compare(Dog d1, Dog d2) {
		return d1.name.compareTo(d2.name);	
	}
}
```

```
NameCompartor nc = new NameCompartor();
if (nc.compare(d1, d2) < 0) {
	d4.bark();
} else {
	d5.bark();
}
```

# Lecture 11
*array set*
a set contains different items
## Iteration
```
for (int i : aset) {
	System.out.println(i);
}
```
now we are going to make our aset support "enhanced for" loop
actually the fancy for loop equals:
```
Interator<Integer> seer = javaset.iteratior();

while (seer.hasNext()) {
	int x = seer.next();
	System.out.println(x);
}
```

```
private class ArraySetIterator implements Iterator<T> {
	public int wizPos;

	public ArraySetIterator() {
		wizPos = 0;
	}

	public boolen hasNext() {
		return wizPos < size;
	}

	public next() {
		T returnItem = items[wizPos];
		wizPos += 1;
		return returnItem;
	}
}
public Iterator<T> iterator() {
	return new ArraySetIterator();
}
```
if a list implements iterator, then it implements `Iterable`.

# Lecture 13
*Asymptotics*
```
for (int i = 0; i < A.length; i++) {
	if (A[i] == A[i + 1]) {
		return true;
	}
}
return false;
```

| operation         | sym. count | count (N = 10000) |
| ----------------- | ---------- | ----------------- |
| `i = 0`           | 1          | 1                 |
| less than (`<`)   | 1 to N     | 0 to 10000        |
| increment (`+=1`) | 0 to N-1   | 0 to 9999         |
| equals (`=`)      | 1 to N-1   | 1 to 9999         |
| array access      | 2 to 2N-1  | 2 to 19998        |
we only care the largest order term

