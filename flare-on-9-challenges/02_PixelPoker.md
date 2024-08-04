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




### Challenge Complete!
