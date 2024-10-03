# Discussion 0
*Introduction to Java*

```
public class CS61BStudent { // Class Declaration 
	public int idNumber; // Instance Variables 
	public int grade; 
	public static String instructor = "Hug"; // Class (Static) Variables 
	
	public CS61BStudent(int id) { // Constructor 
		this.idNumber = id; 
		this.grade = 100;
	} 
	
	public void watchLecture() { 
	
	// Instance Method ... 
	
	} 
	
	public static String getInstructor() { 
	
	// Static Method ... 
	
	}
}
```

```
public class CS61B {

	public String university = "UC Berkely"
	public String semester;
	public CS61BStudent[] students; 

	public CS61B (int capacity, CS61BStudent[] signups, String semester) {
		this.semester = semester;
		this.students = signups;
		this.capacity = capacity;
	}

	public void makeStudentsWatchLecture() {
		for (int i = 0; i < capacity; i++) {
			this.students[i].watchLecture();
		}
	}

	public void changeUniversity(String newUniversity) {
		this.university = newUniversity;
	}

	public void expand(int newCapacity) {
		this.capacity = newCapacity;
	}
}
```

# Disscussion 1
*exam level*

## 1 Give 'em the 'OI Switcheroo
*For each function call in the main method, write out the x and y values of both foobar and baz after executing that line. (Spring ’15, MT1)*
```
public class Foo {
    public int x, y;

    public Foo (int x, int y) {
        this.x = x;
        this.y = y;
    }

    public static void switcheroo (Foo a, Foo b) {
        Foo temp = a;
        a = b;
        b = temp;
    }

    public static void fliperoo (Foo a, Foo b) {
        Foo temp = new Foo(a.x, a.y);
        a.x = b.x;
        a.y = b.y;
        b.x = temp.x;
        b.y = temp.y;
    }

    public static void swaperoo (Foo a, Foo b) {
        Foo temp = a;
        a.x = b.x;
        a.y = b.y;
        b.x = temp.x;
        b.y = temp.y;
    }

    public static void main (String[] args) {
        Foo foobar = new Foo(10, 20);
        Foo baz = new Foo(30, 40);
        switcheroo(foobar, baz);    // foobar.x: ___ foobar.y: ___ baz.x: ___ baz.y: ___
        fliperoo(foobar, baz);      // foobar.x: ___ foobar.y: ___ baz.x: ___ baz.y: ___
        swaperoo(foobar, baz);      // foobar.x: ___ foobar.y: ___ baz.x: ___ baz.y: ___
    }
}

```
```
--------------
foobar.x = 10;
foobar.y = 20;
baz.x = 30;
baz.y = 40;
--------------
foobar.x = 30;
foobar.y = 40;
baz.x = 10;
baz.y = 20;
--------------
foobar.x = 10;
foobar.y = 20;
baz.x = 10;
baz.y = 20;
```


## 2 Quick Math
*What would the contents of the array be after being run through these functions in the main method? (Fall ’16, MT1)*
```
public class QuikMaths {
    public static void multiplyBy3(int[] A) {
        for (int x : A) {
            x = x * 3;
        }
    }

    public static void multiplyBy2(int[] A) {
        int[] B = A;
        for (int i = 0; i < B.length; i+= 1) {
            B[i] *= 2;
        }
    }

    public static void swap(int A, int B) {
        int temp = B;
        B = A;
        A = temp;
    }

    public static void main(String[] args) {
        int[] arr;
        arr = new int[]{2, 3, 3, 4};
        multiplyBy3(arr);

        /* Value of arr: {____________________} */

        arr = new int[]{2, 3, 3, 4};
        multiplyBy2(arr);

        /* Value of arr: {____________________} */

        int a = 6;
        int b = 7;
        swap(a, b);

        /* Value of a: ______   Value of b: ______ */
    }
}

```

```
[2, 3, 3, 4]
[4, 6, 6, 8]
a = 6, b = 7
```


## 3 Static Books
```
class Book {
    public String title;
    public Library library;
    public static Book last = null;

    public Book(String name) {
        title = name;
        last = this;
        library = null;
    }

    public static String lastBookTitle() {
        return last.title;
    }
    public String getTitle() {
        return title;
    }
}

class Library {
    public Book[] books;
    public int index;
    public static int totalBooks = 0;

    public Library(int size) {
        books = new Book[size];
        index = 0;
    }

    public void addBook(Book book) {
        books[index] = book;
        index++;
        totalBooks++;
        book.library = this;
    }
}

```
(a) For each modification below, determine whether the code of the Library and Book classes will compile or error if we only made that modification, i.e. treat each modification independently. 
1. Change the totalBooks variable to `non static`
2. Change the lastBookTitle method to `non static`
3. Change the addBook method to `static` 
4. Change the last variable to `non static` 
5. Change the library variable to `static`

1. compile
2. compile
3. error
4. error
5. compile

(b) Using the `Book` and `Library` classes from before, wirte the output of the `main` method below. If a line errors, put the precise reason it errors and continue execution
```
public class Main {
    public static void main(String[] args) {
        System.out.println(Library.totalBooks);        "0"
        System.out.println(Book.lastBookTitle());      "error: Null pointer"
        System.out.println(Book.getTitle());           "error: not compile"

        Book goneGirl = new Book("Gone Girl");
        Book fightClub = new Book("Fight Club");

        System.out.println(goneGirl.title);            "Gone Girl"
        System.out.println(Book.lastBookTitle());      "Fight Club"
        System.out.println(fightClub.lastBookTitle()); "Fight Club"
        System.out.println(goneGirl.last.title);       "Fight Club"

        Library libraryA = new Library(1);
        Library libraryB = new Library(2);
        libraryA.addBook(goneGirl);

        System.out.println(libraryA.index);            "1"
        System.out.println(libraryA.totalBooks);       "1"

        libraryA.totalBooks = 0;
        libraryB.addBook(fightClub);
        libraryB.addBook(goneGirl);

        System.out.println(libraryB.index);            "2"
        System.out.println(Library.totalBooks);        "2"
        System.out.println(goneGirl.library.books[0].title);   "Fight Club"
    }
}
```

# Discussion 4
## 1 It's a Bird! It's a Plane! It's a CatBus!
(a)
```
interface Vechile {
	public void revEngine();
}

interface Honker {
	public void honk();
}

public class CatBus implements Vechile, Honker {
	@Override
	public void revEngine() { /*hidden*/ }

	@Override
	public void honk() { /*hidden*/ }

	/** Allows CatBus to honk at other CatBuses. */
	public void conversation(CatBus target) {
		honk();
		target.honk();
	}
}
```

(b)  
```
public void conversation(Honker target) {
	honk();
	target.honk();
}
```

(c)
```
Honker cb = new CatBus();
Honker hcg = new CanadaGoose();
```

## Raining Cats and Dogs
(a)

| line | Compile Time                                            | Runtime                                   | Output                                 |
| ---- | ------------------------------------------------------- | ----------------------------------------- | -------------------------------------- |
| 8    | not compile - Animal is not necessarily a Cat           | N/A                                       | compile error                          |
| 9    | Animal's greet(Animal)                                  | Dog's greet(Cat)                          | "Dog Pluto says: woof!"                |
| 10   | Animal's sleep()                                        | N/A - sleep is static!                    | "Naptime!"                             |
| 11   | Cat's play(String)                                      | Cat's play(string)                        | "woo it is so much fun being a cat :D" |
| 12   | Cat's greet(Dog)                                        | Cat's greet(Dog)                          | "Cat Garfield says: Meow!"             |
| 13   | Animal's greet(Dog)                                     | Animal's greet(Dog)                       | "Hi Lucky, I'm Garfield"               |
| 14   | Dog's sleep()                                           | N/A - sleep is static!                    | "Naptime!"                             |
| 15   | works                                                   | works                                     | ok                                     |
| 16   | Error: play(int) not defined                            | N/A                                       | compile error                          |
| 17   | Animal's play()                                         | Error: an Animal is not necessarily a Cat | runtime error                          |
| 18   | works                                                   | Error: a Cat is not a Dog                 | runtime error                          |
| 19   | Error: c is static type Cat but a is static type Animal | N/A                                       | compile error                          |
(b)
cast