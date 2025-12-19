# OS Assignment 1

**Authors:** Nalin Gupta, Tanish Jindal  
**Course:** CSE231: Operating System **(IIITD)**

## Introduction

This document describes the design and implementation of a Simple Loader in C that can load and run a 32-bit ELF executable without using external libraries.

## Approach

Our implementation follows the following steps:

1. Run checks on the ELF file using `access()`.
2. Read the ELF file header using `open()` and `read()`.
3. Locate the *Program Header* using `e_phoff`.
4. Parse program headers to locate `PT_LOAD` segments.
5. Allocate memory using `mmap()` for each segment.
6. Find the entry point for the `_start()` function.
7. Use `launch.c` to execute the `test` files.

## Implementation

### 1. Run checks on the ELF file:

```c
if (access(argv[0], F_OK) != 0) {
    fprintf(stderr, "Error: File '%s' not found.\n", argv[0]);
    exit(1);
```

```c
if (access(argv[1], F_OK) != 0) {
    fprintf(stderr, "Error: File '%s' not found.\n", argv[1]);
    exit(1);
}
// Similar checks using R_OK as well
```

The `access` system call checks the accessibility of a file. The flag `F_OK` tests whether the file exists. The flag `R_OK` tests whether the file is readable or not.

### 2. Reading the ELF file header:

```c
int fd = open(filename, O_RDONLY);
// Before creating ehdr check if open() worked
Elf32_Ehdr ehdr;
  if (read(fd, &ehdr, sizeof(ehdr)) != sizeof(ehdr)){
      perror("ehdr is faulty");
      loader_cleanup();
      exit(1);
}
```

This command stores the content of the ELF Header in `ehdr` and checks for any errors.

### 3. Locate the *Program Header*:

```c
off_t e_phoff = ehdr.e_phoff;
lseek(fd, e_phoff, SEEK_SET);
```

The `lseek` system call moves the file descriptor `fd` to the position `e_phoff`, relative to the start of the file (`SEEK_SET`).

### 4. Locating `PT_LOAD` segment and allocating memory:

```c
for (int i = 0; i < ehdr.e_phnum; i++) {
    read(fd, &phdr, sizeof(phdr))
    Elf32_Phdr phdr;
    if (phdr.p_type == PT_LOAD &&
        ehdr.e_entry >= phdr.p_vaddr && ehdr.e_entry < (phdr.p_vaddr + phdr.p_memsz)) {
        segmentMemory = mmap(NULL, segmentSize, PROT_READ | PROT_WRITE | PROT_EXEC, MAP_PRIVATE | MAP_ANONYMOUS, 0, 0);
    }}
```

**`ehdr.e_entry >= phdr.p_vaddr && ehdr.e_entry < (phdr.p_vaddr + phdr.p_memsz)`**, this condition checks whether the ELF entry point (`e_entry`) lies inside the virtual address range of this segment. `phdr.p_vaddr` = starting virtual address of the segment. `phdr.p_memsz` = size of the segment in memory.

Together, the range is `[p_vaddr, p_vaddr + p_memsz)`. Use `mmap()` to allocate memory.

### 5. Calculate entry point address for `_start` function:

```c
void* entry_point = (void*)((char*)segmentMemory + (ehdr.e_entry - phdr.p_vaddr));
```

**`(char*)segmentMemory`** is cast to a `char*` so that pointer arithmetic works at the byte level.

## AI Generated Code Snippets

The following Code Snippets were added with the help of AI:

### 1. Entry Point Calculation

```c
void* entry_point = (void*)((char*)segmentMemory + (ehdr.e_entry - phdr.p_vaddr));
```

### 2. Function Pointer Typecast

```c
int (*_start)() = (int (*)())entry_point;
```

A typecast of the raw pointer `entry_point` into a function pointer that takes no arguments and returns an `int`.

### 3. Memory Deallocation

```c
if (segmentMemory) {
    munmap(segmentMemory, segmentSize);
}
```

The `munmap` command is similar to the `free()` command. It is used to deallocate memory, which was allocated using `mmap`.

### 4. Makefile Configuration

```makefile
all:
    gcc -m32 launch.c -L../bin -l_simpleloader -Wl,-rpath,'$$ORIGIN' -o ../bin/launch
```

The following flags are used in the Makefile for building the `launch` executable:

- **`-L../bin`**: Adds the `../bin` directory to the library search path, where `lib_simpleloader.so` is located.
- **`-l_simpleloader`**: Links against the shared library `lib_simpleloader.so`. By convention, the `-l` flag looks for a library named `lib<name>.so`.
- **`-Wl,-rpath,'$$ORIGIN'`**: This sets the runtime library search path so that the executable can find `lib_simpleloader.so` in its own directory at runtime.

## Contributions

- **Nalin Gupta:** Implemented MakeFiles, Error Handling, Documentation.
- **Tanish Jindal:** Implemented ELF Parsing, Loader Logic, Memory Mapping.

### Github Repository

Private repository link: https://github.com/chill-nalin/OS-Assignment-1

### References

1. Lecture 02 and 03 slides.
2. https://man7.org/linux/man-pages/man5/elf.5.html
3. https://wiki.osdev.org/ELF_Tutorial
4. https://www.spec.org/cpu2017/flags/gcc.2018-02-16.html
5. https://www.youtube.com/watch?v=BM62xi4FE3c
