---
title: "Lab 7: Functions and the Stack"
author: Cooper Shapard
date: 2024-04-01
category: Jekyll
layout: post
---

[Download Lab Instructions](https://www.coopshap.com/ECE1181/pages/Lab7_FunctionsStack.pdf)

# Overview
Welcome to Lab 7. Here, you will learn how to use subroutines, a.k.a. functions, to avoid rewriting code multiple times. This may be one of the most valuable/useful concepts you learn relating to ARM Assembly. Functions can take multiple inputs and return multiple outputs and can be called multiple times from anywhere in your code. However, there are a couple different ways that you can send and receive data to and from a function. They are:
1. Pass by Register
- This works by leaving values in the register, then accessing those same values from both functions. It's like putting the values on a desk so that multiple people can work on them/change them without having to move them to a different location.

Pros: Easiest to understand and implement.
Cons: Least amount of space for values and can have side-effects (side-effects mean some values may change that you did not want to change).

2. Pass by Reference
- This works by placing the desired values in memory, then just sending the address to that memory location to the function. It's like putting the values in a storage locker, then sending the key to another person. To get the results, you must go back to the storage locker and grab them yourself.

Pros: Most amount of space and no side-effects.
Cons: Slowest method and allows for possible memory leaks (memory leaks are values that get "stuck" in memory and just take up space without doing anything).

3. Pass by Stack
- This works by pushing all the values onto the stack, then letting the function pop them off as needed. It's like putting the values in a box, then sending the box to someone. That person changes the values, then sends the box with the results back to you.

Pros: Large amount of space and no side-effects or memory leaks.
Cons: Only the top value on the stack can be popped unless you add extra code.

Pass by Stack is the preferred method by professionals because it is fast, free of side-effects/memory leaks, and allows for something called *nested function calls*, which are a very important concept. Here is a written explanation, but check the official lab paper for some diagrams if you're a visual learner.

Nested function calls means that you can call another function within a function. Very useful for efficiency, but it's not as simple as calling the new function and moving on. The Link Register (LR) can only store one value at any given time. So let's say I go into a function with a Branch and Link (BL), setting the link register to my return address. If I then call another function within that function using another BL, then it will overwrite the old LR value, and we get stuck in the function since we don't have a return address. 

The way to fix this is by *pushing* the value of the LR to the stack when we enter a new function and *popping* it when we leave the function. This allows us to store multiple LR values, but only use the one that we need in nay given function. With that, you are ready for the lab!

I have simplified the steps for you below for organizational purposes. Remember, this is not a replacement for the lab guide, just a more readable way to phrase the information. If you need any help, be sure to ask a TA!


# Steps
- Step 1: Read through pages 109-116 in the Smith book. It should be review from class, but please actually read it so you can get a different perspective on functions and the stack.
- Step 2: Use the instructions on page 116 to 121 to make the specified Uppercase Revisited program. Make and run the code to check that it works. If not, ask a TA.
- [Step 3](#step-3): Choose your favorite stack convention (IA, IB, DA, DB), and replace all the PUSH and POP commands in your program with the correct STMxx and LDRyy commands (where xx and yy are your stack conventions).
- Step 4: Make sure your code works the exact same way as it did in Step 2. If not, try to find and fix the issues on your own before asking a TA. **Take a screenshot showing that your program works with your chosen stack convention.**
- Step 5: Read through pages 121-124 in the Smith book. Please actually do this.
- [Step 6](#step-6): Take the code from Listings 6-3 and 6-4 and modify them to fit your stack convention like you did in Step 3. This code uses *pass by register* to input and output the data to and from the function. Currently, R0 and R1 contain the addresses of the input and output string, and after the function executes, R0 is replaced by the length of the string. Your goal is to change this code so that it used *pass by stack* instead, which uses STMxx and LDRyy to send data to the toupper function. Then, use STMxx and LDRyy to get the length of the string into R0 after toupper executes. Note that you will need to manually offset the stack pointer to grab R0 and R1 from the stack, since R4 and R5 will go on top of them. Your offset may be positive or negative depending on your stack convention. It may help to draw out your stack so it is easier to visualize.
- [Step 7](#step-7): If you have not already, modify your STMxx and LDRyy statements to include the Link Register (LR) so that your program returns to the same place after performing the subroutine/function. **Take a screenshot showing your progam works with your new Pass by Stack method.**

# Code Given
No code is given for this lab.

# Explanations
## Step 3
Explain that if you use STMIA, you would want to use LDRDB
## Step 6

## Step 7

