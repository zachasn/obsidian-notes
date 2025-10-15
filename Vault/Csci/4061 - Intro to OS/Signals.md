# Signals
## Key Concepts
- **Signal:** A software interrupt that notifies a process that an event has occurred
- **Signal Handler:** A function that gets called when a process receives a signal
- **Signal Mask:** Set of signals whose delivery is currently blocked
- **Pending Signal:** A signal that has been generated but not yet delivered
- **Asynchronous Event:** Signals can arrive at any time, interrupting normal program flow
- **Signal Disposition:** What a process does when it receives a signal (default, ignore, or catch)
- **Atomic Operation:** An operation that cannot be interrupted by signals
## Signal Types
Signals can be classified by their purpose:
### Process Control
- `SIGCHLD` - Child process terminated, stopped, or continued
- `SIGCONT` - Continue if stopped
- `SIGSTOP` - Stop process (cannot be caught or ignored)
- `SIGTSTP` - Terminal stop signal (Ctrl-Z)
### Error Conditions
- `SIGFPE` - Arithmetic exception (e.g., divide by zero)
- `SIGILL` - Illegal instruction
- `SIGSEGV` - Invalid memory reference (segmentation fault)
- `SIGBUS` - Bus error (alignment error)
- `SIGABRT` - Abort signal from `abort()`
### Termination
- `SIGTERM` - Termination signal (default kill signal)
- `SIGKILL` - Kill signal (cannot be caught or ignored)
- `SIGQUIT` - Quit from keyboard (Ctrl-\)
- `SIGINT` - Interrupt from keyboard (Ctrl-C)
### Alarms & Timers
- `SIGALRM` - Timer signal from `alarm()`
- `SIGVTALRM` - Virtual timer expired
- `SIGPROF` - Profiling timer expired
### I/O
- `SIGPIPE` - Broken pipe (write to pipe with no readers)
- `SIGIO` - Asynchronous I/O event
- `SIGURG` - Urgent condition on socket
### User-Defined
- `SIGUSR1` - User-defined signal 1
- `SIGUSR2` - User-defined signal 2
## Signal Dispositions
When a signal is delivered to a process, we can tell the kernel to do one of three things:
1. **Ignore** - Process explicitly ignores the signal
2. **Catch** - Process provides a signal handler function to execute
3. **Default Action** - Each signal has a default action:
	- *Terminate* - Process terminates, default action for most 
	- *Terminate + Core* - Process terminates and creates core dump
	- *Ignore* - Signal is discarded
	- *Stop* - Process is stopped
	- *Continue* - Process is continued if stopped
> [!important] Uncatchable Signals
> `SIGKILL` and `SIGSTOP` cannot be caught, blocked, or ignored.
## System Calls

##### `signal()` (Simple Interface)
```c
#include <signal.h>
void (*signal(int signo, void (*handler)(int)))(int);

// Simpler typedef version:
typedef void sighandler_t (int);
sighandler_t *signal(int, sighandler_t *);
```
- Establishes a signal handler for signal `signo`
- `handler` can be:
  - `SIG_DFL` - Use default action
  - `SIG_IGN` - Ignore the signal
  - Address of a function - Call this function when signal arrives
- Returns previous handler on success, `SIG_ERR` on error
- **Limitation:** Behavior varies across implementations, use `sigaction()` instead
##### `sigaction()` (Reliable Interface)
```c
#include <signal.h>
int sigaction(int signo, const struct sigaction *act, struct sigaction *oact);

struct sigaction {
    void (*sa_handler)(int);              // Handler function or SIG_IGN/SIG_DFL
    sigset_t sa_mask;                     // Additional signals to block during handler
    int sa_flags;                         // Modify behavior
    void (*sa_sigaction)(int, siginfo_t *, void *); // Alternative handler
};
```
- More reliable and portable than `signal()`
- `signo` - Signal number to handle
- `act` - New action for this signal (NULL to query current action)
- `oact` - Previous action is stored here (can be NULL)
- Returns 0 on success, -1 on error

**Common `sa_flags`:**
- `SA_RESTART` - Automatically restart interrupted system calls
- `SA_NOCLDSTOP` - Don't receive `SIGCHLD` when children stop
- `SA_NOCLDWAIT` - Don't create zombies when children terminate
- `SA_NODEFER` - Don't block this signal while handler is running
- `SA_RESETHAND` - Reset handler to `SIG_DFL` after one use
- `SA_SIGINFO` - Use `sa_sigaction` handler instead of `sa_handler`
##### `kill()` & `raise()`
```c
#include <signal.h>
int kill(pid_t pid, int signo);
int raise(int signo);
```
**`kill()`** - Send signal to process or process group:
- `pid > 0` - Send to process `pid`
- `pid == 0` - Send to all processes in sender's process group
- `pid == -1` - Send to all processes sender has permission to signal (broadcast)
- `pid < -1` - Send to all processes in process group `|pid|`
- `signo == 0` - "null signal" - check if process exists without sending signal
- Returns 0 on success, -1 on error

**`raise()`** - Send signal to calling process:
- Equivalent to `kill(getpid(), signo)`
- Returns 0 on success, nonzero on error
##### `alarm()` & `pause()`
```c
#include <unistd.h>
unsigned int alarm(unsigned int seconds);
int pause(void);
```
**`alarm()`** - Schedule `SIGALRM` signal:
- Generates `SIGALRM` after `seconds` seconds
- Only one alarm per process (new alarm replaces old)
- `alarm(0)` cancels pending alarm
- Returns seconds remaining on previous alarm, or 0 if none

**`pause()`** - Suspend process until signal is caught:
- Always returns -1 (interrupted by signal)
- Process sleeps until *any* signal is delivered
##### Signal Set Functions
```c
#include <signal.h>
int sigemptyset(sigset_t *set);           // Initialize to empty
int sigfillset(sigset_t *set);            // Initialize with all signals
int sigaddset(sigset_t *set, int signo);  // Add signal to set
int sigdelset(sigset_t *set, int signo);  // Remove signal from set
int sigismember(const sigset_t *set, int signo); // Test if signal in set
```
- All return 0 on success, -1 on error (except `sigismember`)
- `sigismember` returns 1 if true, 0 if false, -1 on error
- Always initialize with `sigemptyset()` or `sigfillset()` before use
##### `sigprocmask()`
```c
#include <signal.h>
int sigprocmask(int how, const sigset_t *set, sigset_t *oset);
```
- Examine or change the signal mask (blocked signals)
- `how` specifies operation:
  - `SIG_BLOCK` - Add `set` to current mask: `mask = mask | set`
  - `SIG_UNBLOCK` - Remove `set` from current mask: `mask = mask & ~set`
  - `SIG_SETMASK` - Set mask to `set`: `mask = set`
- `set` - New signal set (NULL to query current mask)
- `oset` - Previous mask stored here (can be NULL)
- Returns 0 on success, -1 on error
##### `sigpending()`
```c
#include <signal.h>
int sigpending(sigset_t *set);
```
- Returns set of signals that are pending (generated but blocked)
- Returns 0 on success, -1 on error
##### `sigsuspend()`
```c
#include <signal.h>
int sigsuspend(const sigset_t *sigmask);
```
- Atomically sets signal mask to `sigmask` and suspends process until signal arrives
- When signal handler returns, signal mask is restored to value before `sigsuspend()`
- Always returns -1 (interrupted by signal)
- Solves race condition between checking condition and calling `pause()`
##### `sigsetjmp()` & `siglongjmp()`
```c
#include <setjmp.h>
int sigsetjmp(sigjmp_buf env, int savemask);
void siglongjmp(sigjmp_buf env, int val);
```
- Non-local goto that works correctly with signals
- `sigsetjmp()`:
  - Saves stack context in `env`
  - If `savemask` is nonzero, saves current signal mask
  - Returns 0 when called directly, `val` when reached via `siglongjmp()`
- `siglongjmp()`:
  - Jumps to location saved by `sigsetjmp()`
  - Restores signal mask if it was saved
  - Never returns
## Signal Concepts
### Blocking vs. Ignoring
- **Blocking** - Signal is not delivered, remains pending
- **Ignoring** - Signal is delivered but discarded (no action taken)
### Signal Delivery
1. Signal is **generated** when event occurs
2. Signal is **pending** if generated but not yet delivered
3. Signal is **delivered** when process takes action for it
4. Between generation and delivery, signal is **pending**
### Reliable vs. Unreliable Signals
**Unreliable Signals**:
- Handler reset to default after signal is caught
- Signals can be lost
- Race conditions between checking condition and blocking

**Reliable Signals** (POSIX):
- Handler remains installed
- Signals are queued (standard signals are not, real-time signals are)
- Atomic operations prevent race conditions
### Interrupted System Calls
Some system calls are "slow" → they can block forever:
- Reads from pipes, terminals, network sockets
- Writes to same (when full)
- Opens of devices that wait for condition (e.g., modem)
- `pause()`, `wait()`, etc.

When a signal is caught during a slow system call:
- The system call returns with error `EINTR` (interrupted)
- Must explicitly restart the call or use `SA_RESTART` flag
```c
// Manual restart pattern
again:
    if ((n = read(fd, buf, BUFSIZE)) < 0) {
        if (errno == EINTR)
            goto again;  // Restart read
        // Handle other errors
    }
```
### Reentrant Functions
Signal handlers can interrupt any code, so they must only call **async-signal-safe** functions:
- **Safe:** `read()`, `write()`, `_exit()`, `kill()`, `signal()`, etc.
- **Unsafe:** `printf()`, `malloc()`, `free()`, most library functions

> [!warning] Non-Reentrant Code in Handlers
> Calling non-reentrant functions (like `printf()` or `malloc()`) from signal handlers can cause:
> - Data corruption
> - Deadlock
> - Undefined behavior

**Safe Practices:**
- Set flags in handlers, check them in main loop
- Use only async-signal-safe functions
- Block signals during critical sections
### Race Conditions
Race condition with signals:
```c
if (signal_not_received) {
    pause();  // Signal might arrive here 
}
```

**Solution:** Use `sigsuspend()` for atomic check-and-sleep:
```c
sigset_t newmask, oldmask;
sigemptyset(&newmask);
sigaddset(&newmask, SIGUSR1);
sigprocmask(SIG_BLOCK, &newmask, &oldmask);

if (signal_not_received) {
    sigsuspend(&oldmask);  // Atomically unblocks and suspends
}

sigprocmask(SIG_SETMASK, &oldmask, NULL);
```
## Code Examples

##### Basic Signal Handler
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void sig_handler(int signo) { 
    if (signo == SIGINT)
        write(STDOUT_FILENO, "Caught SIGINT\n", 15);
}

int main() {
    if (signal(SIGINT, sig_handler) == SIG_ERR) {
        perror("signal");
        return 1;
    }

    while (1) {
        pause();  // Wait for signals
    }
    return 0;
}
```

##### Using `sigaction()`
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void sig_handler(int signo) {
    write(STDOUT_FILENO, "Caught signal\n", 15);
}

int main() {
    struct sigaction sa;

    sa.sa_handler = sig_handler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = SA_RESTART;  // Restart interrupted system calls

    if (sigaction(SIGINT, &sa, NULL) < 0) {
        perror("sigaction");
        return 1;
    }

    while (1) {
        pause();
    }
    return 0;
}
```
## Common Patterns

### Setting a Flag in Handler
```c
static volatile sig_atomic_t got_signal = 0;

void handler(int signo) {
    got_signal = 1;
}

int main() {
    signal(SIGUSR1, handler);

    while (!got_signal) {
        // Do work
    }

    // Handle signal
    got_signal = 0;
}
```

### Critical Section Protection
```c
sigset_t newmask, oldmask;

// Block signals during critical section
sigemptyset(&newmask);
sigaddset(&newmask, SIGINT);
sigprocmask(SIG_BLOCK, &newmask, &oldmask);

// Critical section here
// ... manipulate shared data ...

// Restore signal mask
sigprocmask(SIG_SETMASK, &oldmask, NULL);
```

### Signal-Safe Error Reporting
```c
void safe_print(const char *msg) {
    write(STDERR_FILENO, msg, strlen(msg));
}

void handler(int signo) {
    safe_print("Signal caught\n");  // Safe - write() is async-signal-safe
}
```

## Important Rules

> [!tip] Signal Handler Best Practices
> 1. Keep handlers short and simple
> 2. Only call async-signal-safe functions
> 3. Set flags and handle them in main loop
> 4. Use `sigaction()` instead of `signal()`

> [!warning]
> 5. Calling non-reentrant functions from handlers
> 6. Race conditions with `signal()` + `pause()`
> 7. Not reaping child processes → zombies
> 8. Not restarting interrupted system calls
> 9. Assuming signals are queued (standard signals aren't)

> [!important] SIGCHLD Special Behavior
> - Multiple `SIGCHLD` signals can be merged into one
> - Always use `waitpid()` in a loop with `WNOHANG`
> - Use `SA_NOCLDWAIT` flag to prevent zombie creation automatically
