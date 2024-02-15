---
layout: stepguide
---
# Lab 4: Step 3
[Back to Lab 4 Guide](./guide.md) (Try and use the back arrow on your browser. Clicking this removes your checkmarks)

---
# Explanation
You should have gone over different types of index addressing in class. This is your time to implement them on your own!

For this part, you should have 4 groups of lines. I don't have my own red pitaya or book, so I can't check for sure, but the finished product should look something like this:

**Do not assume this works. This is a rough draft meant to show a general shape, not working code. You must implement the changes yourself.**
```ARM
.global _start

_start:
    LDR R1, =mynumber

    @Comparison Template:
    @ Line that loads a {byte/halfbyte/etc} into R2 from [R1, #1]
    @ Line that resets R1 to the address of mynumber
    @ Line that loads a {byte/halfbyte/etc} into R2 from [R1], #1
    @ Line that resets R1 to the address of mynumber

    @ You won't need to change the address of mynumber
    @ between every comparion, so only implement the ones you need

    MOV R0, #0
    MOV R7, #1
    SVC 1


.data
mynumber: .word 0x12345678
```

Then, in the debugger (the gdb command. Make sure you use the `make` command with `DEBUG=1`), you should step over each line that loads a byte into R2, use the `i r` command to see what the value of R2 is, and take a screenshot. In your lab report, compare the two values for each comparison.