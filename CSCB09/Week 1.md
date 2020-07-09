---
title: Week 1
created: '2020-05-12T22:33:31.899Z'
modified: '2020-05-20T14:33:50.599Z'
weight: 1
---

# Week 1

## Processes

> What happens when you run a program.

- Has Input and Output ends:
  - `stdin`: Standard Input - eg. Keyboard input goes through `stdin`
  - `stdout`: Standard Output - eg. Terminal output (printing to screen)

  eg. 
  ```bash
  Navinns-MacBook-Pro:~ home$ sort
  # OS connects keyboard to stdin
  Watermelon
  Apple
  Strawberry
  Mango
  ^D # 'exit' keybind, OS connects screen to stdout
  Apple
  Mango
  Strawberry
  Watermelon
  ```

## Pipelining and Redirection
> Routing outputs of one program **_to the input of another_**, and outputting to a file _instead of the screen_. 
  
  eg. 

  ```bash
  Navinns-MacBook-Pro:Desktop home$ cat myfile # screen is connected to stdout
  # starting text
  This
  this
  is
  is
  a
  file
  haha
  haha
  Navinns-MacBook-Pro:Desktop home$ cat myfile | sort | uniq > myNewFile && cat myNewFile
  # myfile -> stdin of cat => stdout -> stdin of sort => stdout -> stdin of uniq => stdout -> myNewFile
  This
  a
  file
  haha
  is
  this
  ```

## Scripting
> _Combining_ shell commands, instead of being redundant.

eg. 
  - Redundant commands:

    ```bash
    Navinns-MacBook-Pro:Desktop home$ cat mywords | sort | uniq > mywords-unique
    Navinns-MacBook-Pro:Desktop home$ cat yourwords | sort | uniq > yourwords -unique
    Navinns-MacBook-Pro:Desktop home$ cat badwords | sort | uniq > badwords -unique
    ```
  - Simple For-Loop script:

    ```bash
    for w in mywords yourwords badwords; do
      cat $w | sort | uniq > $w-unique
    done
    ```

## Devices and Services
> Presented as files in Unix.

  - Unix kernal _creates_ file names and _emulates_ file operations, known as 'Special Files'

  eg.

  ```bash
  /dev/sda # Hard disk as a special file, restricted
  /dev/zero # Endless stream of 0's as bytes

  # To wipe a hard disk clean:
  Navinns-MacBook-Pro:~ home$ dd bs=4K if=/dev/zero of=/dev/sda
  ```

## Terminology

  - Kernel: Decides which processes may run, and what it can access  

  - OS Processes: More services, features, and background monitoring _not_ in the kernal

  - Shell: Command-line user interface

  - User Processes: _Your_ processes

## The Unix Philosophy

  - Consisting of small programs, that do one thing **_really_** well  

  - Scripting + Pipelining versus rewriting _bigger_ programs for complex tasks

  - Recursive file-like structure

  - Short **_and_** Simple program names -  eg. `cp`: copies files


