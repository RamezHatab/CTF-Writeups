## Challenge 03: Magic 8 Ball
For these challenges, I will be using REMnux and FlareVM as my environments

### Initial Thoughts and the Challenge
When you open the executable for this challenge, you get a magic 8 ball game. It allows you to input a question, shake the ball using the arrow keys, then it will
produce some random 8 ball response. I figured that the challenge for this game would be to input the correct sentence to get a flag out of the 8 ball. With that
as my sparting assumption, I dove right in and opened the file in Ghidra.


### Intuition and Discovering the Main Function
My first intuition was that the program had plenty of strings embedded within it, mainly the ones that the 8 ball would spit out whenever you asked it a question.
That being said, these strings had to be stored somewhere, so I opened up the defined strings window and found the function where they were defined.

![image](https://github.com/user-attachments/assets/091b0d3f-c561-44e6-9a19-421b492b9156)


This was an excellent starting point, since I found a function (**FUN_4027a0**) that used this string_def function which I presumed to be the main (aka where the main game logic occured).


### Understanding the Main
My presumptions that the strings led me to the main function was reinforced when I looked at the code: I could see where the title was implemented, alongside some SDL functions which were responsible for things like input and event handling based on documentation online. So, I started jumping into the subroutines that existed
between the SDL_GetTicks functions. Within one of them, I found this piece of code:

![image](https://github.com/user-attachments/assets/fbc43210-dd3f-42d1-ad62-17676b222763)

It became clear to me after seeing this that not only would you need to input a correct string to get the flag, but you would also need to shake the ball in a
certain pattern which was defined here. So, writing this pattern down, I begain to chase the input string I would need.


###Debugging in IDA
I realized the simplest way to get the string I would need would be through a comparison. At some point the code would have to compare the input string to the 
correct string to produce the flag, so I set a breakpoint at this strncmp 

```iVar2 = strncmp((char *)_Str1, (char *)((int)param_1 +0x5c), 0xf);```

and ran the program. To get to the string comparison, we would need to input the correct sequence of movements I found earlier. I put in a random input and shook the ball in the appropriate pattern, and hit my breakpoint. I navigated to where my input was being stored, and found the string it was comparing it to:

![image](https://github.com/user-attachments/assets/6e8319ef-e2b5-4dd8-a920-a75f248f0b4a)

Upon discovering this, I input the string "gimme flag pls?" and shook the ball in the sequence LLURULDUL and got my flag.

![image](https://github.com/user-attachments/assets/a70d0659-d91f-4379-9913-3eeec9a81a5c)


### Challenge Complete!
