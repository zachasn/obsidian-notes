# OS Midterm Cheat Sheet

## PROCESS CONCEPTS
**Process States**: New → Ready → Running → Blocked → Done
- **Ready**: Eligible for execution, waiting for CPU turn
- **Running**: Currently executing on CPU
- **Blocked**: Waiting for event (I/O, child exit, etc)
- **Done**: Finished executing

**Process Types**:
- **Zombie**: Child terminated, parent hasn't collected exit status
- **Orphan**: Parent terminated before child → adopted by init
- **Daemon**: Background process providing service

## FORK PATTERNS
**Basic fork()**: Returns 0 to child, child PID to parent, -1 on error

**Pattern 1: Unlimited forking**
```c
for (int i = 0; i < n; i++) fork();
// Creates 2^n processes
```

**Pattern 2: Linear forking**
```c
for (int i = 0; i < n; i++) {
    if (fork() == 0) break;
}
// Creates n+1 processes (1 parent + n children)
```

## EXEC FAMILY
**exec() replaces current process with new program**
- `execl(path, arg, ...)` - list args, full path required
- `execlp(file, arg, ...)` - list args, searches PATH
- `execv(path, argv[])` - array args, full path required
- `execvp(file, argv[])` - array args, searches PATH

**What's preserved**: PID, PPID, open file descriptors (unless FD_CLOEXEC), working directory, resource limits
**What's replaced**: Code, data, heap, stack

## WAIT FUNCTIONS
```c
wait(int *status)              // Wait for any child
waitpid(pid, &status, options) // Wait for specific child
```
**waitpid options**:
- `0` - blocks until child terminates
- `WNOHANG` - returns immediately if no child terminated (polling)
- `WUNTRACED` - blocks until child terminates or stops

**waitpid pid values**:
- `pid > 0` - wait for specific child
- `pid == -1` - wait for any child (same as wait())
- `pid == 0` - wait for any child in same process group
- `pid < -1` - wait for any child in process group |pid|

## FILE DESCRIPTORS & I/O
**Standard descriptors**: 0=stdin, 1=stdout, 2=stderr

**open() flags**:
- Required: `O_RDONLY`, `O_WRONLY`, `O_RDWR`
- Optional: `O_CREAT` (create), `O_APPEND` (append), `O_TRUNC` (truncate), `O_EXCL` (fail if exists)

**Permissions** (for O_CREAT):
- `S_IRUSR` (400), `S_IWUSR` (200), `S_IXUSR` (100) - user r/w/x
- `S_IRGRP` (040), `S_IWGRP` (020), `S_IXGRP` (010) - group r/w/x
- `S_IROTH` (004), `S_IWOTH` (002), `S_IXOTH` (001) - other r/w/x

**File offset**: Position where next read/write occurs
- Initialized to 0 on open (unless O_APPEND)
- Incremented by bytes read/written
- Modified with `lseek(fd, offset, whence)`

**lseek whence values**:
- `SEEK_SET` - offset from beginning
- `SEEK_CUR` - offset from current position
- `SEEK_END` - offset from end

**read() return values**:
- Returns bytes actually read (may be < requested)
- Returns 0 at EOF
- Returns -1 on error

## FILE SHARING
**3 kernel data structures**:
1. **Process FD Table**: Maps fd numbers to system file table entries (per process)
2. **System File Table**: Tracks file status flags, offset, v-node pointer (global)
3. **V-node Table**: File metadata, inode info, data block pointers (global)

**Independent processes**: Each gets own system file table entry → independent offsets
**Parent-child after fork()**: Share same system file table entry → shared offset

## DUP & REDIRECTION
```c
dup(fd)        // Returns lowest available fd
dup2(fd, fd2)  // Duplicates fd to fd2, closes fd2 first if open
```
**Both share same system file table entry** (same offset, flags)

**Redirection pattern**:
```c
int fd = open("out.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
dup2(fd, STDOUT_FILENO);  // Redirect stdout to file
close(fd);                 // Close original fd
```

## STDIO LIBRARY
**Buffering types**:
- **Fully buffered**: Actual I/O when buffer full (disk files)
- **Line buffered**: Actual I/O on newline (stdin, stdout)
- **Unbuffered**: Immediate I/O (stderr)

**fread/fwrite**:
```c
fread(dest, byte_size, count, fh)   // Returns items read
fwrite(src, byte_size, count, fh)   // Returns items written
```

**Preferred approach**: `fread(buf, 1, byte_size, fh)` - returns actual bytes read/written

**fseek**:
```c
fseek(fh, offset, whence)
```
- `SEEK_SET` - offset from beginning
- `SEEK_CUR` - offset from current
- `SEEK_END` - offset from end

**Buffer flushing**: Occurs when buffer full, newline (line buffered), or `fflush()`/`fclose()` called

## KEY GOTCHAS
- **All pointers same size**: 8 bytes on x86-64 (regardless of type)
- **Changes not on disk immediately**: Only when kernel buffer full or flushed
- **read() doesn't guarantee full read**: Always check return value
- **execl() needs full path**: Use execlp() to search PATH
- **O_EXCL with O_CREAT**: Fails if file exists (atomic check-and-create)
- **fprintf to stderr**: Not affected by dup2(fd, STDOUT_FILENO)
- **Zombie prevention**: Always wait() for children
- **fork() duplicates FDs**: Parent and child share system file table entry
