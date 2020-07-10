---
title: Week 5
created: '2020-06-10T16:08:47.739Z'
modified: '2020-06-10T17:30:31.702Z'
weight: 5
---

# Week 5

## Programming in C

### Memory Model
  - Array of bytes, "Addresses" are indexes
  - Variables may occupy several consecutive bytes, its address refers to the *first* occupied byte
  - "Pointer": Variable/parameter that stores an address
  ```c
  int i = 69; // Suppose i was occupying bytes 45 to 74
  int *p = &i // 45
  ```

### Memory Regions
  - Text:
    - Stores code
    - Pointers pointing to functions point here
  - Global:
    - Stores global variables
  - Stack:
    - Used for function calls
    - Stores local variables
    - Auto allocation and deallocation
  - Heap:
    - Manual allocation (`malloc(), calloc()`) and deallocation (`free()`)
    - Used for dynamic data outside of functions
  
### Global Variables
  - Two types of varaibles
    ```C
    int pubVar = 10;              // top-level public global variable
    int f() {
      static int privVar = 1010;  // function private global variable
      pubVar++;
      privVar++;
      ...
    }
    ```

### Integer Types
  - All possible combinations: {`signed, unsigned` $\times$ `char, short, int, long, long long`}
  - Byte size depends on platform
    - eg. `x86-64`
  
      | Type                      | Size (bytes) |
      | :------------------------ | -----------: |
      | `char` (default `signed`) |            1 |
      | `short`                   |            2 |
      | `int`                     |            4 |
      | `long`                    |            8 |
      | `long long`               |            8 |

### Integer Literal Notation

| Literal |                 Type |
| :------ | -------------------: |
| 3       |                `int` |
| c       |               `char` |
| 3U      |       `unsigned int` |
| 3L      |               `long` |
| 3UL     |      `unsigned long` |
| 3LL     |          `long long` |
| 3ULL    | `unsigned long long` |
```C
printf("%lu\n", 3UL) // Good usage
printf("%lu\n", 3)   // Bad since 3 is `int`
```

### Type Casting with Numbers
  - Larger size type to smaller:
    - Automatic conversion
    - Lose some information in a natural way (`double` to `int` removes decimal place)
    - Better to explicitly typecast:
      ```C
      double d = 420.69;
      int i = (int)d;
      ```
  - Smaller size type to larger:
    - Automatic conversion
    - Completely lossless

### Implicit Number Promotion
  - The smaller operand type gets *promoted* to the larger operand type
    - Note: `char` and `short` are **_always_** promoted to `int`
  - eg.
    ```C
    // Suppose the following
    double d = 65.0;
    int i = 65;
    char c = 'A';
    /**
    i / c -> promotes c to `int` and preforms integer division
    d / j -> promotes j to `double` and preforms floating-point division
    **/
    ```
### Enumeration Types
  - New "types" and integer constant names
    ```C
    enum rps {
      ROCK,     // ROCK      = 0 
      PAPER,    // PAPER     = 1
      SCISSORS  // SCISSORS  = 2
    };
    enum coin {
      HEAD,     // HEAD      = 0
      TAIL      // TAIL      = 1
    };

    enum rps a = PAPER; // a = 1
    enum coin c = HEAD; // c = 0
    ```
  - "Types" are simply `int`, are mixable and not checked
  - Practically useful for meaningful names only

### Union Types
  - Overlapping "fields" that share the same space
    ```C
    union myUnion { // sizeof(myUnion) = "largest field size" = 4 (for this example)
      unsigned short s;
      unsigned int i;
      unsigned char b[4];
    };
    union myUnion u; // can use u.s, u.i, u.b[j] etc.
    ```
  - Use Cases: 
    - High-level: Data has multiple mutually exclusive cases
    - Low-level: 
      1. Store an `int` in `i`
      2. Read `b[0]` to `b[3]` to discover how `i` splits

### Tagged Union Idiom
  - Example: Suppose you wanted an array that holds **_both_** `int` and `double`
  - Idiom: Make an outer `struct`
    ```C
    struct int_or_double {
      enum { INT, DOUBLE } tag; // remembers which case you are in
      union {                   // shares the same space in memory regardless of int or double
        int i;                  // case value is an int
        double d;               // case value is a double
      } data;
    };
    struct int_or_double a[10]; // as wanted
    ```
### Type Alias with `typedef`
  - Very general, i.e can use with `struct`, `enum`, `int`, `double` etc.
    ```C
    typedef struct node {
      int i;
      struct node *next;
    } nodetype; // use `nodetype` instead of `struct node`
    ```
  - Cannot use the same `typedef` name for more than one thing
    ```C
    typedef coin { HEAD, TAIL } coin;
    typedef int coin; // illegal since `typedef coin` is already defined
    ```

{{< katex >}}
{{</ katex >}}