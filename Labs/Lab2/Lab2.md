---
title: "Lab 2: Tooling Up"
author: Cooper Shapard
date: 2024-09-03
category: Jekyll
layout: post
---

[Download Lab Instructions](https://www.coopshap.com/ECE1181/pages/Lab2_ToolingUp.pdf)

# Overview
Welcome to Lab 2! Here, you will learn the basics of a makefile and how to use the debugger to learn more about your program. Both of these skills are CRUCIAL for your success in the course, and you will use both in every single lab after this one. If you don't understand something or want further clarification, let a TA know and we can help you out. Also, if you need a refresher on any of the skills you learned last lab session, you can always check the [Lab 1 guide](/ECE1181/pages/Lab1/) again!

# Explanations
## Step 1
First, read through pages 53-56 in the Smith textbook and follow the instructions on creating a makefile. This is similar to the "build" file in the last lab, but it will be much easier to edit if needed. You will likely reuse this same file in every lab from here on out, so make sure it's working properly before continuing. 
 
Next, either a make copy of your "HelloWorld.s" (may also be called "main.s") file from Lab 1 or just edit it directly. You want to change the text at the bottom so that it now says "Hello <first name> <last name>! (built from a makefile)". Be sure to update the length of the string in R2!

If everything went correctly, you should be able to just type in "make" into the terminal to build the file. If no red words pop up on the screen, odds are it worked. If not, ask a TA for assistance.

**Take a screenshot of your terminal showing that you built the file using `make` and that running `./HelloWorld` (or whatever your filename is) gives the output "Hello [first name] [last name]! (built from a makefile)".**

## Step 2
Create a new file called "movexamps.s" and fill it with the code from the [Code Given](#code-given) section.

You don't need to fully understand how this code works at this point, but it's helpful to at least know what it's doing. The `MOV` command is placing the value "0x6E" in the register R2 in the rightmost 16 bits. The `MOVT` command is placing the value "0x4F" in the register R2 in the left most 16 bits. The reason we cannot do this with one command is that the MOV command can only take up to eight bits (one byte) as input. Thus, to make the large value "0x4F006E", we need to separate it into two commands. The way we do it here is an uncommon way to do it, and you won't see the `MOVT` instruction again in this course. You will soon learn about the `LDR` instruction, which is much more versatile.

Follow the instructions on pages 57-62 in the Smith book. You will become VERY familiar with this process from now on. Commit it to memory, as you will use it in every lab from here on out. As a summary:
1. Type `gdb <file name without the extention (.s, .o, etc)>`. This should show the debug terminal, denoted by the "(gbd)" at the beginning of the line.
2. Type `b _start` to set a breakpoint to the start of your program. You can see this breakpoint directly in the code. To set different breakpoints, simply change the command to reflect the name of the breakpoint you want to set.
3. Type `r` to run the program. It will automatically stop at your first breakpoint. In this case, it is the start of your program.
4. Here, you will see a line like `_start: MOV R2, #0x6E`. To EXECUTE this line (it has not been done yet), type `s`, which stands for "step." Every time you type `s`, it will execute one line of code. Do this as many times as needed to get where you want (or set a separate breakpoint).
5. At any time, type `i r` (for Inspect Registers) to check what values are stored in all registers. This screen also contains a lot more helpful information, which you will learn about more in the coming labs.
6.  This step isn't particularly useful for this lab, but you can type `x \4ubfx <memory address>` to check the contents of the memory at a specific location. You will learn more about this in a later lab.

Now, open up your "movexamps.s" file and change the code so that the first four digits of your SMUID appear in the register. So, if your SMUID is 0x12345678, you want it to appear in R2 as 0x120034. You should only need to change two numbers. *Make sure to `make` your file again after every edit.* Next, use the debug protocol listed above to run this system and show that the value of R2 is 0x120034 (or whatever your ID is). **Take a screenshot showing the debug process and the first digits of your SMUID in R2.**

## Step 3



# Conclusion



# Code Given
makefile (**WARNING: Make sure that the lines that start with "as" and "ld" both have a TAB before them. Four spaces will not work.**):
```
OBJS = movexamps.o

ifdef DEBUG
DEBUGFLGS = -g
else
DEBUGFLGS =
endif

%.o : %.s
    as $(DEBUGFLGS) $< -o $@
movexamps: $(OBJS)
    ld -o movexamps $(OBJS)
```
movexamps.s:
```asm
@
@ Examples of the MOV instruction.
@
.global _start      @ Provide program starting address
                    @ Load R2 with 0x4F006E first using MOV and MOVT
_start: 
    MOV R2, #0x6E
    MOVT R2, #0x4F

    MOV R0, #0      @ Use 0 return code
    MOV R7, #1      @ Service command code 1
    svc 0           @ Call Linux to terminate
```

# Troubleshooting
1. Check the filename. Is it "moveexamps" or "movexamps"? Either works, but it has to stay consistent throughout the program.
2. Did you write "movexamp.s" instead of "movexamps.s"? This is the most common mistake we see.