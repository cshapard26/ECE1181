---
title: "Pseudocode for Lab 6, Step 2"
author: Cooper Shapard
date: 2024-10-03
category: Jekyll
layout: post
---
```assembly
@ Load the address of msg into a register.
@ Decide which register will keep track of your string locaiton (the "tracker").
@ 
@ Start loop.
@    Load the current character from msg into a register.
@    Check if it is '\n'. If so, print all the characters in msg from 0 to the tracker and end the program.
@    Check if it is a lowercase letter (betwen 0x61 and 0x7A, inclusive). If so:
@       Make it uppercase like you did in Lab 5.
@       Store it back into the same address you took it from.
@    If it is not lowercase, then don't do anything.
@    Update the tracker.
@    Branch to the start of the loop.
```