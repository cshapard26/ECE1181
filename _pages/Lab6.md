---
title: "Lab 6: Controlling Program Flow"
author: Cooper Shapard
date: 2024-03-20
category: Jekyll
layout: post
---

[Download Lab Instructions](https://www.coopshap.com/ECE1181/pages/Lab6_ControllingProgramFlow.pdf)

# Overview
Welcome to Lab 6! Here, you will find only 4 steps... each with 3 substeps and 4 subsubsteps, so get ready!

In this lab, you will begin to see how a program can execute different parts based on **something called a *branch.*** Before, your program would execute each line in your program, one by one, in order. That was called PC+4 behavior because each command is four bytes long, so the program counter (PC) would go up by four bytes to execute the next line. Now, instead the letting the program counter work on its own, **we can change the PC to whatever location we want.** That allows us to do some really fun things like repeating a section of code multiple times without having to rewrite it over and over or executing large blocks of code without the need for conditional execution (subgt, addlt, etc). 

While this new behavior is very useful, it is also very dangerous. You may move the program counter to a location it's not allowed to execute, or if it stops lining up with the four byte lines of code, that could cause problems that are very hard to identify. That second part is easy to find if you know what you're looking for. **The main thing to note is that the program counter must always be a multiple of four.** Luckily, we use hex numbers, so you can check if a number is divisible by four by checking if the hex number ends in 0, 4, 8, or c. If so, then your program counter is lined up correctly!

I have simplified the steps for you below for organizational purposes. Remember, this is not a replacement for the lab guide, just a more readable way to phrase the information. If you need any help, be sure to ask a TA!


# Steps
- Step 1: Read through all of chapter 4 in the Smith textbook (please actually do this). Then, copy the code from the [code given](#code-given) section into a new file with a name of your choosing. Create a makefile, and update it with the new filename. Use `make DEBUG=1` to make the program with gdb enabled. Test the program using `./your_program_name` (without the .s). The program should work just like Lab 5, but it can take up to 50 characters of input. It will simply output the same characters you put in. Make sure that your program does this correctly.
- [Step 2](#step-2): Similarly to last lab, you need to design an algorithm that changes a lowercase letter to an uppercase letter (but not vice versa this time). However, instead of only uppercasing one letter, you need to be able to check up to fifty letters. *You are not allowed to do this by writing the same code 50 times.* You must use branching, which you read about in chapter four, to execute the same command for the length of the string. Your program should:
    - Loop through each character stored at the `msg` label in the .data section of your code.
    - Use a register to keep track of which character you are on (which I call a "tracker"). So if you have already checked/updated the first 14 characters, you should have a register that has the number 14 in it (or some other way to track where you are in the string, like updating the base register).
    - For each character:
        - Check if it is capitalized. If not, capitalize it, then store it back in the same position. If so, ignore it.
        - If it is not a letter, ignore it.
        - Update the location of your tracker.
- Step 3: **In your lab report, describe how your algorithm works.**
- Step 4: Screenshot at least 3 different tests that showcase your program working. Include various upper and lower case mixtures of your first name, last name, and student ID. **Include the screenshots in your lab report.**

# Code Given
```asm
@ 
@ Assembler program to input ASCII characters via stdin 
@ The program will take a string of up to 50 characters and modify it 
@ such that all lower-cased letters will be made upper-case. 
@ R0-R2 - parameters to linux function services 
@ R7 - linux function number 

.global _start  @ Provide program starting at address to linker 
@ Set up the parameters to retrieve characters and then call Linux to do it. 
_start:   mov R0, #0   @ 0 = StdIn 
   ldr R1, =msg   @address to receive input 
   mov R2, #50   @receive at most 50 characters 
   mov R7, #3   @read 
   svc 0    @ Call linux to receive input 
   mov r2, r0   @captures number of characters input 
 
@ Insert your code here 
 
@ Set up stdout to echo those characters back and then call Linux to do it. 
   mov R0, #1   @ 1 = StdOut 
   ldr R1, =msg   @ string to print 
@ mov R2, #2 @ commented out because you will need to calculate the string length 
   mov R7, #4   @ linux write system call 
   svc 0    @ Call linux to print 
   mov R0, #0    @ Use 0 return code 
   mov R7, #1   @ Service command code 1 
@ terminates this program 
   svc 0           @ Call linux to terminate 

.data 
msg:   .fill 51, 1, '\n'  @ Allocates enough memory for 50 characters 
```

# Explanations
### Step 2

Can you check a condition before doing a branch?

If we are ignoring both uppercase letters and numbers, then what is the only ascii range we need to check?

How can we check if we have reached the end of the string? What is the ascii value of a '\n' (also called a Line Feed)?

Can you reuse any code from Lab 5?
