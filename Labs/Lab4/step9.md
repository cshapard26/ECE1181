---
layout: stepguide
---
# Lab 4: Step 9
[Back to Lab 4 Guide](./guide.md) (Try and use the back arrow on your browser. Clicking this removes your checkmarks)

---
# Explanation
#### Student ID in Hex
To transfer your student ID from Decimal to Hex, simply prefix it with "0x".<br>
So, if your student ID is "12345678", then it would be "0x12345678" in hex.

#### Remake the program
Use the commands you have been using throughout this semester to "remake" the program. Make sure to include the `DEBUG=1` in the line `make -B DEBUG=1`!

#### Disassemble the program
I talked with Will, and for this part, you <u>don't</u> need to use the `disassemble` or `objdump` commands. All you need to do is use the `x /4ubfx AddressInR1` command and take a screenshot showing your Student ID in reverse order in memory.
