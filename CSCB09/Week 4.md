---
title: Week 4
created: '2020-05-27T22:12:12.153Z'
modified: '2020-06-02T19:14:38.835Z'
weight: 4
---

# Week 4

## Shell Scripting

### Running Scripts
```bash
$ sh myscript
```
Alternatively, in your `myscript.sh` file, do the following
```bash
#! /bin/sh
chmod u+x myscript # sets 'executable' flag on the file
```
and run it with...
```bash
$ ./myscript
```

### Positional Parameters
```bash
$ ./myscript foo bar baz # come after filename, spaced
```
  - `$#`: Number of arguments (3)
  - `$n`: Parameter name (`n` is the number)
    - eg. `$1 = "foo"`
  - `$*`: One string with all parameter names
    - `"foo bar baz"`
  - `$@`: Comma seperated strings for each parameter name
    - `"foo", "bar", "baz"`
  - `shift`: Shifts Positional Parameters by 1
    ```bash
    $# = 2
    $1 = "bar"
    $2 = "baz"
    $* = "bar baz"
    # good for looping over parameters
    ```

### Example Script
```bash
#! /bin/sh
chmod u+x pydelete # sets exec flag on pydelete.sh

dryrun=
verbose=
while [ $# -gt 0 ]; do # while the number of params > 0
  case "$1" in # takes the first parameter/argument
    -n) # if the argument is -n
      dryrun=y # sets the program as a dryrun "doesnt actually delete files"
      ;;
    -v)
      verbose=y # sets the program as verbose "deletes verbosely"
      ;;
    *) # as soon as there is another different parameter, the while loop breaks
      break
  esac # end of case
  shift # basically $# -= 1
done # end of while

for f in "$@" ; do # for each parameter (after remove the arguments)
  case "$f" in # begins case
    *.py) # if the file is .py extension
      [ -n "$verbose" ] && echo "deleting $f" # if user used `-v`
      [ -z "$dryrun" ] && rm "$f" # if user did not use `-n`, then it will delete
      ;;
    *) # any other file extension
      [ -n "$verbose" ] && echo "not deleting $f" # if the user set verbose
  esac # end of case
done # end of for
```

### `exit`
  - Terminates script and/or shell process
  - Useful for:
    - Early exit
    - Non-zero exit codes: `exit 1`

### Functions

- Example definition
  ```bash
  myfunc() {
    echo "Hot diggity dog"
  }
  ```
- Example function call
  ```bash
  myfunc foo bar baz # positional parameters are now function arguments
  ```
- Returning
  ```bash
  return    # early return
  return 1  # with exit code
  ```
- Variable Scopes
  - Local variables
    ```bash
    myfunc() {
      local x y z
      x=69
      y=351
      z=$(expr $x + $y)
      echo $z
    }
    ```
  - Dynamic Scoping
    ```bash
    $ cat myscript
    x=0
    printnum() {
      local x
      echo $x
    }
    func1() {
      local x
      x=1
      printnum
    }
    func2() {
      local x
      x=2
      printnum
    }
    func1 ; func2
    $ sh myscript
    1 # from func1
    2 # from func2
    ```
### Feeding multi-line text into `stdin`
``` bash
$ cat << EOF # could be any string
﹥Hi I'm Navinn
﹥Variables also work \$x=$x
﹥EOF # tells shell its the end of file
Hi I'm Navinn
Variables also work $x=42069
```
- if end-marker is declared in quotes, `$` is no longer special.

### Command Substitution
- Run a command and take its `stdout` in-place
  ```bash
  for i in $(cat myfile) ; do 
  # if wrapped in double-quotes, `cat myfile` is just one string instead of multiple
    echo "$i"
  done
  ```
### Environment Variables
- Every process has a collection of environment variables
  - eg. `PATH, CLASSPATH, USER, PWD, PS1` etc.
  - Convention is all caps
  - Same syntax: eg. `$PATH, PATH=...`
- `printenv`: prints current env. vars.
  ```bash
  $ printenv
  HOME=/Users/home
  PWD=/Users/home/desktop
  PATH=/usr/local/cms/jdk1.8.0_31/bin:/usr/bin:/bin # colon-seperated list of directories
  ANDROID_HOME=/Users/home/Library/Android/sdk
  SHELL=/bin/bash
  ...
  ```
- Creating a new env. var. is _different_
  ```bash
  LOGNAME=navn          # same init
  export LOGNAME        # different
  export LOGNAME=navinn # alt
  ```

## File Attributes

### General Attributes
  - Size
  - Type
  - Last modified time
  - Last access time
  - Last change time
  - Owning user
  - Owning group
  ...

### Unix Account Organization
Two main accounts: `user`, `groups`
  - `user`: the user (`home`, `navinn` etc.)
  - `groups`: A user can be in multiple groups
    - `groups` command displays which groups the current user is in
    ```bash
    $ groups
    staff everyone localaccounts _appserverusr admin # ...
    ``` 
### Permission Flags
- Each file is assigned one owning user and one owning group. Default is the user who created it and their default group
    - Permissions: take the form `rwxrwxrwx`
      - `r`: read
      - `w`: write
      - `x`: execute
    - File permission notation
      ```bash
      $ ls -l
      rwxr-x--x 1 home  staff  73966  1 Jun 23:07  myscript.sh
      # perms.    user  group  bytes  date_created name
      # rwxr-x--x: user gets `rwx`, users in group get `x`, other users get `x` 
      ```
- For directories:
    - `r`:  list files in the dir `ls`
    - `w`:  add, delete, modify files in the dir
    - `x`: `cd` into dir., use paths mentioning dir

- To Check Perms on a file/dir:
  ```bash
  [ -r path ] # exists and readable by current user
  [ -w path ] # exists and writable
  [ -x path ] # exists and executable
  ```
- Changing Ownership/Permission
```bash
chown user path1 path2        # changes the owning user for both paths
chown user:group path1 path2  # changes owning user and owning group
chown :group path1 path2      # changes group
chmod u=rw,g=r,o= path1 path2 # changes specific perms (u=user, g=group, o=other)
```

### `find`
Literally finds a path and operates on selected files with automatic recursion
```bash
$ find path ...expression
```
_For each given path, recurse down and pick out a file based on the expression_

eg. Find python files, print their path, then delete
```bash
$ find . -name ’*.py’ -exec rm ’{}’ ’;’ -print
```
