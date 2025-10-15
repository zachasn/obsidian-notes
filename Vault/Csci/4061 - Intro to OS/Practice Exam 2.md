# CSCI 4061: Intro to Operating Systems - Practice Exam 2

## 1. True/False Questions
For each false answer, provide a brief 1-2 sentence explanation.

(a) After calling `fork()`, the child process inherits all of the parent's open file descriptors, and each process has its own independent file offset for each descriptor.
- *false*, both parent and child file descriptor point to the same system file entry.

(b) The `execvp()` function will search the PATH environment variable to locate the executable, while `execv()` requires an absolute path.
- *True*

(c) If a parent process terminates before calling `wait()` on its children, those children become zombies.
- *True*

(d) The `dup2(fd, STDOUT_FILENO)` call will always fail if `fd` equals `STDOUT_FILENO`.
- 

(e) When using `fread()`, the return value always equals the `count` parameter if no error occurred.
- *False*, read returns bytes actually read which could be less than request bytes

(f) The `lseek()` function can be used to seek beyond the end of a file, creating a "hole" in the file.
- *True*

(g) If two independent processes open the same file, writes from one process will affect the file offset of the other process.
- *False*, each process gets its own system table entry

(h) The `O_TRUNC` flag causes an existing file to be truncated to zero length when opened.
- *True*

(i) Standard error (stderr) is line-buffered by default, so output is flushed whenever a newline is written.
- *False*, stderr is unbuffered so that errors immediately display

(j) After a successful `read()` call, the file offset is always incremented by the value of the `nbytes` parameter.
- *False*, offset is incremented by actually bytes read

---

## 2. Process Creation
For each code snippet, determine how many processes exist after execution completes (assume all fork() calls succeed):

**(a)**
```c
pid_t pid;
for (int i = 0; i < 3; i++) {
    pid = fork();
    if (pid > 0) {
        break;
    }
}
```
4

**(b)**
```c
pid_t pid1, pid2;
pid1 = fork(); // 2 procs
if (pid1 == 0) { // child forks -> 2 more procs
    fork();
} else { // parent forks -> 2 more forks
    fork();
}
```
 7
 
**(c)**
```c
for (int i = 0; i < 4; i++) {
    if (fork() == 0) {
        if (i % 2 == 0) { // 0,2,4-> fork()
            fork();
        }
        break;
    }
}
```


---

## 3. File I/O and Offsets
Consider the following code. Assume no errors occur and `data.txt` contains exactly 100 bytes of data.

```c
int fd1 = open("data.txt", O_RDONLY);
int fd2 = open("data.txt", O_RDONLY);

char buf[10];
read(fd1, buf, 10); // read 10s from data -> f1 position = 10
read(fd2, buf, 5); /// reads 5 from from data -> f2 position = 5

pid_t pid = fork();
if (pid == 0) {
    read(fd1, buf, 10);  // 10 + 10 = 20
    read(fd2, buf, 5); // 5 + 5 = 10
} else {
    wait(NULL);
    read(fd1, buf, 10); // 10 + 10 + 10 = 30 bytes
}
```

**(a)** What is the file offset for `fd1` in the parent process after all code executes?
- 30

**(b)** What is the file offset for `fd2` in the parent process after all code executes?
- 10

**(c)** Explain why the offsets for `fd1` and `fd2` are different (or the same).
- When fork is called, both parent and child process share the same system file entry. When child reads from f1, it starts off from 10 (current offset after first read) and for f2, it starts off from 10

---

## 4. Implement: Parallel Execution with Output Capture
Implement a function `run_parallel()` that executes multiple commands in parallel and captures each command's output to a separate file.

**Function signature:**
```c
int run_parallel(char *commands[], char *args[], int n_commands);
```

**Parameters:**
- `commands[]`: Array of command names (e.g., `"ls"`, `"pwd"`, `"date"`)
- `args[]`: Array of arguments for each command (can be NULL)
- `n_commands`: Number of commands to execute

**Requirements:**
- Execute all commands in parallel (don't wait between forks)
- Redirect each command's stdout to a file named `output_<i>.txt` where `i` is the command index
- Wait for all children to complete before returning
- Return 0 on success, -1 if any error occurs
- You may assume all commands are in PATH
- Perform error handling for fork() and waitpid() only

**Example usage:**
```c
char *cmds[] = {"ls", "pwd", "date"};
char *args[] = {"-l", NULL, NULL};
run_parallel(cmds, args, 3);
// Creates: output_0.txt (ls -l output)
//          output_1.txt (pwd output)
//          output_2.txt (date output)
```

**Your implementation:**
```c
int run_parallel(char *commands[], char *args[], int n_commands) {
    // Your code here
	for (int i = 0; i < n_commands; i++) {
		pid_t child_pid = fork();
		if (child_pid < 0) {
			perror("fork failed);
			return -1;
		} else if (child_pid == 0) {
			int file_name = snsprintf("output_", 14, "%d.txt", i);
			int fd = open(file_name, O_WRONLY | O_CREAT, U_IWRUSR);
			dup2(fd, STDOUT_FILENO);
			
		} else {}
	}
}










}
```

---

## 5. File I/O with Binary Data
You are given a binary file containing structs of type:

```c
typedef struct {
    int id;
    float score;
    char grade;
} student_t;
```

Implement the function `filter_passing()` that reads students from an input file and writes only those with `score >= 60.0` to an output file.

**Function signature:**
```c
int filter_passing(char *input_file, char *output_file);
```

**Requirements:**
- Return 0 on success, -1 on error
- Perform error handling for fopen(), fread(), and fwrite()
- Don't worry about error handling for fclose()

**Your implementation:**
```c
int filter_passing(char *input_file, char *output_file) {
    // Your code here
















}
```

---

## 6. Process Output Analysis
Analyze the following code and answer the questions below. Assume no errors occur.

```c
int main() {
    int fd = open("log.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);

    printf("A");
    write(fd, "B", 1);

    pid_t pid = fork();

    if (pid == 0) {
        printf("C");
        write(fd, "D", 1);
        close(fd);
        exit(0);
    } else {
        wait(NULL);
        printf("E");
        write(fd, "F", 1);
        close(fd);
    }

    return 0;
}
```

**(a)** What are all possible outputs to the screen (stdout)?

**(b)** What are all possible contents of `log.txt`?

**(c)** If we add `fflush(stdout);` right before the `fork()` call, how does this change the screen output?

---

## 7. Short Answer

**(a)** Explain the difference between a zombie process and an orphan process. How does the operating system handle each?

**(b)** Why does `exec()` not return on success? What happens to the code that follows an `exec()` call?

**(c)** Describe a scenario where using `waitpid()` with the `WNOHANG` flag would be more appropriate than using `wait()`.

**(d)** Explain why `dup2()` is an atomic operation and why this matters for file descriptor redirection.

---

## 8. Debugging Challenge
The following code is supposed to create a child process that writes "CHILD" to a file, while the parent writes "PARENT" to the same file. However, it has several bugs. Identify at least 3 bugs and explain what's wrong.

```c
int main() {
    int fd = open("output.txt", O_WRONLY | O_CREAT);

    if (fork() == 0) {
        char *msg = "CHILD\n";
        write(STDOUT_FILENO, msg, strlen(msg));
    } else {
        char *msg = "PARENT\n";
        write(fd, msg, strlen(msg));
    }

    return 0;
}
```

**Bug 1:**

**Bug 2:**

**Bug 3:**

---

## 9. Advanced: File Descriptor Table
Draw the state of the kernel's file descriptor table, system file table, and v-node table after the following code executes (right before the return statement). You can use a simplified diagram.

```c
int fd1 = open("test.txt", O_RDONLY);
int fd2 = dup(fd1);
int fd3 = open("test.txt", O_RDONLY);

pid_t pid = fork();
if (pid > 0) {
    wait(NULL);
}

return 0;
```

Show the tables for both parent and child processes before they terminate.

---

## 10. Code Tracing
What is the output of this program? Show all possible outputs if there are multiple possibilities.

```c
int main() {
    int x = 0;

    if (fork() == 0) {
        x = 1;
        if (fork() == 0) {
            x = 2;
        }
        printf("%d", x);
        exit(0);
    }

    wait(NULL);
    wait(NULL);
    printf("%d", x);

    return 0;
}
```