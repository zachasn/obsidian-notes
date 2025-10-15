# File Systems
## Key Concepts
- **Absolute Path:** Specifies how to reach file from the *root* directory, always starts with a leading `/`, e.g., `/home/jack/csci4061/syllabus.txt`
- **Relative Path:** Specifies how to reach file from the *current* directory
- **Data Block:** The contents of a file
- **File System:** Data structure that organizes and stores files on storage devices
- **I-node (Index Node):** Data structure containing metadata about a file (permissions, ownership, size, timestamps, pointers to data blocks)
- **Directory:** Special file where it's **data block** is a mapping of filenames to i-node numbers
- **Hard Link:** Directory entry that points to an i-node; multiple hard links can reference the same i-node
- **Symbolic Link:** Special file where it's **data block** is a pathname to another file
- **File Descriptor:** Integer that uniquely identifies an open file in a process
## Stat
The `stat` structure contains information about a file returned by the `stat()`, `fstat()`, `fstatat()`, and `lstat()` system calls.

```c
struct stat {
	mode_t    st_mode;    /* File type and permissions */
	ino_t     st_ino;     /* I-node number */
    dev_t     st_dev;     /* ID of device containing file */
    dev_t     st_rdev;    /* Device ID (if special file) */
    nlink_t   st_nlink;   /* Number of hard links */
    uid_t     st_uid;     /* User ID of owner */
    gid_t     st_gid;     /* Group ID of owner */
    off_t     st_size;    /* Total size in bytes */
    time_t    st_atime;   /* Time of last access */
    time_t    st_mtime;   /* Time of last modification */
    time_t    st_ctime;   /* Time of last status change */
    blksize_t st_blksize; /* Block size for I/O */
    blkcnt_t  st_blocks;  /* Number of 512B blocks allocated */
};
```

**System Calls:** All four return 0 on success, -1 on error
- `int stat(const char *pathname, struct stat *buf)` - Get file status by pathname
- `int fstat(int fd, struct stat *buf)` - Get file status by file descriptor → file needs to be opened
- `int lstat(const char *pathname, struct stat *buf)` - Like stat, but for symbolic links returns info about the link itself
- `int fstatat(int fd, const char *restrict pathname, struct stat *restrict buf, int flag)` - Get file status relative to directory file descriptor; if `pathname` is absolute, `fd` is ignored; `flag` can be `AT_SYMLINK_NOFOLLOW` to behave like `lstat()` 

## File Types
Most files are either regular or directory but there are additional types :
- **Regular File:** Most common type of file, kernel has idea whether data in this file is text or binary , interpretation of the contents are left to the application processing it
- **Directory File:** File that contains the names of other files and pointers to information on these files, processes need *read* permission to read the contents, only the kernel can *write* directly to it
- **Block Special File:** Type of file providing buffered I/O access in fixed-size units to devices
- **Character Special File:** Type of file providing unbuffered I/O access in variable-size units to devices
- **FIFO:** Used for communication between processes
- **Socket:** Used for network communication between processes
- **Symbolic Link:** File that points to another file
The type of a file is encoded in the `st_mode` member of the *stat* structure and you can determine the type of file with the following macros, the argument to each is `st_mode`: 
- `S_ISREG()` → Regular
- `S_ISDIRC()` → Directory
-  `S_ISCHR()` → Character Special
- `S_ISBLK()` → Block Special
-  `S_ISFIFO()` → FIFO
- `S_ISLNK()` → Symbolic Link
- `S_ISSOCK()` → Socket
## File Access Permissions
There are nine permission bits for each file, divided into three categories:
1. **User:** Refers to the owner of the file
	- `S_IRUSR` → User read
	- `S_IWUSR` → User write
	- `S_IXUSR` → User execute
2. **Group:** Refers to users in the file's group
	- `S_IRGRP` → Group read
	- `S_IWGRP` → Group write
	- `S_IXGRP` → Group execute
3. **Other:** Refers to all other users (not owner, not in group)
	- `S_IROTH` → Other read
	- `S_IWOTH` → Other write
	- `S_IXOTH` → Other execute
	
These categories are used in various ways by different system calls:
- To open any types of file by name, you need execute permission in each directory (including the current directory) mentioned in the name; you also need appropriate permission for the file itself(read-only, write-only, append, etc)
- The read permission for a file determines whether you can open an existing file for reading;`O_RDONLY` and `O_RDWR` flags for the `open()` sys call
- The writing permission for a file determines whether you can open an existing file for writing; `O_WRONLY`, `O_RDWR`, and `O_TRUNC` flags for the `open()` sys call
-  Cannot create a new file or delete an existing file in a directory unless you have write and execute permissions in the the directory
- Execute permission for a file must be on to execute any of the seven `exec` functions; The file also has to be a *regular* file
##### File Access Tests
The kernel performs a *file access test* each time a process opens, creates, or deletes a file. This test depends on the owners of the file (`st_uid` and `st_gid`) and the effective user ID and effective group ID of the process.

**Test Process (performed in order):**
1. **Superuser check:** If effective user ID of the process is 0 (root), access is allowed
2. **Owner check:** If effective user ID of the process equals the owner ID (`st_uid`), access is determined by user permission bits
3. **Group check:** If effective group ID of the process or one of the supplementary group IDs of the process equals the group ID of the file (`st_gid`), access is determined by group permission bits
4. **Other check:** Access is determined by other permission bits

**Important Notes:**
- Tests are performed in order and stop at first match
- If process owns file but owner permissions deny access, access is **denied** (group/other permissions are not checked)
- The `access()` and `faccessat()` system calls test accessibility using **real** user ID and **real** group ID instead of effective IDs

## I-node
*I-nodes* (index nodes) are fixed-sized entries that contain most of the metadata about a file. Each file has exactly one i-node and most of the information in the *stat* structure is obtained from the i-node

**I-node Contents:**
- File type (regular, directory, symbolic link, etc.)
- File's access permissions (read, write, execute for owner, group, others)
- Owner's user ID (UID) and group ID (GID)
- File size in bytes
- Number of hard links pointing to this i-node
- Timestamps (access time, modification time, status change time)
- Pointers to data blocks where file content is stored

**Key Properties:**
- I-nodes are identified by i-node number (unique within a file system)
- Directories map filenames to i-node numbers
- Multiple directory entries (hard links) can point to the same i-node
- Deleting a file removes a directory entry; the i-node and data blocks are freed only when link count reaches 0
- I-node does **not** contain the filename (stored in directory entries)

![[i-node.png]]
**i-node array:** A collection of *actual i-nodes* data structure
**i-node number:** the index of that tells you which slot in the array contains a particular file's i-node
**directory block:**  Contains of the directory, just a mapping of filenames to i-node numbers (the index where file's i-node is in i-node array)
**Data Block Pointers:**
- Direct pointers: Point directly to data blocks (typically 12)
- Single indirect: Points to block containing pointers to data blocks
- Double indirect: Points to block containing pointers to indirect blocks
- Triple indirect: Points to block containing pointers to double indirect blocks
> [!summary]
> When you rename a file: the i-node, i-node number, and file's data block stay the same. Only the directory's data block is updated.
## Directories
A directory stores a list of entries detailing its contents:
- Each entry stores a **name** and an **i-node number**
- Entry could be a file or a subdirectory

**Special Directory Entries:**
- `.` (dot) → Current directory (links to directory's own i-node)
- `..` (dot-dot) → Parent directory (links to parent's i-node)

**Key Properties:**
- Directories are special files where the data block contains a mapping of names to i-node numbers
- Only the kernel can write directly to directory files
- Processes need read permission to list directory contents
- Processes need execute permission to access files within a directory

##### Directory System Calls
There is no `writedir()` because **only the kernel can write directly to directory files**.
**Opening/Closing Directories:**
- `DIR *opendir(const char *pathname)` - Opens directory stream, returns pointer to DIR structure; returns NULL on error
- `int closedir(DIR *dirp)` - Closes directory stream; returns 0 on success, -1 on error

**Reading Directory Entries:**
- `struct dirent *readdir(DIR *dirp)` - Reads next directory entry; returns pointer to dirent structure or NULL at end/error
- `void rewinddir(DIR *dirp)` - Resets directory stream to beginning
- `long telldir(DIR *dirp)` - Returns current location in directory stream
- `void seekdir(DIR *dirp, long loc)` - Sets position for next `readdir()` call

**The dirent Structure:**
```c
# defined in <dirent.h> 
struct dirent {
    ino_t d_ino;           /* I-node number */
    char  d_name[256];     /* Null-terminated filename */
};
```

**Creating/Removing Directories:**
- `int mkdir(const char *pathname, mode_t mode)` - Creates new directory; returns 0 on success, -1 on error
- `int rmdir(const char *pathname)` - Removes empty directory; returns 0 on success, -1 on error

**Changing Directories:**
- `int chdir(const char *pathname)` - Changes current working directory using pathname
- `int fchdir(int fd)` - Changes current working directory using file descriptor
- Both return 0 on success, -1 on error

**Getting Current Directory:**
- `char *getcwd(char *buf, size_t size)` - Retrieves absolute pathname of current working directory; returns buf on success, NULL on error

## File Operations  〰 What happens?
- **Making a New File:**
	- Allocate a new i-node
	- Add a new directory entry
- **Modify a File:**
	- Allocate/free data blocks to match new file size
	- Change contents in the relevant data blocks
- **Rename a File:**
	- Modify the directory entry to reflect the new filename
	- No need to change i-node or data blocks
- **Move a File to Different Directory:**
	- Remove directory entry from old directory
	- Add directory entry to new directory
	- i-node and data blocks remain unchanged
## Links
Since a file's metadata and contents are distinct from its name or location,  you can create additional edges in the file system using two types of links: 
- hard links 
- symbolic links (soft links)
##### Hard Links
A **hard link** is a directory entry that points directly to an i-node. Multiple hard links can reference the same i-node, meaning multiple filenames can refer to the same file data.

**Key Properties:**
- All hard links to a file are equal (no "original" vs "copy")
- Each hard link increases the i-node's link count (`st_nlink`)
- File data and i-node are only deleted when link count reaches 0
- Hard links must be on the same file system (can't cross file system boundaries)
- Cannot create hard links to directories (prevents circular directory structures)
- Deleting one hard link doesn't affect other links to the same i-node

**System Call:**
- `int link(const char *oldpath, const char *newpath)` - Creates new hard link; returns 0 on success, -1 on error
- `int unlink(const char *pathname)` - Removes directory entry and decrements link count; returns 0 on success, -1 on error

**Example:**
If `/home/file1.txt` and `/home/file2.txt` are hard links to the same i-node:
- Both files share the same i-node number, size, permissions, and data blocks
- Modifying content through either name affects both
- `st_nlink` = 2 in the stat structure
- Deleting `file1.txt` leaves `file2.txt` intact with all data preserved

##### Symbolic Links
A **symbolic link** (symlink) is a special file whose data block contains a pathname string pointing to another file.

**Key Properties:**
- Symlinks have their own i-node (separate from target file)
- Can cross file system boundaries
- Can link to directories
- Can point to non-existent files ("dangling" or "broken" symlinks)
- If target file is deleted, symlink becomes broken
- File operations typically follow the symlink to the target (except `lstat()`, `unlink()`)

**System Call:**
- `int symlink(const char *target, const char *linkpath)` - Creates symbolic link; returns 0 on success, -1 on error
- `ssize_t readlink(const char *pathname, char *buf, size_t bufsiz)` - Reads contents of symbolic link; returns number of bytes placed in buf, -1 on error

**Example:**
If `/home/link.txt` is a symlink pointing to `/home/original.txt`:
- `link.txt` has its own i-node containing the path string `"/home/original.txt"`
- `stat("link.txt", &buf)` returns info about `original.txt`
- `lstat("link.txt", &buf)` returns info about the symlink itself
- If `original.txt` is deleted, `link.txt` becomes a broken symlink

| Feature                  | Hard Link               | Symbolic Link                  |
| ------------------------ | ----------------------- | ------------------------------ |
| Points to                | I-node directly         | Pathname (string)              |
| Cross file systems       | No                      | Yes                            |
| Link to directories      | No                      | Yes                            |
| Own i-node               | No (shares with target) | Yes                            |
| Breaks if target deleted | No                      | Yes                            |
| Performance              | Slightly faster         | Slightly slower (extra lookup) |
