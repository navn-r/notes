---
title: Week 6
created: '2020-06-16T23:21:27.366Z'
modified: '2020-06-17T00:06:17.870Z'
weight: 6
---

# Week 6

## Programming in `C` (cont.)


### File I/O in `C`

- Content can be accessed as a steam (seq. read/write)
- File functions work with `FILE *`
  - `FILE`: Type rep. stream state, definition varies by platform, most likely a `struct`

### Open
```C
FILE *fopen(const char *filename, const char *mode)
```
- Modes: `"r", "w", "a", "r+", "w+", "a+"`
  - Read, Write, Append, Read & Write, Write & Read, Append & Read
  - Appending and Writing to a file will create a new one if the file does not exist
  - Reading to a non-existent file will cause an error (returns `NULL`)

### Close
```C
int fclose(FILE *stream)
```
- Close stream when finished with the file
- Returns `0` if success, `EOF` if error
- Why should you close the file as soon as you're finished:
  - There is a limit on how many streams are open per process
  - Writing may be buffered until closing
  - No two process can open the same file (Windows only)

### Formatted I/O
```C
// printf and scanf but for a given stream
int fprintf(FILE *stream, const char *format, ...) 
int fscanf(FILE *stream, const char *format, ...)
```
- `printf` is just `fprintf` but `stdout` is specified
  ```C
  scanf(format, args) == fscanf(stdin, format, args) //same for scanf
  ```
- You do not need to manually close `stdin, stdout, stderr` streams

### Character and String I/O

- One single character: returns `EOF` if error or not found
  ```C
  int putchar(int c)  /* stdout */
  int putc(int c, FILE *stream)
  int getchar(void)    /* stdin */
  int getc(FILE *stream)
  ```
- String:
  ```C
  int fputs(const char *string, FILE *stream)   // does not put a newline at the end
  char *fgets(char *dest, int n, FILE *stream)  // Reads at most (n-1) chars or until (and including) newline
  ```

### Arbitrary Data I/O
```C
size_t fread(void *dest, size_t s, size_t n, FILE *stream)
size_t fwrite(const void *data, size_t s, size_t n,FILE *stream)
```
- Reads/Writes `n` items, each of `s` bytes
- Returns how many items have been read/written
- Potential Use Cases:
  - A whole array
  - `struct`
  - Raw bytes (array of `unsigned char`)


### Error versus End-of-Stream Disambiguation

- `getc`, `scanf` return `EOF` on error
- `fgets` returns `NULL`
- `fread` returns < `n`
```C
int feof(FILE *stream)      // returns true if end-of-stream
int ferror(FILE *stream)    // returns true if error
void clearerr(FILE *stream) // clears end-of-stream and error status
```

### Error Information
```C
#include <errno.h>
```
- Global variable stores error reason of most recent error
  ```C
  int errno;
  ```
- Many possible values:
  ```C
  ENOENT  // File does not exist
  EACCESS // No permission
  EDOM    // sqrt(-3.0)
  ```
- Usually, we just use `perror` to print the error message to stderr:
  ```C
  void perror(const char *prefix)
  ```

### Buffering
  - `C` delays file writing
    - Accumulates data in a buffer until it is large, then requests the kernel to write that chunk
  - Also hastens reading
    - Requests kernel to read a large chunk into the buffer, then serves read requests from said buffer

### Buffer Operations
```C
int fflush(FILE *stream) // Returns 0 if success, EOF if error
```
- Writes the buffer for the output stream
- Clears the buffer for the input stream

```C
int setvbuf(FILE *stream, char *buf, int mode, size_t n)
```
| mode | meaning |
| :--: | :-----: |
| `_IOFBF` | full buffering |
| `_IOLBF` | line buffering |
| `_IONBF` | no buffering |


### Default Buffering of `stdin`, `stdout`, `stderr`

- If Terminal:
  - `stdin`: line buffer
  - `stdout`: no buffer
  - `stderr`: **_complicated_**

- If in a file or pipelined:
  - `stdin`: full buffer
  - `stdout`: no buffer
  - `stderr`: full buffer

### Seeking
- Ask current position:
  ```C
  long int ftell(FILE *stream) // returns -1L if error
  ```
- Seek:
  ```C
  int fseek(FILE *stream, long i, int origin) // non-zero return if error
  ```
  | origin | go to `i` bytes from |
  | :--: | :-----: |
  | `SEEK_SET` | beginning |
  | `SEEK_END` | end |
  | `SEEK_CUR` | current position |


