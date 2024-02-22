---
layout: labscreen
---

# Lab 5
**Instructions embedded at the bottom of this page for your convenience**<br>
[Back to Homepage](..)

---
# Overview




## Organization: A way to gracefully manage your lab
This is not a replacement for the lab instructions. Please only use this as mental scaffolding, as it's not meant to have comprehensive information on the topic.

For a detailed explanation on each step, click the Step Number, which will redirect you to a page with the explanation. Your checkboxes should be preserved if you use the back arrow to return to this page, but you can also control+shift+click the links to open them in a new page.

<input type="checkbox"> Step 1: Read through pages 92-95. (Please actually do this)<br>
<input type="checkbox"> [Step 2](./step2.md): Reuse your code from Lab 1 with a modern makefile, and use the `x /4ubfx addreddInR1` command to check the contents of the memory at the specified address. **Take a screenshot of the memory location**, and discuss the range of memory of the string in your lab report.<br>
<input type="checkbox"> [Step 3](./step3.md): Add any number of Arbitrary Commands AFTER the last `SVC 0` statement, but BEFORE the `.data` section. Take note of the total number of bytes you are adding.<br>
<input type="checkbox"> Step 4: Use the `make DEBUG=1` command again and check the new memory address of the `=helloworld` command. **Take a screenshot of the new location**. In your lab report, explain if the number of Arbitrary Commands you added lines up with the new location in memory.<br>
<input type="checkbox"> [Step 5](./step5.md): Now, add a very large about of bytes in the same place as the Arbitrary Commands from earlier. You will need at least `4096 Bytes` to successfully get the error message described in the lab report. **Take a screenshot of the error message**, which will occur when you try and *make* the program. Describe in your lab report how you made this occur. <br>




## Conclusion
- 
- If you have any more questions, specific or general, make sure to ask a TA.
- If you have any suggestions for this site, email me at **[cshapard@smu.edu](mailto:cshapard@smu.edu).** I made this last night, so I couldn't include as much as I wanted to, but it will be better for next week's lab. Let me know what you want to see/what would help you most!

# Instructions
<object data="Lab5Instructions.pdf" type="application/pdf" width="100%" height="700px">
    <embed src="Lab5Instructions.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="Lab5Instructions.pdf">Download PDF</a>.</p>
    </embed>
</object>


<!-- Credit goes to https://stackoverflow.com/users/2301402/suneel-kumar for the fallback link code --> 


