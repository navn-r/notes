---
title: Week 2
created: '2020-05-20T14:27:39.844Z'
modified: '2020-05-26T17:52:24.527Z'
weight: 2
---

# Week 2

## File Management
> How we survived without File Explorer

### Directory Tree
```
/
├── Applications
├── Library
├── System
├── *Users*
├── Volumes
├── bin
├── cores
├── *dev*
├── etc -> private/etc
├── *home* -> /System/Volumes/Data/home
├── opt
├── private
├── sbin
├── tmp -> private/tmp
├── *usr*
└── var -> private/var
```


### Paths

- Absolute Path: _from root dir_
  - eg. `/Users/home/projects/course-notes/index.html`

- Relative Path: _from current dir_
  - eg. `course-notes/index.html` (if current dir is `/Users/home/projects/`)
  
- Parent Directory: `..`

- Directory Itself: `.`
  - Some commands require the dir name, `.` simplifies this task


### Commands

  - `pwd`: _print working directory_

  - `cd`: _change directory_

  - `ls`: _list_
    - `-a || -A`: include dotfiles (`.` for `-a`, `.. && .` for `-A`)
    - `-l`: include more information (size, modification time, etc.)
    - `-d`: include _only_ directories
    - `-t`: sort by modification time
    - `-r`: reverse order
    - `-R`: **_recursive_** list, whole subtree

  - `mkdir`: _make directory_

  - `cp`: _copy_
    - `-R`: **_recursive_** copy, can possibly overwrite

  - `mv`: _move_
    - Can possibly replace existing files

  - `rmdir`: _remove dir_
    -  Precondition: the current dir is empty

  - `rm`: _remove/delete file(s)_
    - `-r`: **_recursive_** delete, used for non-empty directories
    - No "Recycle Bin"/Trash


## Core Utilities
  
  - `cat`: _concatenation_
    - Copy files and/or `stdin` to `stdout`
    - eg.
      ```bash
      $ cat file1 file2 
      ... # dumps file1
      ... # dumps file2
      $ cat file1 - file2
      ... # dumps file1
      ... # user input as stdin, use ctrl+D to exit and continue
      ... # dumps file2
      $ sort myFile | uniq | cat file1 - file2
      ... # dumps file1
      ... # dumps myFile after sorting and uniq
      ... # dumps file2
      ```

  - `head` and `tail`:
    - `head` starts from start of file, `tail` starts from last
    - eg.
      ```bash
      $ head -3     # output first 3 lines
      $ head -n -3  # output all except last 3 lines
      $ tail -3     # output last 3 lines
      $ tail -n +3  # output starting from and including line 3
      ```

  - `wc`: _word count_
    - Can count by words (`-w`), bytes (`-c`), lines (`-l`), characters (`-m`)
    - eg.
      ```bash
      $ wc file1
      2 6 27 file1 # 2 lines, 6 words, 27 bytes
      ```

  - `sort`:
    - Sort, check if sorted, merge sorted
    - Default sort by whole-line
    - eg.
      ```bash
      # sample input
      Navinn CS 420
      UofT  Expensive 69
      GiveJobPls iNeedJob 180

      $ sort -b -k 3,3n       # sort by key=3rd field, parse 3rd field as number
      UofT Expensive 69       # lowest number
      GiveJobPls iNeedJob 180
      Navinn CS 420           # greatest number
      ```

  - `tr`: _translate_
    - Similar to a `String.replace()` method

    - eg. 
      ```bash
      $ tr 12 ab        # replace '1' with 'a', '2' with 'b'
      $ tr 1234 '[a*]'  # replace  '1','2','3','4' with 'a'
      $ tr A-Z a-z      # replace Uppercase with lowercase
      ```
    - `-c`: compliment
      - eg.
        ```bash
        $ tr -c 1234 '[a*]' # replace everything except '1','2','3','4' with 'a'
        ```

    - `-s`: squeeze
      - Replace consecutive occurences by a single occurrence
      - eg.
        ```bash
        $ tr -s 12              # can convert '11122abc211' to '12abc21'
        $ tr -s 12 abc          # replaces then squeezes
        $ tr -cs 0-9a-z ’[\n*]’ # replace everything except digits and letters with line breaks
        # then squeezes multiple line breaks -> 'one word per line'
        ```
    - `-d`: delete
      - Remove occurence
      - eg.
        ```bash
        $ tr -d 12    # removes '1' and 2'
        $ tr -ds 12 3 # deletes '1','2' then squeezes '3'
        ```

  - `tee`: _T_
    - Branching out, extra pipe (T-shaped)
    - Copy `stdin` to `stdout`, and also copy to file (extra branch)
    - eg.
      ```bash
      sort | tee sortedfile | uniq
      # similar to sort | uniq 
      # but stdout from sort is now copied to sorted file before pipelining into uniq stdin
      ```

  - `yes`: 
    - Unending lines of `y`
    - Use case: if a program requires `[y/N]` a lot
    - `[expletive]`: outputs `expletive` instead of `y`














