# HW0
## Problem 1
**[Self-Check 1.26: Confusing](https://practiceit.cs.washington.edu/problem/view/bjp5/chapter1/s26%2DConfusing)**
```
I am method 1.  
I am method 1.  
I am method 2.  
I am method 3.  
I am method 1.  
I am method 1.  
I am method 2.  
I am method 1.  
I am method 2.  
I am method 3.  
I am method 1.
```

## Problem 2
**[Exercise 2.5: `starTriangle`](https://practiceit.cs.washington.edu/problem/view/bjp5/chapter2/e5%2DstarTriangle)**
```
for (int i = 0; i < 5; i++) {
    for (int j = 0; j <= i; j++) {
        System.out.print("*");
    }
    System.out.print("\n");
}
```

## Problem 3
**[Self-Check 2.25: `numberTotal`](https://practiceit.cs.washington.edu/problem/view/bjp5/chapter2/s25%2DnumberTotal)**
```
24 1
22 2
19 3
15 4
10 5
```

## Problem 4
**[Exercise 3.23: `printIndexed`](https://practiceit.cs.washington.edu/problem/view/bjp5/chapter3/e23%2DprintIndexed)**
```
public static void printIndexed(String str) {
    int length = str.length()-1;
    for (int i = length; i >= 0; i--) {
        System.out.print(str.charAt(length-i));
        System.out.print(i);
    }
}
```

**[Exercise 4.17: `stutter`](https://practiceit.cs.washington.edu/problem/view/bjp5/chapter4/e17%2Dstutter)**
```
public static String stutter(String str) {
    int length = str.length();
    String returnArray = "";
    for (int i = 0; i < length; i++) {
        returnArray = returnArray+str.charAt(i)+str.charAt(i);
    }
    return returnArray;
}
```

## Problem 5
**[Self-Check 4.5: `ifElseMystery1`](https://practiceit.cs.washington.edu/problem/view/bjp5/chapter4/s5%2DifElseMystery1)**
```
ifElseMystery1(3, 20) (13, 21)
ifElseMystery1(4, 5) (5, 6)
ifElseMystery1(5, 5) (6, 5)
ifElseMystery1(6, 10) (7, 11)
```

## Problem 6
**[Exercise 4.19: `quadrant`](https://practiceit.cs.washington.edu/problem/view/bjp5/chapter4/e19%2Dquadrant)**
```
public static int quadrant(double x, double y) {
    if (x*y == 0) {
        return 0;
    }
    if (x>0&&y>0) {
        return 1;
    }
    if (x<0&&y>0) {
        return 2;
    }
    if (x<0&&y<0) {
        return 3;
    }
    return 4;
}
```
