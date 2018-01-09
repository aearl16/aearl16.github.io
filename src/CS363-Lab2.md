---
title: CS363 Lab 2 Walkthrough
---

# CS363 Lab2 Walkthough (Fall 2016)

## Compiling and testing the vulnerable application


```c 
//
//  vulnerable.c
//  
//
//  Created by Yanwei Wu on 10/12/17.
//

#include <stdio.h>
#include <string.h>

void foo (char *arg) {
    char buffer[64];
    strcpy(buffer, arg);
}

int main(int argc, char *argv[]) {
    foo(argv[1]);
    return(0);
}
``` 

### Compiling the C Application

```sh
gcc -g vulnerable.c -o vulnerable.x
```
The -g option enables the debugging mode built into the gcc compiler. The -o tells 
the compiler how to name the compiled executable.

### Testing the C Application

```sh
./vulnerable.x A
```

This will not overflow the buffer and will run normally.

```sh
./vulnerable.x AAAAAAAAAAAAAAAAAA
```

This will cause a buffer overflow and segmentation fault. This can be exploited to create 
the shellcode injection attack.

## Writing Application using Intel x86 Assembly
For this project we're going to be creating an assembly application that returns the 
user ID of the current Linux shell user.

Here is the program in C

```c
#include <stdlib.h>
#include <stdio.h>

void main()
{
    system("id");
}
```

This will look like this in ASM

```asm
section .text
global _start
_start:
    ; Clear out all of the registers
    ; xoring registers with themselves nulls them out
    xor eax, eax ; technically using eax 8-bit equivalent
    xor ebx, ebx
    xor ecx, ecx
    xor edx, edx
    ; pushing a null character onto the stack. Since shell code
    ; will not allow the use of null characters we're pushing the value
    ; of a register that is known to contain a null value.
    push    eax
    ; here we push the command we wish to execute using execve. In this
    ; case, "/usr/bin/id". In this case the characters don't fit nicely
    ; into a register so we're going to add a slash to the string as
    ; follows:
    ; "/usr/bin//id"
    ; Remember: This is a stack so we're going to be pushing this string onto the stack in reverse.
    push    0x64692f2f ; di//
    push    0x6e692f2f ; nib/
    push    0x7273752f ; rsu/
    ; Now we move the value of the stack pointer into ebx.
    mov ebx, esp
    ; Then we place a null onto the top of the stack after the
    ; current edx stack pointer.
    push    edx
    ; No we move the current stack pointer address into ecx.
    mov ecx, esp
    ; Now we null edx
    xor edx, edx
    ; Here we move the kernel shell call for execve (11) into al.
    ; Note: al is the 8-bit version of the eax register.
    mov al, 0xb
    int 0x80
```

Another good resource for ASM tutorials can be found here: [ASM Tutor]("http://asmtutor.com/")

### Experimenting with BASH
First we're going to run the id application in BASH so we can see how it works.

```sh 
root@Hacking_Live:/# /usr/bin/id
uid=0(root) gid=0(root) groups=0(root)
```

id is the application we want to run with our shell code. To start this process we're
going to write some x86 assembly that we can turn into shell code.

```asm

```
  

## Creating Shell Code from x86 Assembly Code

## 