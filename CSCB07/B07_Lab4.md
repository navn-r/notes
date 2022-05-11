---
slug: "lab-4"
title: "Lab 4"
weight: 2
---

# Lab 4 Notes
> Lab 4, **Due 10/24/2021 23:59 EST**

- Handy Links:
  - https://www.eclemma.org/userdoc/index.html
  - https://junit.org/junit5/docs/5.0.1/api/org/junit/jupiter/api/Assertions.html

- **Use Eclipse**, don’t bother with other editors

- Use these points for testing `isEquilateral`:

  ```java
  Point p1 = new Point(0, 0);
  Point p2 = new Point(Math.sqrt(5), 0);
  Point p3 = new Point(Math.sqrt(5) / 2.0, Math.sqrt(15) / 2.0);
  ```

- For submission, you can name the file anything

- **Code Annotations**:
	- Green: Fully Covered
	- Yellow: Partially Covered
	- Red: Not Covered

- It’s fine to not cover all branches, as long as the coverage is **100%**

  - https://piazza.com/class/kti52spsvua71f?cid=113

- To get rid of the coverage colors in the file, *just modify the file*

![code](https://i2.paste.pics/26dd62cb3f91cf965c631aa6318c7e51.png)



### Tips for Unit Testing

- Use one assertion per test

  ```java
  // Bad Example
  @Test
  void testThis() {
  	Point p1 = new Point(0, 0);
  	Point p2 = new Point(1, 1);
      
      assertNotEquals(p1, p2);
      assertEquals(p2.hashCode(), 8);
  }
  
  // Good Example
  @Test
  void testHashCode() {
      Point p = new Point(1, 1);
      assertEquals(p.hashCode(), 1);
  }
  
  @Test
  void testEquals() {
      Point p1 = new Point(0, 0);
  	Point p2 = new Point(1, 1);
      assertNotEquals(p1, p2);
  }
  ```

- Use assertions that match what you’re trying to test, even though it works

  ```java
  // Bad -> Good (a and b are type Object)
  assertTrue(a.equals(b)) -> assertEquals(a, b)
  assertEquals(a, null) -> assertNull(a)
  ```

- Try and write tests for sake of testing logic, *and not just for coverage* (focus on edge cases more)

  ```java
  // Example of a dumb test
  @Test
  void testEquilateral() {
  	Point p1 = new Point(0, 0);
  	Point p2 = new Point(0, 0);
  	Point p3 = new Point(0, 0);
      Triangle T = new Triangle(p1, p2, p3);
      assertTrue(T.isEquilateral());
  }
  ```

  

