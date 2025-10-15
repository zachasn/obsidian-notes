# Low Level I/O
## Key Concepts
- **Buffer:** A temporary chunk of memory where we store information that was read in or is about to be sent out
- **Buffering:** The technique of accumulating pending I/O operations until there is a chunk to  `read`/`write`
- **Fully Buffered:** Defer actual `read`/`write` until a buffer (maintained within kernel) is full → *files on disk*
- **Line Buffered:** Defer actual `write` only when a newline is encountered → *stdin, stdout*
- **Unbuffered:** Data is written immediately → *stderr*
- To the kernel, **everything** is a file and all open files are referred to by **file descriptors**
- **file offset:** An integer that measures the number of bytes from the beginning of the file
- **File Descriptors:** 
	- A file descriptor is a non-negative integer from 0 to `OPEN_MAX -1`,  where `OPEN_MAX-1`is system dependent
	- Every process comes with these three file descriptors automatically:
		- 0 → standard input (`STDIN_FILENO`)
		- 1 → standard output (`STDOUT_FILENO`)
		- 2 → standard error (`STDERR`)
## File Sharing
The kernel uses three data structures to represent an open file, and the relationships
among them to determine the effect one **process** has on another with regard to file sharing.
1. **Process File Descriptor Table:** Each process maintains its own table mapping file descriptor numbers (0, 1, 2, ...) to entires in the system file table
2. **System File Table:** Global table that contains one entry for each open file, each entry tracks:
	-  **File status flags:** `O_RDONLY`, `O_WRONLY`, `O_RDWR`, `O_APPEND`, etc 
	- **Current file offset:** where the next read/write will occur
	- **V-node pointer:** pointer to the actual file metadata
3. **V-node Table:** Contains the actual file metadata (inode information), including file type, size, permissions, and pointers to the actual data blocks on disk.
 
**Key Point:** Multiple file descriptor can point to the same file, but they may have *independent* or *shared* file offset depending how they were opened.
![[file_sharing.png]]
###### Scenario 1: Independent Processes open the same file
```c
#include <fcntl.h>
// Process A
int fd = open("data.txt", O_RDONLY ); 

// Process B
int fd = open("data.txt", O_RDONLY ); 
```
- Each process gets its own *file descriptor* 
- Each process gets its own *system file entry from* `open` but both entries point to the same *v-node* (file)
	- *result:* Because each process has its own independent *file offset*, they can read/write/seek from different positions without affecting each other
###### Scenario 2: Parent-Child relationship
```c
int fd = open("data.txt", O_RDONLY);
if (fork() == 0) {
    // Child process
    read(fd, buf, 10);  // increments file's offset by 10
}
// Parent process
read(fd, buf, 10);  // Reads from position 10
```
- After `fork()`, both parent and child's file descriptor tables point to the same *system file entry*
- *result:* They share file's *offset*, so read/write in one process affects where the other will read/write/seek next
## System Calls
##### `open()`
```c
#include <fcntl.h>
int open(const char *path, int oflag, mode_t mode );
```
- `path` is name of the file to open or create
- `flag` is a bit vector indicating which options we want to enable:
	- *Required (one must be specified):* `O_RDONLY`, `O_WRONLY`, `O_RDWR`
	- *Optional:* `O_CREAT` (create if doesn't exist), `O_APPEND` (append mode), `O_TRUNC` (truncate to 0), `O_EXCL` (fail if exists with O_CREAT)
- `mode` specifies permissions when creating a new file and is only required when using `O_CREAT`
- *Permissions:*
	- `S_IRUSR`(400) → User Read 
	- `S_IRGRP` (040) → Group Read
	- `S_IROTH` (004) → Other Read
	- `S_IWUSR` (200) → User Write
	- `S_IWGRP` (020) → Group Write
	- `S_IWOTH` (002) → Other Write
	- `S_IXUSR` (100) → User Execute
	- `S_IXGRP` (010) → Group Execute
	- `S_IXOTH` (001) → Other Execute
- file offset is initialized to 0, unless `O_APPEND` is specified
- returns a file descriptor on success (guaranteed to be the lowest number unused descriptor) or 1 on error
###### `close()`
```c
#include <fcntl.h>
int close(int fd);
```
- returns 0 on success or -1 on error
- When a process terminates, all of its open files are closed automatically
###### `lseek()`
```c
#include <fcntl.h>
off_t lseek(int fd, off_t offset, int whence);
```
- explicitly set an open file's offset
- *offset:* How far to move, can be positive or negative
- *whence:*  Determines starting point for the seek (what value *offset* is added to)
	- `SEEK_SET` → file's offset is set to *offset* bytes from the beginning of the file
	- `SEEK_CUR` → file's offset is set to it's current value + *offset*
	- `SEEK_END` → file's offset is set to the size of the file +*offset*
- returns new file *offset* on success, -1 on error
###### `read()`
```c
#include <unistd.h>
ssize_t read(int fd, void *buf, size_t nbytes);
```
- Starts  from the file's current offset, increments file's offset by the number of bytes actually read before a successful return
- returns number of bytes read on success, 0 if end of file, -1 on error
- can return less bytes than requested:
	- if end of file is reached before requested number of bytes have been read.
	- interrupted by a signal and a partial amount has already been read
###### `write()`
```c
#include <unistd.h>
ssize_t write(int fd, const void *buf, size_t nbytes);
```
- returns numbers of bytes written ( usually equal to nbytes) on success, -1 on error
- starts at the file's current offset, unless `O_APPEND` is specified
- After a successful write, the file's offset is incremented by the number of bytes written
###### `dup`&`dup2`
Create duplicate file descriptors that point to the same open file entry in the system file table, both return new file descriptor on success, -1 on error
```c
#include <unistd.h>
int dup(int fd);

int dup2(int fd, int fd2);
```
- Creates a copy of file descriptor `fd`
- Returns the **lowest available** file descriptor number
- Both `fd` and `new_fd` share the **same system file table entry**
```c
#include <unistd.h>
int dup2(int fd, int fd2);
```
- Duplicates `fd` to a **specific** descriptor number `fd2`
- If `fd2` is already open, it's **closed first** (atomically)
- Returns `fd2` on success
##