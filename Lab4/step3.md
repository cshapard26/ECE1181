---
layout: stepguide
---
# Lab 4: Step 3
[Back to Lab 4 Guide](./guide.md) (Try and use the back arrow on your browser. Clicking this removes your checkmarks)

---
# Explanation
Most coding languages have something called "wrapper" code. Like a candy wrapper, it surrounds the code and tells you something about what is inside. There are many variations of wrapper code, but for the ARM Assembly Language in this class, we use the following:
```ARM
.global _start

_start: 

    @ Your code goes here!

    MOV R0, #0
    MOV R7, #1
    SVC 0

@ Any .data or other information goes down here!
```

I'm a bit short on time to explain why we put each of these things here, but I'll do so in a later lesson!
