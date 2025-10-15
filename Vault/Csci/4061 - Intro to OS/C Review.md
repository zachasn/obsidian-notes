# C Review
## Data Types

|       Type T       |                Description                 | sizeof(T) |
|:------------------:|:------------------------------------------:|:---------:|
|        int         |        Positive or negative number         |     4     |
|      unsigned      |            Non-negative number             |     4     |
|        long        |     Large positive or negative number      |     8     |
|   unsigned long    |         Large non-negative number          |     8     |
|     long long      |   Very large positive or negative number   |     8     |
| unsigned long long |       Very large non-negative number       |     8     |
|       short        |     Small positive or negative number      |     8     |
|   unsigned short   |         Small non-negative number          |     2     |
|       float        |              Fractional value              |     4     |
|       double       | Fractional value, more range and percision |     8     |
|        char        |       Single letter or small number        |     1     |
Sizes here are for Linux with 64-bit architecture
Use `stdint.h`library if you want fixed sized integers
- *int8_t, uint8_t:* 8 bits(1 bytes)
- *int16_t, uint16_t:* 16 bits(2 bytes)
- *int32_t, uint32_t:* 32 bits(4 bytes)
- *int64_t, uint64_t:* 64 bits(8 bytes)
## Expressions
- Any value in c is either `truth`or`false` 
- typically non-zero values are `true`and zero is `flase`
- *for pointers:* the special `NULL`pointer is false, all others `true`
- expressions like assignments have a value too
- *Example:* `x = 92`is interpreted as having value **92**, so `if (x)`and `if (x = 92)`branches are always taken
## Pointers
- Pointers give you visibility into a programs memory
- The value of any pointer is a *memory address*-where some specific piece of data lives
- The type of a pointer tells you what is stored at that address 
	- `int *`→ stores address of an int value
	- `double *`→ stores address of a double value
- *Remember:* memory addresses are just binary numbers, regardless of what lives at that address
	- `sizeof(double *) == sizeof(int *) == sizeof(char *) == ... === 8`
#### Pointer Operations
- `int *a;` → Declares an integer pointer with name *a*
- `&x`; → Yields the address of the variable *x*
- `int *a = &x;`→ Declares an integer pointer *a* with address of *x* as value
- `*a`→ Get/Set the value at address stored in *a* 
- `*a = 42;`→ Dereferences *a* to store value *42* (just changed *x*, not *a* itself)
#### Swap Example
```c
void swap1(int a, int b) {
	int temp = a;
	a = b;
	b = temp;
}
int x = 1;
int y = 0;
swap1(x,y);
```
- No actual swap occurs here, because a copy of `x`and `y`are passed into the faction, not the actual variables.
- When the function ends, the copies are destroyed and `x` and `y` remain unchanged.
- you decide whether `C`is **pass-by-reference** or **pass-by-value**
## Arrays — Memory Layout
- `C`arrays are guaranteed to  store their elements in *contiguously* in memory → this makes them very cache-friendly
- Name of the array by itself is **memory address where array starts**
- ```c
  int x = 4;
  int* p = &x;
  int a[3] ={8,15,16};
  int* ap = a;
  ```

| Address | Symbol | Value |
| :-----: | :----: | :---: |
|  #3014  |   ap   | #3002 |
|  #3010  |  a[2]  |  16   |
|  #3006  |  a[1]  |  15   |
|  #3002  |  a[0]  |   8   |
|  #2994  |   p    | #2990 |
|  #2990  |   x    |   4   |
### Array/Pointer Duality
- Arrays and pointers in `C`are closely relate but not purely equivalent
- **Convention:** Prefer pointers for function parameter types → `f (int* a)`, not `f (int a[])`
- You then can use either brace notation or pointer  arithmetic
- `p[i]`is equivalent to `*(p + i)` → "give me the string address offset by size `i`elements"
## Structs:  Heterogeneous Data
- **Structs** are `C`'s way of defining a heterogeneous collection of data
- Access elements with dot (`.`) notation
- *Pointer to a struct* → use arrow (`->`) notation
```c
typedef struct {// declare type
	double height;
	int age;
	char name[16];
} person_t;

person_t wizard; // declare variable
wizard.height = 6.0; 
wizard.age = 394930;
wizard.name = "Gandalf";


person_t wizard = {// declare and define
	.height 6.0;
	.age 394930;
	.name "Gandalf";
};
```
## Strings in `C`
- There is no dedicated `string`type in `C`, instead we use `char`arrays
- You can also use pointers and string literals → a string declared this way is *immutable*
```c
// char arrays
char char_ary_name[10] =  {'C', 'S', 'C', 'I', ' ', '4', '0', '6', '1', '\0'}; 

// string literal
char* class_name = "CSCI 4061";
```
### `strtok` Function
```c
char* strtok(char* str, const char* delim)
```
- Tokens are the chunks of the original string between delimiters, this function "remembers" information from previous call
1.  Call `strtok`with original string and delimiter → first token returned
2. To keep parsing same string, call `strtok`with `NULL`as first argument
3. Repeat until `strtok`returns NULL → nothing left to parse
### Whats wrong with this code?
```c
char** split(char* string, char* delim) {
	char* string[50];
	// < Do some strtok magic to fill up the array >
	return string;
}
```
- `String` is declared on the function stack, so when the function returns and all local variables go out of scope, the pointer returned will point to invalid memory
## Heap Memory
- Data on the heap exists outside the scope of any function call
- We can ask for exactly the space we need at runtime using `void* malloc(size_t size)`
- *size* is the number of **bytes** you need, returns a *void pointer* so you can assign it to any type of pointer
- `malloc()`is often used in combination with `sizeof()`

> [!NOTE]
> - You must free any memory you allocate using `malloc()`!
## Files 
 A`FILE`is a *structure* () that contains :
```c
// Simplified view of what's inside FILE
struct _IO_FILE {
    char* buffer;           // I/O buffer
    char* buffer_end;       // End of buffer
    char* read_ptr;         // Current read position
    char* write_ptr;        // Current write position
    int fd;                 // Underlying file descriptor
    int flags;              // Status flags
    // ... many more fields
};
```
- When you call `fopen()`for example, it:
	1. It calls `open()`system call to get a *file descriptor*
	2. Allocates a `FILE`structure
	3. Allocates a *buffer* (usually 4 or 8KB)
	4. Returns a pointer to this `FILE` structure
- A **stream** is an abstract concept - it's the flow of data between the program and file/device
- A **file descriptor** is the kernel's handle to the file
##### Buffering
- Instead of every `fgetc()`,`fputc()`triggering a system call, the standard I/O uses buffers
- Buffering matters because system calls are expensive(context switch to kernel mode)
```c
// Read example
FILE* fh = fopen("ab.txt", "r");
char c = fgetc(fh); // first call
```
1. `fgetc()`checks if there's data in the `FILE`'s buffer
2. Buffer is empty, so it calls`read()`system call to fill the entire buffer (e.g 4kb)
3. Returns the first character from buffer
4. Next 4095(4kb) calls to get`fgetc()`just return data from the buffer → *No system call happens*
```c
// write example
FILE* fh = fopen("ab.txt", "w");
char c = fputc('a', fh); // first call
```
1. Char`a`goes into the `FILE`'s buffer
2. When buffer fills up **OR** `fflush()`/`fclose()`is called, then `write()`system call is happens
##### Types of Buffering
- **Fully Buffered:** Actual`read()`and`write()`take place when `FILE`'s buffer is full
- **Line Buffered:** Actual`read()`and`write()`when a newline is encountered
- **Unbuffered:** No buffering, every operation triggers a system call 
##### File Operations 
- Any actively used file in a `C`program has a "file handle" of type `FILE *`
- You must pass a file handle as a parameter for any file-related function calls
- **To open a new file:** `FILE* fopen(char* name, char* mode)`→ *mode* is "r" for reading, "w" for writing, "a" for appending
- **To close a file:** `fclose(FILE* handle)`
- **Error Handing:** `fopen()`returns `NULL`on error;`fclose()`returns `EOF`on error
##### The File Abstraction
- A *file* is a name sequence of bytes, stored persistently on some device 
- Two general types of file, depending on contents
1. **Text File:** Bytes are all printable characters
	- Convenient → human readable
	- Portable → most architectures represent text data identically (ASCII & Unicode standards)
	- Inefficient → usually larger than binary files, have to parse values (e.g. convert the string "5922" to a binary int)
2. **Binary File:** Bytes are unformatted, identical to the in-memory representation of each type
	- Less convenient → not human readable, can't just open and edit
	- Less portable → how do I know how many bytes represent an int?
	- Efficient → more compact than text file, load directly into appropriate data types, no parsing needed
	
##### Reading/Writing Text Files
- Use `fscanf`to read, `fprintf`to write
```c
int fscanf(FILE* fh, char* format, addr1, addr2, ...);
```
- returns a special `EOF`value when end of file is reached
```c
int fprintf(FILE* fh, char*format, arg1, agr2, ...);
```
##### Reading/Writing Binary Files
- Little bit different → use `fread`and `fwrite`
- `fread`and`fwrite`are best to use regardless of the type of file
```c
size_t fread(void* dest, size_t byte_size, size_t count, FILE* fh);
```
- Attempts to read `byte_size *`count bytes from open file to location at `dest`
- Note the `void *` → we can read into whatever pointer type we want
```c
size_t fwrite(void* src, size_t byte_size, size_t count, FILE* fh);
```
- Attempts to write `byte_size *`count bytes to open file form location `src`
- Note the `void *` → we can write from whatever pointer type we want
##### Two different ways
```c
// Approach 1: Read/write 1 element of byte_size bytes
// returns 1 (success) if it reads/writes complete byte_size
// returns 0 (failure) if it reads ANYTHING less than byte_size
fread(dest, byte_size, 1, fh); 
fwrite(src, byte_size, 1, fh); 
//          ^size      ^count


// Approach 2: Read/write byte_size elements of 1 byte each
// returns the actually number of bytes (from 0 up to byte_size) read/written
fread(buf, 1, byte_size, fh); 
fwrite(src, 1, byte_size, fh); 
//          ^size      ^count

```
- Approach 2 is almost always better because it handles partial reads/writes and gives you useful information about how much data is actual read/written
##### Text File Example
```c
#include <stdio.h>
int main() {
	FILE* fh = fopen("my_integer.txt", "w");
	if (fh == NULL) {
		printf("Failed to open file\n");
		return 1;
	}
	int my_integer = 42;
	fprintf(fh, "%d\n", my_integer);
	fclose(fh);
	return 0;
}
```
##### Binary File Example
```c
#include <stdio.h>
int main() {
	FILE* fh = fopen("my_integer.bin", "wb");
	if (fh == NULL) {
		printf("Failed to open file\n");
		return 1;
	}
	int my_integer = 42;
	fwrite(&my_integer, sizeof(int), 1, fh);
	fclose(fh);
	return 0;
}
```
##### Exercise
- Implement `int write_people_to_text(char* file_name, person_t* people, int n_people)`and `int write_people_to_binary(char* file_name, person_t* people, int n_people)`
```c
typedef struct {
	double age;
	int height;
	char initial;
} person_t;

int write_people_to_text(char* file_name, person_t* people, int n_people) {
	FILE* fh = fopen(file_name, "w");
	if (fh == NULL) {
		return 1;
	}
	for (int i = 0; i < n_people; i++) {
		fprintf(fh, "%lf %d %c\n",people[i].age,people[i].height,people[i].initial);
	}
	fclose(fh);
	return 0;
};	
```

```c
int write_people_to_binary(char* file_name, person_t* people, int n_people) {
	FILE* fh = fopen(file_name,"wb");
	if (fh == NULL) {
		return -1;
	}
	for (int i = 0; i < n_people; i++) {
		fwrite(&people[i], sizeof(person_t),1,fh);
	}
	fclose(fh);
	return 0;
};
```

### Moving Around in a File
- Any open file keeps track of a **position:** starting point for the next read/write operation, usually measured as number bytes from file's start
- Usually start at position 0 when a file is first opened
- Reads and writes implicitly advance this position by number of bytes consumed/produced

![[file_pos.png]]
- We can also explicitly ask to adjust the current position using:
```c
int fseek(FILE* f, long offset, int whence);
```
- *offset:* How far to move, can be positive or negative
	- Positive → move forward towards the end
	- Negative → move backwards towards the beginning
- *whence:*  Determines starting point for the seek (what value *offset* is added to)
	- SEEK_SET → *offset* is relative to beginning of file
	- SEEK_CUR → *offset* is relative to the current position in file
	- SEEK_END → *offset* is relative to end of file
- Use `ftell()`to get the current position
![[fseek.png]]