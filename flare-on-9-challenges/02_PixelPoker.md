# Flare-On 9
## Challenge 02: PixelPoker
For these challenges, I will be using Kali Linux as my environment

### Initial Thoughts
The first thing I noticed is that unzipping the file produce an executable. So, naturally, I decided to run it. However, this is when I learned that Kali Linux does not have a built in way to run executables, so I 
did some research and installed Wine, which allows you to run executables using:
```
$ wine executable_name.exe
```
So, after getting that installed and configured I was able to boot up the exe.


### The Challenge
When you run the executable, it simply opens a window that is full of colored dots/pixels. As you start to click around, you see your guess counter at the top go up, and after 10 incorrect guesses the window aborts. So,
it would appear that the challenge we need to solve here is that we need to find the correct pixel to click... CRAZY! There are what seems to be a couple hundred thousand pixels in this exe, and we need to click the
SINGULAR correct one.


### Solution Process
I have some previous experience working with executables in Ghidra to decompile, so I decided that would be my first step. I wanted to see if anything in the assembly code could point me in the right direction. So,
I opened up the exe in Ghidra and got to work.

To understand the inner workings of PixelPoker.exe, I started by inspecting the WinMain function. Here’s a high-level overview of the function's flow:
  -Load a Resource Image: The image used to set the window's dimensions.
  -Register and Create a Window Class: This involves functions sub_401120 and sub_401040.
  -Enter Window Message Dispatch Loop: Where the main interaction logic resides.

So now that I understood how the window was and loading were created, I could better comprehend how to find the correct pixel. I decided to follow the functions until I stumbled accross the subroutine that held the 
game logic. The reason I knew this was because I found the condition for the game failure: if your guesses hit 10, it prints "Womp Womp". So, observing this subroutine further, I found the win condtion: 
```
if ((uVar8 == (uint)s_FLARE-On_00412004._0_4_ % DAT_00413280) &&
 (uVar6 == (uint)s_FLARE-On_00412004._4_4_ % DAT_00413284))
```

Tracing the variable names, I found that uVar8 was equivalent to the x coordination of the clicked pixel and uVar6 was the y coordinate. This meant that the answer was right in front of me; all I had to do was complete
the modulo calculation in the code snippit above and that would give me the x and y coordinates of the correct pixel!

### A Lesson in Debugging
So this is where I ended up spending most of my time on this challenge. I had found the win condition rather quickly, and even managed to get the value of the integer on the left side of the modulo. However, the part
that stumped me were the two variables ```DAT_00413280``` and ```DAT_00413284```. After deep inspection, I understood that these represented the window width and height respectively. I thought to myself it would be simple,
just find where the instantiated the window size in the code and bingo, we got it! That's where the issue was... they never assigned the variable in the code, only accessed it.

I wasn't exactly sure how this was possbile, but I figured it had to be populated at some point. So, I attempted to use Ghidra, IDA Pro, GDB, and a ton of other debugging solutions but none of them would work. I came to
the realization, it was because this was a 32-bit executable, and I was running a 64-bit system. so, I found a 32-bit debugger, Ollydbg. Using this, I was able to dynamically analyze this executable and set breakpoints.
I traced the width and height variables at their respective memory addresses and finally got the values:
```
Width = 741
Height = 741
```
This meant I could finally get the correct pixel's coordinates:
```
correct_x = 95
correct_y = 313
```

So, testing it out, I clicked that pixel and voila, challenege completed!


### Challenge Complete!
