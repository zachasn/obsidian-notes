# Csci 4061: Intro to Operating Systems practice midterm exam

  

## 1. General Questions
Choose either true or false for each question below. If the answer is *false*, write a one or two sentence explanation of why that is.

(a) On an x86-64 CPUl, a char pointer is represented using 1 bytes while an int pointer is represented using 4 bytes.
- *False*, All pointers are represented using 8 bytes

(b) When a call to fwrite() makes a change to a file we can assume that this change has been made on disk as soon as the kernal buffer has been updated.
- *False*, changes are made when buffer is full or flushed.

(c) If we ask to read in 512 bytes from a file using the read() function and no error occurs, then the function will always return the value 512.
- *False*, read returns the actually number of bytes read, it doesn't guarantee that you'll always read  512 bytes.

(d) If the current position (offset) in an open file is moved forward, this can affect file descriptors in multiple processes.
- *False*, Each process gets its own its own independent file offset unless they have parent-child relationship.

(e) The fork() function call has two possible return values 0 and -1. fork returns -1 on error or 0 in the parent and child success.
	*False*,  on success, fork returns 0 to the child, child's pid to the parent. On error, it returns -1.

(f) The WNOHANG flag can be used for a polling based design when using waitpid
- *True*

(g) To run pwd program, a process can call execl("pwd", "pwd", NULL)
- *False*, execl() expects an absolute path
(h) If read() returns 0, then we have reached the end of the file.
- *True*

(i) The offset (current position) for an open file is stored in a global (system) file table
- *True*
(j) /minitar.c is a relative path, while /home/mhamilton/grades.txt is an absolute path.
- *False*, A relative path is dependent on the current directory
## 2.
For each of the code snippets below, how many processes are there by the end, assuming fork() always succeeds

(a)
```c
pid_t pid;
for (int i = 0; i < 4; i++) {
	pid = fork();
}
```
- 2^4 = 16    

(b)
```c
pid_t pid;
for (int i = 0; i < 9; i++) {
	pid = fork();
	if (pid == 0) {
		break;
	}
}
```
- 1 parent + 9 children = 10
## 3. I/O with stdio library
Say we have a binary file storing structs of the following type:
```c

typdef struct {
	double x;
	double y;
	double z;
} point_t;

```
The structs are stored contiguously without any padding between their representations (as if we had written an array of point_t instances to the file). Your task is to implement the function copy_for_n which will copy n point_t instance from a source file to a destination file (which will also be a binary file), skip the next 1

instance, then copy n point_t instance, skip 1 instance, and repeat until the end of the source file is reached.

The function takes three arguments:
- char *source file: The name of the binary file to copy from
- char *dest_file: The name of the binary file to copy to
- unsigned n: The number of point_t instances to skip after copying an instance

The function should return 0 on success or -1 on error. Error handling is expected for all library function calls except fclose() and fwrite(). Fill in the blanks in the code below to complete the implementation of copy_skip_n.
```c

int copy_for_n(char *source_file, char *dest_file, unsigned n) {

	FILE *f = fopen(source_file, "r");
	if (f == NULL) {
		return -1;
	}
	FILE *g = fopen(dest_file, "w");
	if (g == NULL) {
		fclose(f);
		return -1;
	}
	point_t point_buf;
	size_t read_count; 
	
	while (read_count = fread(&point_buf, 1, sizeof(point_t), f) > 0) {
		fwrite(&point_buf, 1, sizeof(point_t), g); 
		if (fseek(f, n * sizeof(point_t), SEEK_CUR) != 0) {
			fclose(g);
			return -1;
		}
	}
	if (ferror(f)) {
		fclose(f);
		fclose(g);
		return -1;
	}
	fclose(f);
	fclose(g);
	return 0;
}
```

## 4. Process Creation and Management
Your task is to implement a function, run_in sequence that will execute ls several times in sequence. Each Is program starts only after its predecessor in the sequence has exited. Furthermore, each program should have its output redirected to a file with a name based on the program's position in the sequence, starting at index 0.

If a program is at position i in the sequence, then its output should be redirected to the file i. txt. That is, the programs will have their output redirect to the files 0. txt,1.txt, and so on.
- The run sequence function has two arguments: location_names, which is an array of strings containing the name of the directories to execute ls on (in the expected order), and n_location, the length of this array.
- Your function should return 0 on success or -1 if an error occurs.
- You may assume that the Is program is in the PATH environment variable.
- You are expected to perform error handling when creating or waiting for child processes but do not need to perform any other error handling in your code below.
- You may assume each executed ls succeeds and terminates normally.
  
Hint: The code snippet below shows an example of creating a file name with the.txt extension from the value of an integer variable. You do not need to worry about checking the return value of snprintf for errors in your code.

```c
char file_name[10];
int i = 5;
snsprintf(file_name, 10, "%d.txt", i);
```

```c
int run_in_sequence(char *location_names[], int n_location) {
	for (int i = 0; i < n_location; i++) {
		pid_t child_pid = fork();
		if (child_pid < 0) {
		perror("fork failed");
		return -1;
		} else if (child_pid == 0) {
			char file_name[10];
			snsprintf(file_name, 10, "%d.txt", i);
			int fd = open(file_name, O_CREAT | O_TRUNC | O_WRONLY, S_IRUSR | S_IWUSR);
			dup2(fd, STDOUT_FILENO);
			close(fd);
			execlp("ls", "ls", location_names[i], NULL);
			perror("execlp failed");
			exit(1);
		} else {
			if (wait(NULL) < 0) {
			perror("wait failed");
			return -1;
			}
		}
	}
}
```

## 5. File I/O
Say we run the following C code on our computer. You may assume for all questions regarding this code that no errors occur, writes are atomic, and that the file out.txt doesn't exist when the code starts executing.
```c

int fd = open("out.txt", O_WRONLY | O_CREAT | O_EXCL);
char *msg1 = "Hello, World!\n";
char *msg2 = "I am having fun!\n";
pid_t child_pid = fork();

if (child_pid == 0) {
	dup2(fd, STDOUT_FILENO); // redircted
	fprintf(stderr, "%s", msg1); // Hello, World! is printed on screen
	write(fd, msg1, strlen(msg1)); // Hello, word! is written on screen
} else {
	wait(NULL); // waits for child -> below code ONLY runs when child is finished
	printf("%s", msg2);  //  I am having fun! is printed on screen
	write(fd, msg2, strlen(msg2)); // I am having fun! is written on out.txt
}
close(fd);
return 0;
```

(a) What is one possible set of contents a user will see on their screen when all processes related to this code finish their execution?
- Hello, World!
- Hello, World!

(b) What is one possible set of contents a user will see in the file out.txt when all processes related to this code finish their execution?
- I am having fun!
- I am having fun!

(c) What is a second possible set of contents a user will see on their screen when all processes related to this code finish their execution? If there are no other possible contents, write "N/A".
- N/A

(d) What is a second possible set of contents a user will see in the file out.txt when all processes related to this code finish their execution? If there are no other possible contents, write "N/A".
- N/A
  
(e) Does the answer to any of the above change if the file out.txt already exists when the code starts executing? Why or why not?
- Yes, an error occurs
## 6.
A Process lifecycle has a fixed number of states and transitions between those states. Name the states and label the transitions between them.
- New:  being created
- Ready: Eligible for execution but waiting for a turn
- Running: In the middle of it's turn on the CPU
- Blocked:  Waiting for some event
- Done: Finished executing, no longer eligible for execution