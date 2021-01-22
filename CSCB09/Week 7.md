---
title: Week 7
created: '2020-07-07T19:10:18.491Z'
modified: '2020-07-08T17:41:52.862Z'
weight: 7
---

# Week 7

## Programming in `C` (cont.)

### Compiler and Linker Stages

`myProgram.c` $\to$ **Compiler** $\to$ **Machine Code** (`myProgram.o`) $\to$ Libraries + **Linker** $\to$ **Executable** 

- `myProgram.o` is the *object code file*
- Libraries: where methods from `stdio.h/stdlib.h` come from
- Linker: Merges object files and libraries into one executable
  - `gcc` serves as a convenient linker frontend

### `C` Compiler Stages

The `C` compiler futher breaks down into:
  1. Pre-processor: For `#` directives, determines the actual `C` code seen by the compiler proper
      ```bash
      $ gcc -E ... # to see what the pre-processor actually does
      ```
  2. Compiler proper: Translates `C` code into machine code
      ```bash
      $ gcc -c ... # creates the object file with the machine code
      ```

### Pre-processor Directives

  - `#define` macros
    - Textual substitution
        ```C
        #define FINAL_COURSE_MARK 100 // to define a macro
        #undef FINAL_COURSE_MARK      // to remove the macro
        ```
  - `#include` for header files
    - Header files usually contain
      - Macro defintions
      - Types of exported functions and global variables
    - Implementation
        ```C
        #include <foo.h> // looks for file in system-wide places ('/usr/include')
        #include "foo.h" // looks for file among user source code
        ```
  - Conditional Compilation
    ```C
    #ifdef DEBUG_FLAG
      fprintf(stderr, "x=%d\n", x);
    #endif
    ```
    - Common technique for debugging code 
    - Also useful to check current OS (Windows/Linux)

### Modularity and Seperate Compilation
  
  - Keep closely-related code in the same files 
  - Key idea/principle for good software design
  - Easier to modify and recompile one small file then one large file
  - Example:
    ```C
    // Rectangle Struct with area function
    typedef struct rect_struct {
      double width, height;
    } rect;
    double calcArea(const rect *r) {
      return ... //implementation goes here
    }

    // Linked-List Struct 
    typedef struct ll_node_struct {
      rect r;
      ll_node_struct *next;
    } listNode;

    // Main Program
    int main() {
      rect r;
      listNode *newNode = (listNode *)calloc(1, sizeof(listNode));
      printf("%d\n", calcArea(&r));
      ...
    }
    ```
      - This large file can be split into smaller more cohesive files
        - `rect.h`: includes type definition for `rect_struct` and `calcArea()` prototype
        - `rect.c`: includes `calcArea()` implementation/definition
        - `llnode.h`: includes `ll_node_struct` definition
        - `main.c`: includes the main program <br><br>
    ```bash
    # only compile to machine code if it is necessary (new changes)
    $ gcc -c rect.c
    $ ...
    # takes the machine code and makes the executable
    $ gcc rect.o bb.o mainprog.o -o mainprog
    ```

### Header Files
  - A seperate file *specifically* for macro/type definitions and function prototypes
  - Compiler wants to see the definition and prototype but you do not want to manually copy it to multiple files
  - **_Note:_** It is illegal to see a type definition twice when compiling 
    - Solution: Using Conditional Compilation to check if the header file was already included from another file, and including if it hasn't
      ```C
      #ifndef _FOO_H // if _FOO_H has been defined already, skip
      #define _FOO_H // if it has not been defined, then define it
      typedef struct node {
        int i;
        struct node *next;
      } node;
      #endif
      ```

## Makefiles and `make`

### Most Basic Clause/Rule
- Form:
  ```sh
  TARGET : PREREQUISITES
    RECIPE
  ```
- Example:
  ```sh
  bb.o : bb.c bb.h rect.h # if `bb.o` is missing or older than `bb.c`, `bb.h` and `rect.h`
    gcc -c bb.c           # then run this command (compile to machine code)
  ```

- If there are multiple rules in a Makefile:
  - `make` triggers the first rule (which may trigger other subsequent rules)
  - Order does not matter otherwise
  
- Customary to write first rule as
  ```sh
  all : myexe1 myexe2 myexe3  # triggers other rules to build each myexe file
  .PHONY : all                # means `all` is just a label, not an actual target file
  ```

### File Clean Up
- Customary to add
  ```sh
  clean :         # no prerequisites
    rm -f *.o myexe1 myexe2 myexe3
  .PHONY : clean 
  ```
- Run `make clean` to invoke this target rule

### Variables
  - Defining variables from within a Makefile
    ```sh
    CFLAGS = -g
    ```
  - Setting variables outside of a Makefile
    ```sh
    make CFLAGS='-g -DMY_DEBUG_FLAG' # overrides CFLAGS from within Makefile
    ```
  - Using a variable
    ```sh
    gcc $(CFLAGS) -c bb.c
    ```

### Automatic Variables and Pattern Rules 

  - Non-example
    ```sh
    mainprog : mainprog.o bb.o rect.o
      gcc -g mainprog.o bb.o rect.o \
          -o mainprog

    mainprog.o : mainprog.c bb.h rect.h
      gcc -g -c mainprog.c

    bb.o : bb.c bb.h rect.h
      gcc -g -c bb.c

    rect.o : rect.c rect.h
      gcc -g -c rect.c
    ```

  - Using pattern rules
    ```sh
    mainprog : mainprog.o bb.o rect.o
      gcc -g $^ # all prereqs 
          -o $@ # target
    
    %.o : %.c       # any `.o` target with `.c` prereq
      gcc -g -c $<  # build the first prereq
    ```

  - Automatic prerequisite listing
    ```sh
    $ gcc -MM mainprog.c bb.c rect.c
    mainprog.o : mainprog.c rect.h bb.h
    bb.o : bb.c rect.h bb.h
    rect.o : rect.c rect.h
    # copy the output to your Makefile
    ```