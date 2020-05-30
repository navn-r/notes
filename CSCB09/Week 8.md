---
title: Week 8
created: '2020-07-18T17:23:58.032Z'
modified: '2020-07-18T20:09:18.284Z'
weight: 8
---

# Week 8

## File System with `C`

### i-nodes

- The file system has an array of 'i-nodes'

- Every file and directory is identified using an i-node

- An i-node stores:
  - Type: Regular file, dir, link, device, socket, etc.
  - Permissions
  - The owning user and owning group (numerical ids)
  - Size
  - Timestamps (created, last modified)
  - Which disk blocks are used
  - Other metadata

- You can get most metadata by using the `stat` command
  ```sh
  Desktop ~$ stat
  2942805567 687 crw--w---- 1 home tty 268435456 0 "Jul 18 13:29:59 2020" "Jul 18 13:29:59 2020" "Jul 18 13:29:59 2020" "Dec 31 19:00:00 1969" 131072 0 0 (stdin)
  ```

### Directories

  - Stores the mapping of filenames to i-node numbers
  - Data structure varies by system
  - The `C` functions `opendir, readdir, closedir` can access directories portably

### Linking

#### Unlinking
  - Also known as deleting a file by its filename
  - When deleting a file, the kernel will:
    1. Decrease the reference count of filenames referring to the i-node number
    2. If the reference count is now 0, the disk space and the i-node is freed up
  - System call for delete is `unlink`

#### Hard link
  - Creates a literal link between 2 files
  - Useful when you want to access a file in an inaccessible directory
  - `ln` can create another filename that has the same i-node number as an existing file
    ```sh
    $ ln path/to/file1 path/to/file2 # this creates a 'hard link'
    ```
  - The hard link between 2 files will not be broken if one of the file names change or even get deleted
  - Hard linking dirs is **_disallowed_** unless implementing special dirs like `.` and `..`

#### Symbolic Linking (Symlink)
  - Special file that simply stores a path (sort of like a shortcut)
  - Most system calls and `C` library functions/programs follows symlinks
  - Create a symlink using `ln -s`

### Bitwise Operators in `C`

- Bitwise AND (`&`), OR (`|`), NOT (`~`), XOR (`^`)
- Left Shift (`<<`) and Right Shift (`>>`)
- Example:
  ```C
  unsigned char a = 10001001; // 8-bit binary
  unsigned char b = 00000011;
  a & b   = 00000001          // logical AND on each bit
  a | b   = 10001011          // logical OR on each bit
  a ^ b   = 10001010          // XOR on each bit
  ~ a     = 01110110          // flips all bits
  a << 1  = 00010010          // shifts all bits 1 to the left
  b >> 2  = 00000000          // shifts all bits 2 to the right
  ```

### File Mode Bits

- `st_mode` bitwise layout 
  ```
      File Type                       Permissions
  ---------------- ------------------------------------------------
  | - | - | - | - | U | G | T | R | W | X | R | W | X | R | W | X |
                              ----------- ----------- -----------
                                  User       Group        Other
  ```

- Macros for checking individual permissions:
  - `S_IRUSR`: Binary `0000 100 000 000`
  - `S_IWUSR`: Binary `0000 010 000 000`
  - `S_IXUSR`: Binary `0000 001 000 000`

### System Calls for File I/O
```C
int open(const char *path, int flags);
int open(const char *path, int flags, int mode);
  flags: O_WRONLY, O_RDONLY, O_RDWR, O_EXCL, O_TRUNC, O_APPEND;
// Combine flags with bitwise OR

size_t read(int fd, void *buf, size_t count);
size_t write(int fd, void *buf, size_t count);
off_t lseek(int fd, off_t offset, int origin);
  origin: SEEK_SET, SEEK_CUR, SEEK_END;

// Takes another fdt entry to the same open file table entry
int dup(int oldfd);
// takes fdt entry to be `newfd` and duplicates `oldfd` to it
int dup2(int oldfd, int newfd);

int close(int fd);
```

### File Descriptor (`fd`)

- Every process has a *finite* 'File descriptor table' for opened files
- The File Descriptor is an array index into it
- Example of file descriptors:
  - 0: `stdin`
  - 1: `stdout`
  - 2: `stderr`
- `open()` and `dup()` consumes the lowest-number free entry
- `close()` frees entries