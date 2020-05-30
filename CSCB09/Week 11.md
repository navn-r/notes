---
title: Week 11
created: '2020-08-09T22:46:54.978Z'
modified: '2020-08-09T23:28:18.091Z'
weight: 11
---

# Week 11

## Multiplexing Input and Output
> Handling multiple input/output clients with `select` and `epoll`

### `select`
```C
int select(int n, fd_set *read_fds, fd_set *write_fds, NULL, struct timeval *wait_timeout);
```
- Blocks until fds are ready
- Returns 0 if reaches the timeout, else returns positive count if some fds are ready
- Modifies the given `fd_set`s, set them again before the next call
- `fd_set`: holds a set of file descriptors
- `n`: The highest fd you specify, + 1

#### `fd_set` functions
```C
void FD_ZERO(fd_set *s);           // empties the set

void FD_SET(int fd, fd_set *s);    // adds an fd to the set

void FD_CLR(int fd, fd_set *s);    // deletes an fd

int FD_ISSET(int fd, fd_set *s);   // queries if fd is in the set
```

#### Limitations 

- Max size for `fd_set` usually 1024
- Slow when using many file descriptors

### `epoll`
> Only available on Linux
```C
int epoll_create1(int flags);
```
  - Creates a new `epoll` instance 
  - Returns the file descriptor of this instance (`epfd`)
  - `flags`: 0 or `FD_CLOEXEC`
  - Instance only useful for other `epoll` functions

```C
int epol_ctl(int epfd, int op, int another_epfd, struct epoll_event *ev);
```
  - Sets what the `epoll` instance will wait for
  - Returns 0 if successful
  - `op`: `EPOLL_CTL_ADD`, `EPOLL_CTL_DEL`, `EPOLL_CTL_MOD`
  - `ev`: Events to wait for

```C
int epoll_wait(int epfd, struct epoll_event *evs, int n, int timeout);
```
  - `evs`: Array to recieve events
  - `n`: Length of events array
  - Returns count of ready file descriptors 

```C
struct epoll_event {
  uint32_t      events;
  epoll_data_t  data;
};
```
- You can combine `events` using bitwise operators
- `events`:

  - `EPOLLIN`: Ready to read

  - `EPOLLOUT`: Ready to write

  - `EPOLLONESHOT`: Monitor once only

  - `EPOLLET`: Notify whenever changes from not-ready to ready

  - `EPOLLHUP`: Other end of pipe/socket has closed

  - `EPOLLERR`: Error condition

```C
typedef union epoll_data {
  void      *ptr;
  int       fd;
  uint32_t  u32;
  uint64_t  u64;
} epoll_data_t;
```
- You store to this data when calling `epoll_ctl`
- Stored data gets returned when you call `epoll_wait`
- Usually store the fd, or a pointer to some `struct` to keep track of history
