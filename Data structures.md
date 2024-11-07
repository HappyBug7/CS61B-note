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
**array resizing:** ^778dec
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

# Lecture 14
*Disjoint Sets*
The choice of data structure we use hugly effect how fast our code run.
The Disjoint set data structure has two operations
- `connect(x,y)`: connect `x` and `y`
- `isConnected(x,y)`: Returns whether `x` and `y` are connected (connection can be transitive)

we don't need to keep track of how they are connected. Instead, we can divide the nodes into different sets
- `connect(x,y)`: 
	```
	if x is uninnitialized:
		if y is uninnitialed:
			assign x and y to a new set
		else:
			assin x to y's team
	else:
		same as previous
	```

- `isConnected(x,y)`
```
	return x.team == y.team
```

idea #1:
- use `List<Set<Integer>>`
- **NO!!!**
- requires iterating trough all the set to find anything, **SLOW!**
- constructor: $\Theta(N)$
- `connect(x,y)`: $\Theta(N)$
- `isConnected(x,y)`: $\Theta(N)$

improvement: **quick find**
use an array to keep track of all the items:
```
innitially: [0,1,2,...,N]
connect i and j: 
for (int k = 0; k < a.length; k++) {
	if (a[k] == a[i]) {
		a[k] = a[j]
	} 
}
check if i and j are in the same team: a[i] == a[j]
```
- `connect`: $\Theta(N)$
- `isConnected`: $\Theta(1)$

improvement: **quick union**
```
innitial: [-1, -1, ..., -1]
assign i to j's team: Find root(i), Find root(j), j's root = i's root
isConnected: root(x) == root(y) (can use recursive implementation)
```
![[Pasted image 20241003215951.png]]
- `connect(x,y)`: $O(N)$
- `isConnect(x,y)`: $O(N)$

improvement: weighted quick union
*always chase the shortest height*
![[Pasted image 20241003221203.png]]
put the smaller tree under the larger tree
- `connect(x,y)`: $O(\log(N))$
- `isConnected(x,y)`: $O(\log(N))$

**Even more good**:
every time you call `isConnected`, tie the node to the root, which will flatten your tree!

# Lecture 15
*Asymptotics II*
```
int N = A.length;
for (int i = 0; i < N; i++) {
	for (int j = 0; j < N; j++) {
		if (a[i] == a[j]) {
			return true
		}
	}
}
```

```
for (int i = 0; i < N; i++) {
	for (int j = 0; j < i; j++) {
		// 1 unit of work
	}
}
```

```
for (int i = 0; i < N; i++) {
	// N - i - 1 unit of work
}
```
$\sum\limits_{0}^{N - 1} = \frac{N(N-1)}{2}$

```
for (int i = 1; i < N; i = i * 2) {
	for (int j = 0; j < i; j++) {
		// 1 unit of work
	}
}
```

| N    | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | ... |
| ---- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| C(N) | 1   | 3   | 3   | 7   | 7   | 7   | 7   | 15  | ... |
$C(N) = 1+2+...+N = 2N-1$, if $N$ is the power of 2
best case: $N$ is the power of 2, $C(N) = 2N-1$
worst case: $N+1$ is the power of 2, $C(N) = 2(\frac{N+1}{2})-1 = N$
$C(N)\in [N,2N-1]$
therefore $C(N) = \Theta(N)$

```
for (int i = 0; i < N; i++) {
	if (i is the power of 2) {
		resize();
	}
	addLast();
}
```
with total runtime be $\Theta(N)$

**recursion**
```
public static int func() {
	if (n <= 1) {
		return 1;
	}
	return func(n-1) + func(n-1);
}
```
$C(N) = 1 + 2 + 4 + ... + 2^{N-1} = 2^{N} - 1$
$R(N) = \Theta(2^{N})$

**merge sort**
*use recursive ways to sort*
merge two arrays $\Theta(N)$
```
static void sortTime(int n) {
	sortedTime(n/2);
	sortedTime(n/2);
	// n units of work
}
```
$C(N) = \begin{cases} 1  \quad\quad\quad\quad\quad\quad\quad N < 2 \\ 2C(N/2)+N \quad N \ge 2 \end{cases}$
$R(N) = \Theta(N\log N)$

# Lecture 16
*abstract data type*
An Abstract Data Type (**ADT**) is defined only by its operations, not by its implementations

```
List<Integer> L = new ArrayList<>();
```
the built-in `java.util` package provides a number of useful:
- Interfaces
- Implementations

**Binary search tree**
same as binary search, we could break the list at the middle, and the middle node's left is the center node of the left list, right the same

A tree consists of :
- A set of nodes
- A set of edges that connect those nodes
	- There's exactly one path between two nodes
a node is also a tree

parent: the first node on the path to `root` (formal)

**BST property:** For every node X in the tree:
- Every key in the left subtree is **less** than X's key
- Every key in the right subtree is **greater** than X's key
**no duplicate key!!!**

```
static BST find(BST T, Key sk) {
	if (T == null) {
		return null;
	}
	if (sk.equals(T.key)) {
		return T;
	} else if (sk < T.key) {
		return find(T.left, sk);
	} else {
		return find(T.right, sk);
	}
}
```

how to insert?
```
static BST insert(BST T, Key ik) {
	if (T == null) {
		return new BST(ik);
	}
	if (ik < T.key) {
		T.left = insert(T.left, ik);
	} else if (ik > T.key) {
		T.right = insert(T.right, ik);
	}
	return T;
}
```
???

how to delete?
break down to cases
- node has no children
- node has one children
- node has two children

Choose the predecessor or successor
![[Pasted image 20241008232947.png]]

# Lecture 17
*B-trees*
worst case BSTs' height are $\Theta(N)$

**depth**
the steps take from `root` to `node`
**height**
the depth of the deepest leaf

**B-trees**
ideas
- Never add new leaves at the bottom![[Pasted image 20241011173721.png]]
- set a limit `L` to the number of items an leaf could have
- once a node has more than L items, givr an item to parent![[Pasted image 20241011174401.png]]
the only way that B-tree can grow is by dividing roots, which ensures the nodes beneath almost full

B-trees ensures:
- all leaves has the same depth
- if a node has more than one element, it must have `L+1` childs
- height grows with $\Theta(log_{L}(N))$

**special case:**
- `L = 2`, we call "2-3 trees"
- `L = 3`, we call "2-3-4 trees"

# Lecture 18
*Red Black Trees*

**Tree Rotation**
`rotateLeft(G)`: let x be the right child of G. Make G the new left child of x.
![[Pasted image 20241012142113.png]]
![[Pasted image 20241012142129.png]]
![[Pasted image 20241012142202.png]]
![[Pasted image 20241012142211.png]]
reverse operation: `rotateRight(G)`

by a sequence of rotation, we can transfrom the shape of a BST.

**Left Leaning Red Black Tree(LLRBs)**
keep two trees, one 2-3 Tree, and one BST
![[Pasted image 20241012144701.png]]
- LLRBs are normal BSTs
- There's a 1-1 correspndence between an LLRB and an equivalent 2-3 tree
- The red is just a convenient fiction. Red links dont 'do' anything special

an LLRB has no more than $2x$ the height of its 2-3 tree

Where do LLRBs come from?
- insert as usual into a BST
- then use zero or more rotations to maintain the 1-1 relationship
![[Pasted image 20241012151027.png]]
- for node without child
	- add left: use red link
	- add right: use red link then `rotateLeft(Node)`
- for node with left child
	- add left: `rotateRight(parentNode)`
	- add right
In general,  **add** then **rotate**
![[Pasted image 20241012151714.png]]

# lecture 19
*Hash Tables*
our search tree require items to be comparable

BobaCounter set
put item with same tail together
use linked list can prevent space weast

let $N$ be the number of items, $M$ be the number of sets, then `contains(item)` runs in $\Theta(N/M)$ time

suppose we set a rule that when $N/M$ is $\ge 1.5$, we double $M$ (just like what we do when building arraylist)
![[Pasted image 20241013155139.png]]
this operation makes the size of set around $N$, which made `add(item)` and `contains(item)` takes constant time.

our set works pertty good for integers, but we want to store all type of items
example: String -> Integer
- Base 26: $cat_{26} = (3 \times 26^{2})+(1\times 26^{1})+(20\times 26^{0}) = 2074_{10}$
- Hash code
some item's hashcode is negative, how can we compute negative numbers mod integers?
-1 mod 4 = 3 mod 4 = 3 (remenber to use `Math.floorMod()`)

# Lecture 20
*Hashing II*
what's the default hashcode mathod for objects?
return their address
we want our hash function can spread our items evenly

since the default hashcode method is a good spread, then why should we have customized function?

another case where hash table may crash:
mutable object

# Lecture 21
*PQ & Heaps*
```
public interface MinPQ<Item> {
	public void add(Item x);

	public Item getSmallest();

	public Item removeSmallist();

	public int size();
}
```
Usful cases:
- capture the largest/smallest $M$ items

Binary min-heap: Binary tree that is complete and obeys ***min-heap property***
- every node is less than or equal to both of its children
- complete: all nodes are as left as possible and no missing items at front `height - 1` levels

`getSmallest()`: return root *constant time*

`insert()`: 
- add to the end of the heap
- swim up the hierarchy to rightful place.
$\log(n)$ time

`removeSmallest()`: 
- delete root
- put the last item to the root
- sink down to its rightful place
$\log(n)$ time

**implementation**
- Tree impletation
- Array impletation
`node.leftChild = 2 * i + 1`
`node.rightChild = 2 * i + 2`
`node.parent = (i - 1) // 2`

# Lecture 22
*graphs and traversal*

***traversal passed***

A graph consists of:
- A set of nodes
- A set of edges that connects the nodes

A simple graph is a graph with:
- No edges that connect a vertex to itself
- No two edges that connect the same vertices

is there a path between two vertex?
- Does `s==t`? if so, return true
- Otherwise, if `connected(v,t)` for any neighbor v of s, return true
- return false
? will cause infinate loop

fix: add state for every node
- mark s.
- Does `s==t`? if so, return true
- Otherwise, if `connected(v,t)` for any unmarked neighbor v of s, return true
- return false
also called **depth first traversal**

# Lecture 23
*graph implementation*
```
public class Graph {
	public Graph(int V);
	public void addEdge(int v, int w);
	Iterable<Integer> adj(int v);
	int V();
	int E();
	...
}
```

```
public class Paths {
	public Paths(Graph G, int s);
	boolean hasPathTo(int v);
	Iterable<Integer> pathTo(int v);
}
```

```
public class DepthFirstPaths {
	private boolean[] marked;
	private int[] edgeTo;
	private int s;
	...
}
```
Graph representation
(1) Adjacency Matrix
![[Pasted image 20241017231923.png]]

(2) store Collection of edges
$\{(0,1), (0,2), (1,2)\}$

(3) Adjacency Lists
operation time cost
```
for (int v = 0; v < G.V(); v++) {
	for (int w: G.adj(v)) {
		System.out.printLn(v + "-" + w);
	}
}
```
Runtime to iterate over `v`'s neighbors: $\Omega(1), O(V)$
therefore the `print` method takes best case: $\Theta(V)$, worst case: $\Theta(V^{2})$
acturally the `print` method takes $\Theta(V+E)$ runtime
because:
- `v++` happens V times
- Print happens 2E times

# Lecture 24
BFS vs. DFS
- **Correctness:** both yes
- **Output Quality**
	- BFS guaranteed the shortest path (fewest node)
- **Time efficiency**
	- very similar
- **Space efficiency**
	- DFS is worse for spindly graphs
		- call stack gets very deep
	- BFS is worse for brushy graphs

Dijkestra Algorithm
for each child:
- if not in SPT, add edge
- if in, check if the path is better. if so, add edge and remove the previous edge (relax the edge)

# Lecture 25
*minimum spanning trees*
$A^{*}$
find the path from **Denver** to **NY**
- using Dijkestra algrorism, we will search everywhere 2000 km from Denver
- using $A^{*}$, we can have preference
storing vertices in order of $d(source, v)+h(v,goal)$

**minimum spanning tree**
Given a undiracted graph, a **spanning tree** T is a subgraph of G, where T:
- Is connected
- is acyclic
- Includes all of the vertices

Cut property:
![[Pasted image 20241021220953.png|]]
**minimum-weight crossing edge must be in the MST!**
then we build BST as below:
- choose a random node
- get the minumum edge
- assign the nodes connected as a team, the rest the other
- then repeat the operation until all the node are connected

# Lecture 26
*MSTs & Tries*
Prims's demo:
- choose a random node
- get the minumum edge
- assign the nodes connected as a team, the rest the other
- then repeat the operation until all the node are connected
*slow*

real Prims:
use dijketra, **but!** only cares the edge distance, (because the connected node are considered one node, which is MST)

Kruskal's demo:
- sort every edge and then add then to MST as long as this edge wont create a cycle
- remember to stop when the amount of edges reaches $n-1$
improvement:
- when adding edge, use QuickUnion to put the two node to the same team
- then when checking whether a edge will create cycle, check whether the nodes connected by the edge is in the same team
- when sort edges, create a PQ to deal with it
both of the algrorithm are based on ***cut porperty***
Prims: create MST by the order of nodes
Kruskals: create small MSTs and then union them

**Tries**
- each node contains a single letter of the string
- nodes can be shared by mutiple keys

# Lecture 27
*soft engineering*
working on small project isn't the same as working on large scale
**Complexity**

# Lecture 28
implemente Trie:
- using array map
- using hash map
- using BST
Use Tire, it's easy to implemente "prefix" matching operation
using prefix operation, we can implement auto completation
and we can attach extra information to "***key***" node (the end of a string), which implemented "string - value" map
**Directed Acyclic Graphs**

# Lecture 29
**reduction**
we reduces "DAG-LPT" to "DAG-SPT" by simply add some preprocess and postprocess

Sort algorithoms
An **inversion** is a pair of elements that are out of order with respect to **<**
![[Pasted image 20241028110431.png]]
**Selection sort**
- scan the whole array, and find the smallest item
- put the item to the very front of the array
![[Pasted image 20241028110855.png]]
$\Theta(N^{2})$

**Heap sort**
- insert all the item into the heap
- remove the root item and collect it
- then reconstruct the heap
![[Pasted image 20241028111314.png]]
$O(N\log(N))$
extra space: $O(N)$

**In-place Heapsort**
- heapification (heapify) $O(N\log(N))$
	- sink from back
- remove the top element, then heapify
$O(N\log(N))$

**Merge sort**
- firstly, we can merge two sorted array by:
	- using to pointers, each pointing at the start of the arrays
	- compare to get the smallest
		- put smallest into the result array
		- corresponding pointer++
	- continue doing so
- merge arrays seems a recursive process
- `sort(leftArray)`, `sort(rightArray)` and then merge them
- if we split all the way down to the size one, then all the time cost would be:
	- $O(N\log(N))$ !!!
![[Pasted image 20241028143558.png]]
the main cost is the `merge` operation

# Lecture 30
**Insertion sort**
- pick item one by one, and put them to the right palce

**In-place Insertion sort**
- pick an item, and travel ahead until meet smaller item.
- then choose the next item
- **BUBBLE SORT!**
$\Omega(N)$, $O(N^{2})$

**Quick sort**
- choose a pivot
- everything less than or equal to the pivot would be put to left, and others right
Best case: $\Theta(N\log(N))$
Worst case: $\Theta(N^{2})$

# Lecture 31
*software engineering*

# Lecture 32
*more sorting!*
if the piviot always end up at least 10% from either edge
then for the least lucky subarray: $N(\frac{9}{10})^{n} = 1$
$n = \log_{\frac{10}{9}}N = C\log N$
then total time cost would also be $\Theta(N\log N)$, the only difference is that the time scaled by constant figure.
Quick sort is BST sort!

Dealing with bad ordering
- Pick privots randomly
- Shuffle before sort

quick sort implementation
- in-place partitioning
	- two pointers one pointing at the beginning, the other pointing at the end of the array
	- firstly, move `L` pointer until it meet the item larger than the piviot
	- then move `G`, until it meet the item smaller than the pivot
	- then, swap `L` and `G`'s items
	- when they crossed, done.

Quick select
- first partition on a piviot, if the piviot is larger than the exact midium, re-partition the right part, else, re-partition the left part
- then recursive doing so
- finally, we found the exact midium
time cost: $N + \frac{N}{C} + \frac{N}{C^{2}} + ... = N*(\frac{1-(\frac{1}{C})^{\log_{C}N}}{1-\frac{1}{C}}) = N*\frac{1-\frac{1}{N}}{1-\frac{1}{C}} =\frac{C(N-1)}{C-1} = k(N-1) = \Theta(N)$
same as what we do when implementing array list
![[#^778dec]]

# lecture 34
*Theoretical bounds on sorting*
why's most sorting method ends up to $\Theta(N\log N)$? 
can we do  better? or can we proof that this is the best we can do?

Comparison sorts, which sort a list item using only two operations:
- swap
- compareTo

consider $N!$ and $(\frac{N}{2})^{\frac{N}{2}}$
is $N! \in \Omega((\frac{N}{2})^{\frac{N}{2}})$
yes!

then $\log(N!)\in \Omega(\frac{N}{2}\log(\frac{N}{2}))$
$\log(N!)\in \Omega(N(\log N-\log 2))$
$\log(N!)\in \Omega(N\log N)$

$N\log N\in \Omega(\log(N!))$?
$\log(N!) = \log (N) + \log (N-1) + ... + \log 1$
$\le \log(N) + \log(N) + ... + \log(N)$
$\in\Omega(N\log N)$

then, $N\log N = \Theta(\log(N!)$

! if we want to order a list, that means we have $N!$ universies, to sort them, the minimum height of our desition tree is $\log(N!)$, which is $\Theta(N\log N)$!

# Lecture 35

**digit by digit sort**

**counting sort**
- $O(N+R)$
- that means which slows us is the total amount of elements in the array and the range of keys. 

combine them, and we get:
digit by digit counting sort, which is also called: **Radix sort.**

# Lecture 36
*Radix sort*
**Least Significant Sort**: digit by digit sort form the last digit to the first digit
**Most Significant Sort**: digit by digit, but after each digit, split the array and then sort the sub arrays

