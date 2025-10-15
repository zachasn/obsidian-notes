# Processes & Programs
## Key Concepts
- **Process:** An environment in which a program executes
- **Process Image:** The complete state of a process including code, data, heap, stack, and system resources needed to support this process
- **Process States:** New → Ready → Running → Blocked → Done
- **Process Relationship:** Parent—child relationships between processes
- **Zombie Process:** Terminated process whose exit status hasn't been collected by parent.(parent forks, child exits before parent waits for child)
- **Daemon:** Process that is always running in the background to provide a service.(parent forks child, doesn't wait for child, instead waits for input, then does work on that input)
- **Orphan:** Process whose parent has terminated before the child → Orphan processes are adopted by init

## Definitions 
 **Process ID(pid):**  Unique integer identifier assigned to each process by the OS, pids are positive integers and pid 1 is reserved for the init process
 
**Parent Process ID (ppid)::** the pid of the process that created the current process through `fork()` 

**Process States:** the current state of a process.
1. *Ready:* Eligible for execution but waiting for a turn
2. *Running:* In the middle of it's turn on the CPU
3. *Blocked:* Waiting for some event, e.g. input from user,  child process exits, etc
4. *Done:* Finished executing, no longer eligible for execution

## Process System Calls
##### `fork()`
```c
#include <unistd.h>
pid_t fork(void);
```
- Creates an exact copy of the calling process
- Returns 0 in the child process, child pid in parent process, -1 on failure
- the child process gets a copy of the process image(e.g., address space) and all open file descriptor
##### `exec()` Family of System Calls
```c
#include <unistd.h>
/* 
 l : Arguments are passed as a list of strings
 v : Arguments are passed as an array of strings
 p : Path(s) to search for the new running program
 e : Environment can be specified by the caller
*/

# First arugment is the full pathname to the executable (bin/cat)
int execl(const char *path, const char *arg, ...);
int execv(const char *path, char *const argv[]);
int execle(const char *path, const char *arg, ..., char * const envp[]);
int execve(const char *path, char *const argv[], char *const envp[]);

# First argument is just the program name, the system searches for it in the directories listed in the PATH Enviroment viariable 
int execlp(const char *file, const char *arg, ...);
int execvp(const char *file, char *const argv[]);
```
- When any`exec()`is called the calling process is completely replaced with a **new program**, and that **new program** starts executing its `main`function
- **Whats replaced:** Current process image (code, data, heap, stack)
	1. Current process's image (code, data, heap, stack segments)
- The **new program inherits:**
	1. *Process(`pid`), parent process(`ppid`), and process group(`pgid`) IDs* 
	2. *Real user(`uid`), group(`gid`), and session IDs*
	3. Current *working directory*
	4. *Root directory*
	5.  *File mode creation mask* 
	6. Open *file descriptors* - unless marked with `FD_CLOEXEC`flag
	7. Pending *signals* and process *signal mask*
	8. *Resource limits*
- `exec()`calls don't return on success, if a call to `exec()`fails, returns -1
##### `wait()`& `waitpid()`
```c
#include <sys/wait.h>
pid_t wait(int *status);
```
- Parent waits for any child to terminate
- blocks until a child exits
- stores the exit information of the child in status unless `NULL` is passed in
- returns the pid of the terminated child or -1 on error
```c
#include <sys/wait.h>
pid_t waitpid(pid_t pid, int *status, int options);
```
- `waitpid()`can wait for a specific child OR any child:
	- `pid > 0`wait for that specific child
	- `pid = 0`wait for *ANY* child whose process group id is equal to the calling process
	- `pid = -1`wait for *ANY* child — same as `wait()`
	- `pid < -1`wait for *ANY* child whose process group id is equal to the absolute value of 'pid'
- 3rd argument is set of options that control the behavior of the operation:
	- `options = 0`— parent blocks until the child terminates
	- `options = WNOHANG`— **doesn't block**, returns even if no child has terminated
	- `options = WUNTRACED`— parent blocks until the child terminates or stops

##### Error Handling
> [!tip] Always Check Return Values 
> System calls can fail — always check return values and handle errors appropriately

> [!warning] Zombie Processes
> Failing to wait for child processes creates zombies:
> ```c
> if (fork() == 0) {
> 	 // do something 
> 	 // here
> 	exit(0); // child exits
> }
> // parent doesnt wait → child becomes a zombie
> ```

## Code Examples
##### `fork()`
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h> 

int main() {
	pid_t pid = fork();
	if (pid == -1) {
		perror("fork failed");
		return 1;
	} else if (pid == 0) { // Child process
		printf("Child process: PID = %d, PPID = %d\n", getpid(), getppid());
	} else { // Parent process
		printf("Parent process: PID = %d, Child PID = %d\n", getpid(), pid);
		wait(NULL); // Wait for child to complete
	}
	return 0;
}
```

##### `fork()`& `exec()`
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
	pid_t pid = fork();
	if (pid == -1) {
		perror("fork failed");
		return 1;
	} else if (pid == 0) { // Child executes ls command
		execl("/bin/ls", "ls", "-l", NULL);
		// or
		execlp("ls", "ls", "-l", NULL);
		perror("exec failed"); // Only reached if exec fails
		return 1;
	} else { // Parent waits for child
		int status;
		waitpid(pid, &status, 0);
		printf("Child exited with status %d\n", WEXITSTATUS(status));
	}
	return 0;
}
```