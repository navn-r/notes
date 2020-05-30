---
title: Week 3
created: '2020-05-26T01:27:05.962Z'
modified: '2020-06-02T17:42:22.081Z'
weight: 3
---

# Week 3

## Introduction to Shell
> More features available from `man sh`

### Simple Commands

4 general cases of commands

1. Built in commands: `cd`, `ls` etc.
2. User defined functions
2. Aliases eg. `alias pls="sudo"`
3. Commands that refer to the program name
    - eg. `tr` command runs the `tr` program

### Sequential List

Multi-line commands in a single line
```bash
$ cd projects ; sort file1 | uniq # runs cd then runs sort with pipe
# equivalent to
$ cd projects
projects$ sort file1 | uniq
```
- Grouping:
  - Explicit: `{grep foo file1 ; ls ; } > file2`

### Exit Codes

0 for success, non-0 for failure
```bash
$ sort file1    # assume this works
$ echo $?       # prints `?` command
0
$ cd asdgkj
-bash: cd: asdgkj: No such file or directory
$ echo $?
127             # non-zero -> fail
```

### Logical AND OR NOT

```bash
$ mkdir foo && cd foo               # sequential unless one fails
$ mkdir foo1 || mkdir foo2 || exit  # runs sequential command if first one fails
$ ! mkdir foo                       # trivial (true to false, v.v)
```

Operator Precedence:
  - `|` _highest_
  - `!`
  - `&&` `||` - same precedence
  -  `;` _lowest_ 

### Boolean Conditions

- String comparisons: 
  ```sh
  [ s1 = s2 ] # `!=`, `<`, `>` also work
  ```
- Number comparisons:
  ```sh
  [ num1 -eq num2 ] # `-ne`,`-gt`,`-ge`,`-lt`,`-le` also work
  ```
- Logical connectives
  - `-a`: _and_
  - `-o`: _or_
  - `-e`: _not_

### Conditionals and Iteratives

  - `if` statement:
    ```sh
    if list1 ; then
      list2
    elif list 3 ; then
      list4
    else
      list5
    fi # end statement
    ```
  - `while` loop:
    ```sh
    while list1 ; do
      list2 
    done # end statement
    ```
  - `for` loop:
    ```sh
    for var in word1 word2 word3 ; do
      echo $var
      mkdir $var
    done # end statement
    ```

### Filename Patters

  - `*`: any string
  - `?`: matches one character
  - `[nav]`: matches 'n', 'a', 'v'
  - `[0-9]`: matches digit
  - `[!0-9]`: matches non-digit

eg.
```bash
for i in *.sh ; do # for all files in dir with ext .sh
  echo $i          # echo its name
done
```

### Escaping and Quoting

  - `\`: _backslash_
  - `''`: _single quotes_
  - `""`: _double quotes_

Use Cases:
  - Spaced files: `cd "Onedrive - University of Toronto"`
  - Regex for `grep`: `grep ’<[a-z]*>’ *.html`
  - Operators in test commands: `[ ! ’(’ aaa ’<’ abc -o aaa ’>’ abc ’)’ ]`

### Variables
  
  - Declaration: 
    ```bash
    var=69 
    # no spaces
    # if uninit, `echo $var` returns empty string (`echo ${var}` also works)
    # uninitialized var is not neccessarily an empty string by default
    ```
  - Double Quotes
    ```bash
    $ f=`Stupid Example.js`
    $ ls $f   # equiv to `ls Stupid ; ls Example.js`
    $ ls "$f" # equiv to `ls "Stupid Example.js"`
    # double quotes and variables($) are 'special'
    ```
  
### String Testing
  
  - Non-Empty String: `[ -n string ]`
  - Empty String: `[ -z string ]`
  - **_Tricky:_**
    ```bash
    # suppose {V} is an empty string
    [ -n $V ]   # doesnt work (interpreted as `[ -n ]`)
    [ -n "$V" ] # works!
    ```

### Case Matching

eg.
```bash
case "$var" in
  *.py)                   # Case python 
    rm "$var"           
    ;;
  *.c | *.sh | myscript)  # Case C, Shell, myscript
    echo w00t "$var"
    ;;
  *)                      # else
    echo meh "$var"
esac # end statement
```
    







