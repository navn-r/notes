---
slug: "final-review"
title: "Final Review"
---
# CSCA48 - Final Review
#### Winter 2020

> If you think it won't work in C, it probably will.

## Units 1 + 2:

- ### Variables and lockers:

    > Uniquely numbered in increasing order, reserved _**only**_ for the program using the box to store and access information in said box.

    #### Three ways to get a locker:
    1. Variable Declaration 
        ```C
        int boostMyMark = 420; // locker has been created
        ```
    2. Return Values
        ```C
        return boostMyMark; // new locker has been created, (copied value)
        ```
    3. Input Parameters
        ```C
        void killAverage(double currentAvg, double midtermMark) {...} // lockers created to store currentAvg, midtermMark
        ```
- ### Arrays:

    > _**Fixed**_ length and data type, _**consecutive**_ boxes allocated in memory and are **Passed-By-Reference** for function calls.
    
    #### Array Declaration:
    ```C
    int crunchyArray[10]; // creates 10 consecutive boxes of memory
    ```

    #### Strings: 

    - Array of chars
    - End-of-string delimiter: '\0'

- ### Pointers: 

    > Just a variable (own space in memory), and stores a _**memory address**_ in its locker of the _same_ data type.
    
    Example:
    ```C
    int num = 69; // creates locker
    int *p = &num; // the contents of (*) p gets the address of (&) num
    *(p) ++; // increments the contents of (*) p
    ```

    #### Arrays and Pointers:

    - Arrays get passed as a pointer in a function (index 0)
        ```C
        int arr[3] = {1, 2, 3}; // 3 new consecutive lockers for arr
        int *p = NULL; // empty pointer
        // the following are equivalent
        p = &arr[0]; // p gets the address of the value of arr at index 0
        p = arr; // p gets the start of arr
        ```
    - Arrays can be iterated using an offset pointer _**or**_ the indicies
        ```C
        int nums[5] = {1, 2, 3, 4, 5};
        int *p = nums; // &nums[0]
        for(int i = 0; i < 3; i++) {
            // the following are equivalent
            printf("%d\n", nums[i]); // nums at index i
            printf("%d\n", *(p + i)); // the contents of memory address (p + i)
        } 
        ```


## Unit 3:

- ### Dynamic Memory Allocation

    > Reserving space in memory so it **_doesn't get released_** when your function exits

    #### Initializing on the stack versus the heap:
    ```C
    int crunchyFunction() {
        int stackNum = 69420; // allocates memory on the stack
        int *heapNum = (int *)calloc(1, sizeOf(int)); // allocates on the heap
        return 0;
    }
    ```

    _After the function exits, `stackNum` will be released from memory **but** `heapNum` must be freed by the user_ 

    ```C
    free(heapNum);
    ```

    #### Malloc:
    
    - Does not clean up, but is faster than calloc, just be careful.
        ```C
        Type *type_name = (Type *)malloc(numOfElements, sizeOf(Type));
        free(type_name); //same as calloc
        ```

- ### Dynamic Arrays
  
    > Taking the best parts of Arrays ($O(1)$ lookup, consecutive boxes in memory) but **_without_** fixed length constraint.

    _Essentially, when an array of size $N$ is at capacity (check using a counter variable), create a new array of size $2N$, and copy existing elements over to the new array_ 

    ```C
    int *infiniteChocolateCopy(int someChocolate[n], int n) {
        int *moreChocolate = (int *)calloc(2 * n, sizeOf(int)); // allocates size 2n on the heap
        for(int i = 0; i < n; i++) { //iterates through someChocolate 
            *(moreChcolate + i) = someChocolate[i]; //copies over
        } 
        return moreChocolate;
    }
    ```

- ### Compound Data Types

    > Storing **_multiple components_** of data types (primitive and/or compound), packaged into a _single_ container. Used when storing information too complex for a single data type.

    #### Defining a CDT struct:
    ```C
    typedef struct student_struct { // creates the struct
        char name[1024];
        int year;
        double gpa;
        Markbook *marks; // example of passing a CDT pointer (CDT-ception)
        // as many as you want here
    } Student;
    ```
    
    #### Initializing a CDT:
    ```C
    Student *sweaty_nerd = (Student *)calloc(1, sizeOf(Student)); // allocate memory
    strcpy(sweaty_nerd->name, "TryHard on Piazza"); // for strings
    sweaty_nerd->year = 2023; // arrow (->) operator for pointers
    sweaty_nerd->gpa = 2.718;
    sweaty_nerd->marks = bad_marks; 
    ```

    #### Memory Model:
    - CDT's get one locker for all of it's contents (like a Bento Box)
    - Passing a CDT into a function:
        ```C
        myCDT randomCrunchyCDTFunction(myCDT crunch) { // makes a copy (not passed-by-ref)
            ...
            return crunch //returns a copy 
        } 
        ```
        _It is typical to pass and return CDTs as a pointer, instead of copying_ 

- ### Abstract Data Types

    > Implementation **_independent!_** It is just the idea of a container that stores a collection of data.

    #### List ADT: 

    - Stores items sequentially in nodes not necessarily consecutive, containing a reference to the next node in the list
    - Not fixed in length
    - Head: 1st node in list
    - Tail: last node in list
    - Typical operations, $O(N)$ worst case:
        - Insert
        - Remove
        - Update
        - Search
    - Queue ADT: FIFO (first-in-first-out)
    - Stack ADT: FILO (first-in-last-out)

    #### Linked List:

    > Best used when data is added and queried in random order.

    - Basic dynamic implementation of List ADT
        ```C
        typedef struct linked_list_struct {
            int data // payload (can be any type)
            struct linked_list_struct *next; // pointer to the next node in the list, NULL if tail node
        } ListNode;
        ```
    - Iterating through a Linked List

        ```C
        for(ListNode *n = head; n != NULL; n = n->next);
        ```
    - Insert
        1. At head - $O(1)$ complexity:
            ```C
            ListNode *wantToInsert = (ListNode *)calloc(1, sizeOf(ListNode));
            wantToInsert->data = 51 // coincidentally the course midterm average
            wantToInsert->next = head; // where head is the head of the LL
            ```
        2. At tail - $O(N)$ complexity:
            ```C
            //same initialzation of wantToInsert as 1.
            ListNode *n = head;
            if(head != NULL) { // always check if head is not null to avoid errors
                for(; n->next != NULL; n = n->next); // traverses until at the tail
                n->next = wantToInsert; // sets the tail as wantToInsert
            }
            ```
        3. Somewhere in between
            ```C
            //same initialzation of wantToInsert as 1.
            ListNode *n = head;
            if(head != NULL) {
                for(; n->data != insertHereNum; n = n->next); // traverses list
                wantToInsert->next = n->next; // links wantToInsert to n->next
                n->next = wantToInsert; // links n to wantToInsert
            }
            ```

            **Note:** _When inserting inside the list, we would use a condition to compare unique identifiers so we know **exactly** where to insert. For this example we assumed all the numbers were distinct and we knew `insertHereNum`'s value was given and inside the Linked List._

    - Search - $O(N)$
        1. Traverse from head
        2. Check if current node is the desired node using a _compare_ operation/function (see Note)
        3. Return
            - A **pointer** of the desired node, user can modify the node and list
            - A **copy** of the data type (compound/primitive), user **_cannot_** modify the node and list

   - Delete - $O(N)$
        ```C
        ListNode *p1 = head;
        ListNode *p2 = NULL; // is always 1 node behind p1
        if(head != NULL) {
            for(; p1 != NULL && p1->data != wantedNum; p2 = p1, p1 = p1->next); //traverses through LL, one behind the other until wantedNode is found
            if(p1->data == head->data) p2 = head->next; // only if the wantedNode is the head
            else p2->next = p1->next; // all other cases, links the node before wantedNode with the node after
            free(p1); // releases the node from the list
        }
        ```

## Unit 4:

- ### Computational Complexity:
    > Measuring the **_amount of work done_** by an algorithm as a **function** of the _number of data items_ the algorithm is working on, and thus **_predicting_** how algorithms will preform against each other without having to test on large $N$ values.
    
    #### The Big $O$ Notation
        
    > Comparing algorithms in a machine and implementation dependent manner.

    - _Mathematically put,_
        $$
            f(x) = O(g(x)) \iff \exists c \in \mathbb{R}_{>0} \text{ such that for sufficiently large } x \text{, } \newline
            |f(x)| ≤ c \text{ } \cdotp{g(x)}, \quad x > x_0
        $$
        
        _$O(g(x))$ is the **smallest** function of $N$ that puts an **upper bound** on $x$_

    - Given a set of candidate algorithms, the **_fastest_** algorithm will have the **slowest growing** Big $O$ complexity.

    - _In terms of efficiency,_
        $$
            O(1) < O(log (N)) < O(N) < O(Nlog(N)) < O(N^2) < O(N^3) < O(2^N) < O(N!)
        $$
    
    #### Binary Search for Arrays
    
    - Array **_must be sorted_**
    - Complexity of $O(log_2(N))$
    - Procedure (`Pseudocode`):
        ```C
        int binarySearch(int wantedNum, int Array[]) {
            int middleNum = Array[floor(length / 2.0)]; // finds middle index (floor if odd length)
            if(wantedNum == middleNum) {
                return index(middleNum); // returns index of middleNum in Array
            } else if (wantedNum > middleNum) {
                binarySearch(wantedNum, Lower Half of Array); // all values greater than middleNum
            } else {
                binarySearch(wantedNum, Upper Half of Array); // all values less than middleNum
            }
        }
        ```

    #### Complexity of Linked Lists versus Arrays

    - All **_basic_** operations on a Linked List have _worst case_ $O(N)$ complexity.
    - Unsorted Array:
      - Search: 
        - Index lookup: $O(1)$
        - Linear search: $O(N)$
        - Sort:  make a **_reasonable assumption_** :^)
    - Sorted Array:
      - Search:
        - Index lookup: $O(1)$
        - Binary Search: $O(log(N))$

    #### The Consequence of Sorting

    - Bubble Sort:

        > Traverse the array and swap adjacent decreasing entries until the array is sorted.

        Worst-Case Complexity: $O(N^2)$
        ```C
        void notSoCrunchyBubbleSort(int array[], int N) {
              for(int i = 0; i < N; i++) { // N iterations
                for(int j = 0; j < N - 1; j++) { // N - 1 iterations
                    if(array[j] < array[j-1]) { // compares if the adj. entries are decreasing
                        swap(array[j], array[j+1]);
                    }
                }
            }
        }
        ```
    - Quick Sort (`qsort`):

        > Choose a **_random pivot_**, split elements into 2 arrays: values **less than** the pivot, and values **greater than or equal** to the pivot. Repeat until all sub-arrays have lengths ≤ 1. _Reconstruct the now sorted array._

        - Average-Case Complexity: $O(Nlog(N))$ (not so **_crunchy_**)
        - Worst-Case Complexity: $O(N^2)$ (first/last entry pivot $\rightarrow$ **_insertion sort_**)
    
    - Insertion Sort:

        > Build the sorted array in place (no splits), shifting elements as we traverse the array to sort.

        - Best-Case Complexity: $O(N)$
        - Worst-Case Complexity: $O(N^2)$
        - Procedure:
            1. Choose **first element** to be "sorted"
            2. Look at next element in array, and insert it **_inside_** the "sorted" portion by comparing it to the values in said portion.
            3. Repeat 2. until the end of array

- ### Trees

    > A **_generalization_** of Linked Lists, linking one node in the Tree to sucessor (children) nodes. Recursive in nature, each sub-tree is also a Tree.

    #### Binary-Search Trees (BST)

    >Variation of a Binary Tree (left and right children only) such that the BST property holds for each node and duplicate nodes **_are not allowed_**.

    - BST Property:
      - Data in nodes on the _left_ sub-tree are **_less than or equal to_** data in the root node
      - Data in nodes on the _right_ sub-tree are **_greater than_** data in the root node

    - Basic Implementation 

        ```C
        typedef struct dollarStoreChocolate_BST_Struct {
            int data; 
            struct dollarStoreChocolate_BST_Struct *left; // left child pointer
            struct dollarStoreChocolate_BST_Struct *right; // right child pointer
        } BST_Node
        ```

    - Search - $O(log(N))$:
        ```C
        if(root == NULL)
            return NULL;
        else if(givenData == root->data) // check root
            return root; 
        else if(givenData <= root->data) 
            return search(root->left, givenData); // search left sub-tree
        else 
            return search(root->right, givenData); // search right sub-tree 
        ```
    
    - Insert - $O(log(N))$
        ```C
        // assuming new_node was initialized and correctly allocated to the heap
        if (root == NULL) // empty tree
            return new_node; // inserts at root
        else if (new_node->data > root->data)
            root->right = insert(root->right, new_node); // inserts in right sub-tree
        else
            root->left = insert(root->left, new_node); // inserts in left sub-tree
        return root;
        ```

        **Note:** _Just like Linked Lists, we must use our own comparison function if the data in the node is a CDT._

    - Traversal - $O(N)$
      - In Order - For listing a BST in sorted order:
        ```C
        if (root != NULL) {
            inOrder(root->left); // traverses left sub-tree
            printf("%d\n", root->data); // any operation can be preformed
            inOrder(root->right); // traverses right sub-tree
        }
        ```
      - Pre Order - For Copying an entire BST:
        ```C
        if (root != NULL) {
            printf("%d\n", root->data); // operation
            preOrder(root->left); // traverses left sub-tree
            preOrder(root->right); // traverses right sub-tree
        }
        ```
      - Post Order - For deleting an entire BST:
        ```C
        if (root != NULL) {
            inOrder(root->left); // traverses left sub-tree
            inOrder(root->right); // traverses right sub-tree
            printf("%d\n", root->data); // operation
        }
        ```
    
    - Delete - $O(log(N))$

      - Searching for node to delete is the same implementation as Insert and Search
      - No children:
        ```C 
        free(root); // if only the final exam was as simple as this
        ```
      - One Child:
        - Left only
            ```C
            BST_Node *temp = root->left; // points to left child
            free(root);
            return temp;
            ```
        - Right only
            ```C
            BST_Node *temp = root->right; // points to right child
            free(root);
            return temp;
            ```
      - Two Children:
        ```C
        BST_Node *temp = find_successor(root->right); // smallest node in the right subtree
        copyNode(root, temp); // copies all the data from temp to the root
        root->right = delete(temp); // deletes the successor recursively
        return root;
        ```

    #### Complexity of Binary Search Trees versus Linked Lists and Arrays

    - Building the data structure:
        - **LL:** $O(N)$
        - BST: $O(Nlog(N))$
        - Array + Merge Sort: $O(Nlog(N))$

    - Search:
        - LL / Unsorted Array: $O(N)$
        - **BST:** $O(Log(N))$
        - **Sorted Array:** $O(Log(N))$

    - Space Complexity:
      - Array: Fixed size
      - **LL:** $O(N)$
      - **BST:** $O(N)$


## Unit 5:

 - ### Graphs:
    
    > A model to represent **_items and their relationships_** between them. Composed of Nodes (Verticies) and Edges, with optional direction and weight.

    $$
        G=(V,E)
    $$
    #### Graph Representation

    - Direction:
      - Undirected: Two way relationship
      - Directed: One way relationship

    - Neighbourhood: 

        > The neighbourhood of Node U is __*the set of all nodes*__ that are neighbours of Node U.

      - Neighbour: 2 Nodes are considered neighbours if they are joined by an edge
        
        Directed Example: Node U $\rightarrow$ Node V

        - In-neighbour: U of V
        - Out-neightbour: V of U

        - In-neighbourhood: Edges **_arriving_** at Node U
        - Out-neighbourhood: Edges **_leaving_** Node U

     

    - Degree:

        > The size/dimension (**_number of nodes_**) in the neigbourhood of the Node.

        - In-degree: Degree of In-neighbourhood
        - Out-degree: Degree of Out-neighbourhood

    - Traversals:
      - Breadth First Search (BFS) - By path of neighbours
      - Depth First Search (DFS) - Level by level


    #### General Applications of Graphs

    - Social Networks

    - Transportation (Maps services)
    - Genomics and Bioinformatics
    - Computer Networks and the Internet

    #### Representing Graphs

    - Adjacency List:
        > An array with **_one entry per node_**. Each entry points to a **_linked list_** containing the **neigbourhood** for that node.

    - Adjacency Matrix:
        > A 2D $N\text{ x }N$ **_matrix_**, $N$ is the number of nodes in the graph. If Node $i$ and Node $j$ share an edge then AdjMat[$i$][$j$] > 0. For Undirected graphs, AdjMat[$i$][$j$] $=$ AdjMat[$j$][$i$]

    #### Operation Complexity on Graphs

    > $N = |\text{Vertices}|$ is the number of nodes in the graph, and $M= |\text{Edges}|$ is the number of edges in the graph

    <center>

    Operation | Adjacency List | Adjacency Matrix
    --- | --- | ---
    Edge Query |$O(N)$ | $O(1)$
    Inserting a Node | $O(1)$ | $O(N^2)$
    Removing a Node | $O(M)$ | $O(N^2)$
    Inserting an edge | $O(1)$ | $O(1)$
    Removing an edge | $O(N)$ | $O(1)$

    </center>

  - ### Principles of Recursion:

    > The repeated application of a **_recursive_** procedure. Can make some problems **_super trivial_** to solve.

    #### Types of problems that benefit from recursion:

      - Sudoku, N Queens (pretty **_c r u n c h y_**)
      - BST Operations (duh)
      - Graph Operations (every sub-graph is still a graph)
      - Search and Path Finding (BFS and DFS)
       > _Generally any problem that contains **_a smaller version_** of itself as a sub-problem._

    #### The process of designing a recursive solution

      1. Base Case(s):
          - Specific to the problem itself, multiple can exist
          - The smallest problem with a **_trivial solution_**
          - Any solution must always reach the base case
      2. Recursive Case:
          - Multiple ways to split the problem at hand, some better than others
          - After each recursive call, the sub-problem must be closer to the base case
          - Always best to visually interpret/draw out the solution

    #### Recursive Sort and their complexities

     - `DoofusSort/chewySort/notAProGamerSort`:
        
        1. First Entry goes into one sub-array, the rest into another
        2. Recursively sort each sub-array
        3. Merge to form the sorted array

        Complexity: 
        
        - Worst Case: $O(N^2)$ (Essentially Insertion Sort) 

    - Merge Sort:

        1. Choose the _middle_ entry as pivot
        2. Split into 2 sub-arrays, one less than, one greater than or equal
        3. Recursively sort each sub-array
        4. Combine to form the sorted array

        Complexity:

        - Worst Case: $O(Nlog(N))$ (Super Duper **_Crunchy_**)

    - `qsort`:

        See: [The Consequences of Sorting](#the-consequence-of-sorting)

    #### The Memory Model

    > Each time a recursive call has been made, part of the stack is reserved (stack frame) for variables, parameters and return type. Each stack frame **_will only be cleared_** until each recursive call is completed.

     - Stack Overflow (like the webpage): 
       - Results when the stack has ran out of empty space
       - Recursive solution must reach the base case in a **_reasonable_** number of steps
       - Tail Recursion can prevent this
         - The recursive call would be the last thing the function does before returning
         - Similar to an iterative solution, better for **_DEEP_** recursive solutions

## Unit 6:

  - ### Software Design:

    > Developers are lazy, we need modular solutions to be **_as useful for others as possible._**

    #### Properties of good software design

    1. Modularity
       - Does **one** thing, **_ridonkulously_** well.
       - Minimizes code replication
       - Simplifies testing and debugging

    2. Reusablity
        - Any module can be reused by other applications requiring **_that specific task._**

    3. Extendibility 
        - Software that is **_easy to improve_** and extend its **functionality** and **usability.**
    
    4. Maintainablity
       - **Organized, explicit** and _so_ **_well-commented_** that a noob would understand it.

    5. Correctness
       - Devs may be lazy, but they aren't _super_ lazy. Include **_detailed documentation_** and test cases.

    6. Efficiency
        - Gotta go **_fast_**, $O(1)$ or bust.
    
    7. Openness
        - You're not a giant tech company, contribute to the **_open source software_** community.

    8. Privacy and Security
        - **Secure** data exchanges and **reliable and safe** solutions over personal/enterprise networks.
    
    <br>
  - ### Application Programming Interfaces (APIs)
    
    > Interacting with software modules, without having to understand how they work internally.

    - Use-case: _Specific_ situation in which components of the API may be used
    - Dependency: The relationship of one module requiring **_another module_** in order to function

    #### Properties of a good API

    - Easy to maintain
    - Easy to extend and improve
    - Easy to learn
    - Difficult to use incorrectly
    - Suitable for those who will be using it

    #### Expanding an API

    - Must consider required use-cases for each module
    - Applications that rely on the API must _not_ experience any significant impacts if expanded
      - Method overloading is a good workaround for any **_CRUNCHY_** use-cases after the initial rollout
    - If a bad API is expanded/modified, any software relying on said API will most likely **_stop functioning correctly_**

    #### Limitations of C

    > _Basically C is !(OOP)._

    - No Encapsulation and Information hiding
    - No Polymorphism or Inheritance
    - No Method overloading

    <br>
  - ### Object Oriented Programming (OOP)
    > Model for developing software components based on **_Encapsulation._**

    - Encapsulation: 
      - Wrapping required components together to implment the functionality of a data type.
      - **_Hides_** data and functionality from the user using Access Control Modifiers to prevent misuse.
    - Access Control Modifiers:
      - Private: Accessed **_only_** by the class that its in
        - Protected: Gives inherited classes **_access_** to the _parent_ class's **private data**
      - Public: Can be accessed by any code **_outside_** of the class


    #### Method Overloading
    > Methods with the **_same name_**, but with **different** input parameters, return type and implementation.

    - Useful for multiple use-cases in which that particular solution is needed

        Example (Java):

        ```Java
        public int addNumbers(int a, int b) {
            return a + b;
        }

        // same name but different return type and input parameters
        public double addNumbers(double a, double b, double c) { 
            return a + b + c;
        }
        ```

        _



    #### Classes and Objects
    > The **_template/blueprint_** for building objects and their variables and methods.

    - Components of a class:
      - Private member variables
        - Used **_inside_** the class only
      - Constructor
        - Creates **_new_** instances/objects
        - Assigns values to the input paramenters for the instance
      - Destructor
        - Called when **_finished_** with the class
      - Object Methods
        - Any functions declared in the class that an object can **_use_**

    An example of an object class (Java):
    ```Java
    class ComputerScienceNerd { // initialize the class
        // initializes member variables for ComputerScienceNerd object
        private String name;
        private double gpa;

        public ComputerScienceNerd(String name, double gpa) { // object constructor
            // private variables (this.) get the parameter values
            this.name = name; 
            this.gpa = gpa;
        }

        /* 
        * Object methods, public in nature
        * Call the method with (ComputerScienceNerdObject).method();
        */

        String getName() {
            return name;
        }

        double getGPA() {
            return gpa;
        }

        void screech() {
            System.out.println("bY ThE WaY, dId yOu kNoW I'm iN CoMpUtEr sCiEnCe?");
        }
    }
    ```

    #### Polymorphism and Inheritance

    - Inheritance:
        > The idea of passing methods and definitions to a hierarchy of **_Children_** classes.
      - A Child class _inherits_ the same "traits" as the Parent class
      - Example:
        - Parent Class: `Instrument`
        - Child Class: `WindInstrument` __inherits__ `Instrument`
        - Child Class: `Flute` __inherits__ `WindInstrument`
      - The "is a(n)" Test implies possible inheritance
        - [x] "Flute is an instrument" $\rightarrow$ `Flute` can _inherit_ `Instrument`

    - Polymorphism:
        > The idea that **_similar classes_** should be used the same way. Different objects belonging to the same inheritence hierarchy may posses the same functions, but the behaviour is **_characteristic_** to the object itself.

        Example (`Pseudocode`):
        ```Java
        Class Instrument {
            Instrument() {
                double duration;
                double freq;
            }
            playNote() {
                play(duration, freq); // generic implementation set by Parent class
            }
        }
        Class Guitar extends Instrument { // child of Instrument
            Guitar() { 
                Instrument(); // uses the same constructor as parent class
            }
            playNote() { // overrides the playNote() from the parent
                // implementation here is specific to the Guitar class
            }
        }
        Class Piano extends Instrument { // child of Instrument
            Piano() {
                Instrument(); // uses parent constructor
            }
            playNote() { // overrides the playNote() from the parent
                // implementation here is specific to the Piano class
            }
        }
        ```

        _Obviously, a Guitar and a Piano **do not** sound the same (unless you're deaf). As a result, `playNote()` should be modified for each different sounding instrument. The idea/functionality of `playNote()` **must be the same,** regardless of which Child is calling it._

    #### Abstract Classes
    
    > A class that **_only declares_** methods, and any subclass must provide implmentations for each method.

    - A Parent class that lets its children hold all implementations
    - Example:
      - Parent Class: `Instrument`
        - Methods: `playNote(), tunePitch(), smashOnSomeonesHead()`
        - No implementations
      - Children Classes: `Piano, Guitar`
        - Methods: Same as parent **_but_** each child has a different implementation
          - `Piano: smashOnSomeonesHead()` $\rightarrow$ `"Smash Head on Piano"`
          - `Guitar: smashOnSomeonesHead()` $\rightarrow$ `"Smash Guitar on Head"`



{{< katex >}}
{{</ katex >}}