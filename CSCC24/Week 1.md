---
title: Week 1
weight: 1
---

# Week 1

## Translation

> The process of converting high-level source code into machine code.

A Programming Language (PL) is **_neither_** compiled or interpreted, it's **implementation** *can* be.

### Types of Translation

- **Compilation**: translated *before* execution
  
  ```
  [Source Code] -> {Compiler} -> (Target Code)
  ```
  
  - Target code optimises for speed and security verification (potential memory leaks) *for the specific machine*
    
    - 'Heavy' information (i.e. variable types) only needed during compilation
    
  - Can expose potential errors/bugs before run-time
    
  - `C`, `Rust` are complied languages
    
    
  
- **Interpretation**: translated **_line by line_**
  
  ```
  [Source Code] -> (Interpreter)
  ```
  - Useful for prototyping, scripting
  
  - Easier to debug; stack trace uses the source code rather than binaries
  
  - More portable; only requires an interpreter
  
  - `Python`, `JavaScript`, `Shell` are interpreted languages
  
  
  
- **Pseudo-Compilation**: hybrid of compilation and interpretation
  
  ```
  [Source Code] -> {Compiler} -> [(Intermediate Code)] -> (Interpreter)
  ```
  - Compiler translate all of the source code before execution but into Intermediate Code
  
  - Interpreter executes the Intermediate Code line by line
  
  - Intermediate Code can be executed on any machine with the interpreter
  
  - `Java` is a hybrid language, it's intermediate code is *bytecode*



### The Process of Translation

1. **Lexical Analysis**: 
  
   - Creates a sequence of **tokens**; the *smallest meaningful element* (language specific)
  
2. **Syntactic Analysis**

   - Structures tokens into an **Initial Parse Tree**
     - internal nodes are syntax rules, leaves are tokens
     - Rules are determined by a **Precedence Table**
     
   - Where syntax errors are discovered

3. **Symantic Analysis**
   -  Annotates parse tree with semantic actions
4. **Code Generation**
   - Produces machine code