---
slug: "lab-4"
title: "Lab 4"
weight: 4
---

# Lab 4 Notes

> **Due 06/26/2022 23:59 EST** (2 weeks)

{{< hint info >}}

**Important Links**:

- [Lab Handout PDF](https://q.utoronto.ca/courses/260774/files/21127182)
- [OOP Example from Lecture](https://q.utoronto.ca/courses/260774/files/21136248)
- [The `super` keyword in Java](https://www.javatpoint.com/super-keyword)
- [More info on `equals` and `hashCode`](https://www.baeldung.com/java-equals-hashcode-contracts#overview)
- [Eclipse Error: `...does not declare serialVersionUID` fix](https://stackoverflow.com/a/9382331)   
  

{{< /hint >}}


### Creating Subclasses (Inheritance)

Example:
```java
public class Square extends Shape {
  public Square(Any arg) {
    // Pass `arg` to the `Shape` constructor
    super(arg);
  }
}
```
- The `extends` keyword makes `Square` the subclass

- The `super` method invokes the constructor of `Shape`, allowing any attributes in the parent class to be initialized and to be used by the child class.

- `super()` will be called implicitly if you don't need to pass in any arguments to the parent constructor, but in this case we do.


### Abstract Classes
> Sort of a hybrid between interfaces and concrete classes

Definition:
```java
// Abstract class
public abstract class MarkBooster9000 {

  // Abstract method - no implementation (signature only)
  public abstract void boostMyMarkPlease(Student student);

  // Concrete method (signature + body)
  public static void curveGrades(Student[] students) {
    for (Student s: student) {

      // calling abstract method - completely valid
      this.boostMyMarkPlease(s);
    }
  }
}
```

- The `abstract` keyword can apply for *both* methods and classes
  
- Abstract methods **have no implementation**, and are meant to be defined by the child classes that extend the abstract class

- Abstract classes can have concrete methods, attributes and a constructor

### Exceptions
> Slides 27 - 33 of the [Object Oriented Programming Lecture](https://q.utoronto.ca/courses/260774/files/21065297)
  
Creating a custom exception:
```java
import java.lang.Exception
```
- Extend the `Exception` class, make sure to call `super` with the message in the construtor
- Set any attributes accordingly

Throwing exceptions:
```java
// class AcademicDishonestyException extends Exception { ... }

// Add throws declaration
public void passCourse(Student s) throws AcademicDishonestyException {

  if (s.hasCheated()) {
    // Throwing the actual exception
    throw new AcademicDishonestyException(s.name + " copy pasted code");
  }

  // any code here does not run if an exception is thrown
  // like an early return
}
```

- Exceptions bubble up: if no `try/catch` or any other handlers are present, the exception errors to `System.err` 

### Implementing the `Comparable` Interface

- `Comparable` allows us to compare two non-primitive types with each other
  
- Allows a list of Objects to be sorted based on `compareTo` implementation ([`sort` method](https://www.javatpoint.com/java-list-sort-method))
  - i.e. `String` is sorted lexicographically (default), but what about `Food` or `Song` classes?

Example:
```java
import java.lang.Comparable;

public class Student implements Comparable<Student> {
  int mark;

  public Student(int mark) {
    this.mark = mark;
  }

  /**
   * @return (ANY NUMBER < 0) IF `this` < `s`
   *         (ANY NUMBER > 0) IF `this` > `s`
   *         0 IF `this` = `s`
   */
  @Override
  public int compareTo(Student s) {
    // Could also sort by name, etc.
    // Up to your implementation
    return this.mark - s.mark;
  }
}
```
- `implements` is the keyword for Interfaces, just as how `extends` is for classes
  
- The `@Override` annotation indicates that we're *specifically* implmenting `Comparable.compareTo` and not just any other method called `compareTo`

- The type passed to `Comparable<T>` is used as the type of input parameter in `compareTo`

- When returning a number, `-1` and `-20` make **zero difference** to Java, as long as the number is `< 0`, that indicates that `this` student is 'less' that `s`. Same applies for `> 0`.