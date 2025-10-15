# Virtual Memory

## Key Concepts
- **Address Space**: Range of memory addresses visible to a process and their contents, one per process
- **MMU (Memory Management Unit)**: Hardware that maps virtual addresses to physical addresses
- **Virtual Address**: Address used by a program (what the CPU generates)
- **Physical Address**: Actual address in physical RAM
- **Page**: Fixed-size block of virtual memory
- **Frame**: Fixed-size block of physical memory (same size as page)
## Physical Addressing
CPU sees exact same address used by memory system:
- Process knows where everything lives
- Process can see entirety of memory
- Process knows how much space is available in physical memory

### Problems with Physical Addressing
1. **No isolation**: Processes can access each other's memory
2. **No protection**: Malicious/buggy programs can corrupt OS or other processes
3. **Limited flexibility**: Hard to relocate programs in memory
4. **Fragmentation**: Memory becomes fragmented over time
## Virtual Addressing
CPU generates virtual addresses that are translated to physical addresses by the MMU.
### Benefits
- **Isolation**: Each process has its own address space
- **Protection**: OS controls which memory regions processes can access
- **Flexibility**: Programs can be located anywhere in physical memory
- **Illusion of large memory**: Processes can use more memory than physically available (via disk)
- **Sharing**: Multiple processes can share memory safely
### Address Translation
![[vm.png]]
The MMU uses a **page table** to perform this translation.
## Paging
Memory is divided into fixed-size blocks called **Pages** each containing 4 KiB (4096 bytes):
- Map each chunk a process sees and uses to the slot in memory that actually stores it
- Map each virtual page of a process to a page frame in physical memory
- Applied to both physical and virtual memory 
### Page Table
Data structure that stores the mapping from virtual pages to physical frames.

**Page Table Entry (PTE)** contains:
- **Valid bit**: Is this page in physical memory?
- **Physical frame number**: Where is the page located?
- **Protection bits**: Read/write/execute permissions
- **Present bit**: Is page in memory or swapped to disk?
- **Dirty bit**: Has the page been modified?
- **Referenced bit**: Has the page been accessed recently?
### Address Translation Process
A virtual address is split into two parts:
```
| Virtual Page Number (VPN) | Offset |
```

1. Extract VPN from virtual address
2. Use VPN as index into page table
3. Get Physical Frame Number (PFN) from page table entry
4. Concatenate PFN with offset to get physical address

**Example** (32-bit address, 4KB pages):
- Page size: 4KB = 2^12 bytes
- Offset: 12 bits (lower bits)
- VPN: 20 bits (upper bits)

```
VA: | 20-bit VPN | 12-bit offset |
PA: | 20-bit PFN | 12-bit offset |
```
## Translation Lookaside Buffer (TLB)
**Problem**: Page table is in memory, so each memory access requires 2 memory accesses (1 for page table, 1 for actual data).

**Solution**: TLB - a hardware cache for page table entries.
### TLB Operation
1. Check TLB for VPN
2. **TLB Hit**: PFN found in TLB, use it directly
3. **TLB Miss**: Look up page table in memory, update TLB
### TLB Properties
- Small and fast (typically 64-512 entries)
- Fully associative or set-associative
- High hit rates (>99%) due to locality
- Flushed on context switch (or tagged with process ID)
## Multi-Level Page Tables
**Problem**: Single-level page table can be huge (e.g., 4MB for 32-bit address space with 4KB pages).

**Solution**: Hierarchical page tables (2-level, 3-level, or 4-level).
### Two-Level Page Table
```
| Page Directory Index | Page Table Index | Offset |
```

1. Use page directory index to find page table
2. Use page table index to find PFN
3. Combine PFN with offset

**Advantages**:
- Only allocate page tables for used parts of address space
- Saves memory for sparse address spaces
## Page Faults
When a process accesses a page not in physical memory:
1. **Page fault exception** triggered
2. OS finds page on disk (swap space)
3. OS finds free frame (or evicts a page)
4. OS loads page from disk into frame
5. OS updates page table
6. OS restarts faulting instruction
### Types of Page Faults
- **Minor fault**: Page in memory but not in page table (e.g., first access)
- **Major fault**: Page must be loaded from disk
- **Invalid fault**: Access to unmapped memory (segmentation fault)
## Page Replacement Algorithms
When memory is full, which page should be evicted?
### FIFO (First In, First Out)
- Evict oldest page
- Simple but inefficient
- Suffers from Belady's anomaly
### LRU (Least Recently Used)
- Evict page that hasn't been used for longest time
- Good performance but expensive to implement perfectly
- Approximated with clock algorithm or aging
### Clock Algorithm (Second Chance)
- Circular list of pages with reference bit
- If reference bit = 1, set to 0 and move to next
- If reference bit = 0, evict page
### Optimal (MIN)
- Evict page that won't be used for longest time
- Impossible to implement (requires future knowledge)
- Used as theoretical benchmark
## Working Set and Thrashing
- **Working Set**: Set of pages a process is actively using
- **Thrashing**: System spends more time paging than executing
  - Happens when total working set size > physical memory
  - Solution: Reduce multiprogramming level, add more RAM
## Memory-Mapped Files
Map file contents directly into process address space:
- File I/O through memory operations (loads/stores)
- OS handles paging file in/out
- Multiple processes can share mapped files
- Used for: shared libraries, IPC, efficient file I/O
## Copy-on-Write (COW)
Optimization for `fork()`:
- Parent and child share same physical pages initially
- Pages marked as read-only
- On write, page is copied (lazy copying)
- Saves memory and time
## Summary
| Feature | Physical Memory | Virtual Memory |
|---------|----------------|----------------|
| Address Space | Shared | Per-process |
| Protection | None | MMU-enforced |
| Relocation | Hard | Easy |
| Memory Size | Limited by RAM | Limited by address space |
| Performance | Fast | Slower (TLB helps) |
| Isolation | None | Strong |

