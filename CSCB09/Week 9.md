---
title: Week 9
created: '2020-07-25T18:21:42.164Z'
modified: '2020-07-25T20:45:11.571Z'
weight: 9
---

# Week 9

## Processes and Redirection 

### Launching a New Process

1. Clone the process
    ```C
    pid_t fork(void); // child gets return value 0, parent gets child's pid 
    ```
    - Both processes (child and parent) run the same code
2. The child can switch to running another program 
    ```C
    execlp(path, arg0, arg1, ..., (char *)NULL); // 'exec' family of system calls
    ```
    - Things such as environment variables, pid, fd, current dir, etc. are preserved
    - File descriptors can be closed before exec by marking them as `close on exec`

#### Why seperate `fork` and `exec`?
  - Some use cases do not require `exec`
  - The child process can do some prep before `exec` (file redirection and pipelining)

### The only 'parent-less' process: `init`

  - `fork` is the only way to launch new processes
  - As the kernel boots, it launches `init`, with pid 1

### Process Commands

  - `ps`: List processes
  - `pgrep`: Find processes by name, users, etc.
  - `top`: Process list that periodically refreshes (think of it as a terminal task manager)
  - `htop`: `top`, but better (more features)
  - `kill` and `pkill`: Terminates a process if allowed (`pkill` finds like `pgrep`)

### Waiting for a Child process

```C
pid_t wait(int *status); // status is for child's exit code
pid_t waitpid(pid_t pid, int *status, int options);
// pid > 0 -> wait for the given child
// pid == -1 -> wait for any child
// options == WNOHANG -> don't hang waiting
```

#### Useful macros
  - Normal Termination:
    - `WIFEXITED`, `WEXITSTATUS`
  - Killed by signal:
    - `WIFSIGNALED`, `WTERMSIG`, `WCOREDUMP`
  - Stopped and continued by signal:
    - `WIFSTOPPED`, `WIFCONTINUED`

### Termination of Parent before Child

  - If the child terminates first and the parent process is still running and does not call `wait`
    - A 'zombie' process of the child retains the entry of the child, but is not actually running
  - If the parent terminates but the child is still running
    - The child is now an 'orphan' process and gets the parent pid of `init`
  - If the child terminates and the parent calls `wait`
    - Just regular termination and no 'zombie'


### File Redirection

  - Before `exec` and `fork`, open the file
  - Duplicate the file descriptor with `dup2(curr_fd, new_fd)`
  - Close the file descriptor or request `close on exec`
  - Then call `exec`

### Pipes

```C
int pipe(int pipefd[2]); // creates a unidirectional pipe
// pipefd[0] is for read end
// pipefd[1] is for write end
```
- This is how shells do pipelining
- Usually, only one process at both ends
- For `dup/dup2`, `stdout` for write end, and `stdin` for read end.
- Close fds you do not need as soon as possible
- Kernel has a buffer for unread data if the `write end` is writing faster than the `read end` can handle


## Signals

> How the kernel notifies processes of some events and severe errors

#### Examples
  - Interupt (`CTRL+C`): `SIGINT`

  - Suspend and Resume: `SIGSTOP`,`SIGCONT`

  - Child died/suspended/resumed:`SIGCHLD`

  - Broken pipe:`SIGPIPE`

  - Request for termination (shell ‘`kill`’ default):`SIGTERM`

  - Hard request for termination:`SIGKILL`

  - Illegal memory access (two types:`SIGBUS`,`SIGSEGV`)

  - Application-specific:`SIGUSR1`,`SIGUSR2`


### Signal Life Cycle

  - Some events generate a signal
  - Kernel tries to deliver the signal, and is pending until delivered
  - Normal exec resumes if signal ignored or handler returns normally
  - Shell Command:
    ```sh
    $ kill -SIGKILL 31337
    $ kill -9 31337
    ```
  - System Calls:
    ```C
    int kill(pid_t pid, int sig);
    int raise(int sig); // to self
    ```

### Signal Actions/Handlers

```C
int sigaction(int sig, const struct sigaction *act, struct sigaction *oldact)
```
- `sig`: The signal type
- `act`: The new action you want
- `oldact`: The old action
- on `fork`: Signal actions cloned
- on `exec`: Handlers are replaced by default

#### `struct sigaction`
```C
struct sigaction {
  void *sa_handler(int sig);  // pointer to handler function, SIG_IN, or SIG_DFL
  sigset_t sa_mask;           // mask these signals when running handler
  int sa_flags;               // options
  void *sa_restorer(void);    // not for application use
}
```

#### `sigset_t` Operations

```C
int sigemptyset(sigset_t *set);
int sigfillset(sigset_t *set);  // add all signals
int sigaddset(sigset_t *set, int sig);
int sigdelset(sigset_t *set, int sig);
int sigismember(const sigset_t *set, int sig);
```

#### `sa_flags` Flags

```C
// If you install handler
SA_NODEFER    // don't mask signal when running handler
SA_RESETHAND  // reset action to default before running handler
SA_RESTART    // auto-restart most syscalls before running handler

// For SIGCHLD
SA_NOCLDSTOP  // don't signal for child stop/cont.
SA_NOCLDWAIT  // don't turn terminated child into zombie
```
















