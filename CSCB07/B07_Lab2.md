---
slug: "lab-2"
title: "Lab 2"
weight: 2
---

# Lab 2 Notes

> **Due 06/05/2022 23:59 EST**

{{< hint info >}}

**Handy Links**:

- [Lab Handout PDF](https://q.utoronto.ca/courses/260774/files/21013446)
- [Set `JAVA_HOME` macOS](https://mkyong.com/java/how-to-set-java_home-environment-variable-on-mac-os-x/#java-home-and-macos-11-big-sur)

{{< /hint >}}

### Creating Files

**Note**: You don't need to use Eclipse or any IDE for this lab, since you will compile and run everything via the terminal.

If you don't like the terminal, you can just treat the
```bash
$ touch lab2/SomeFile.java
$ open lab2/SomeFile.java
```
as just creating and opening a new file using a File Explorer or Text Editor in the `/lab2` directory.

### `Polynomial` Constructor

In the Handout, it asks for a 'no-argument' constructor, and one that takes a double array. This is a form of [constructor overloading](https://www.geeksforgeeks.org/constructor-overloading-java/).

```java
public class Polynomial {
  private double[] coefficients;

  // no-argument
  public Polynomial() {
    // TODO
  }

  // double array
  public Polynomial(double[] coefficients) {
    // TODO
  }
}
```

### `hasRoot` Clarification

**YOUR METHOD BODY/IMPLEMENTATION MUST BE A 1-LINER!**

Hint: Can you think of another method in `Polynomial` that can help you?

### Adding Polynomials

It is *not* guaranteed that both polynomials would have the same degree. You will need to determine how to handle this in the for-loop when adding the coefficients arrays.

```java
double[] c1 = { 6, 5 };
Polynomial p1 = new Polynomial(c1);

double[] c2 = { 0, -2, 0, 0, -9 };
Polynomial p2 = new Polynomial(c2);

// valid
Polynomial s = p1.add(p2);
```

### Code Snippets


**Step 3.f** - `Driver.java`

```java
package lab2;

public class Driver {
  public static void main(String[] args) {
    Polynomial p = new Polynomial();

    System.out.println(p.evaluate(3));

    double[] c1 = { 6, 0, 0, 5 };
    Polynomial p1 = new Polynomial(c1);

    double[] c2 = { 0, -2, 0, 0, -9 };
    Polynomial p2 = new Polynomial(c2);

    Polynomial s = p1.add(p2);

    System.out.println("s(0.1) = " + s.evaluate(0.1));

    if (s.hasRoot(1)) {
      System.out.println("1 is a root of s");
    } else {
      System.out.println("1 is not a root of s");
    }
  }
}
```