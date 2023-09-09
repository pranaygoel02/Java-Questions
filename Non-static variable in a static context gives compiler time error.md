# Why Non-Static variables cannot be accessed in a static context and why doing so throws a compile time error?
Static block is executed at the class loading time. We can use static keyword for blocks, variables, and functions and the order in which they are executed is also the same. Now when a class loads, all static block is first executed, then any static variable or function given memory space.

Non-static variables are nothing but instance variables, i.e, it belongs to the instance of the class, popularly known as objects. So an instance or non-static variable is allocated memory space when the object is created.

For example, say we have 2 variables (1 static and 1 non-static) and 2 functions (1 static and 1 non-static) in class A.
```java
// Program saved in a file called A.java

class A {
  static {
    System.out.println("I am a static block, and I'll run even before the main function runs");
  }
  
  static int a = 5;
  int b = 5;
  
  public static void display1() { // this belongs to the class
    System.out.println(a); // this works fine
    System.out.println(b); // this gives compile-time-error
  }
  
  public void display2() { // this belongs to the object of class A
    System.out.println(a); // this works fine
    System.out.println(b); // this works fine
  }
  
  public static void main() {
    A.display1(); // works fine
    A.display2(); // compile-time-error
  }
}
```
Straight forward, the JVM compiler is smart enough to understand the rules of a valid java program, here refering to the invalid syntax of using non-static variables in a static context and thus throws the above errors at compile time.
## But WHY?
For a moment let's assume that there's no concept of compiler and no errors are checked, program is saved and is being executed.
For this let's see this in a series of steps of what would happen when the program would actually run.
- Step 1: We run the java application `A.java`
- Step 2: Byte Code generated.
- Step 3: JVM starts the interpretation of byte code and starts to allocate memory at CLASS LOADING TIME.
- Step 4: All static blocks, variables and functions have been allocated memory.
  In our example say,

  Memory Stack
  | Variable | Address |
  | -------- | ------- |
  | a | 100 |

  | Methods Allocated Memory Space |
  | -------- |
  | display1() |

  Since static variable is allocated memory only once so there's only one copy of the variable available during execution, i.e., single memory address.
  But for non-static variables, multiple copies are made for every object created and each object is given a separate stack, with each variable being allocated a fresh memory space.
  So for say,

  A obj1 = new A();
  
  A obj2 = new A();

   two copies of variable b is created say at 102 and 103 memory address. For this reason, when b is called in a static context- common to all objects, JVM won't understand which copy of b is being referenced and would throw an error.
  To prevent such scenarios, the compiler gives errors during compilation itself.

  Hence `variable b` in display2() works fine is as it is non-static so it different for every object being created and knows which b it is pointing to.
