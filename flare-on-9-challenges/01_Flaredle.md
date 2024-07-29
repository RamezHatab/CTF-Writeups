# Flare-On 9
## Challenge 01: Flaredle
For these challenges, I will be using Kali Linux as my environment

### Initial Thoughts
Starting off, I unzipped the file and noticed that there were html, js, and css files. This led me to believe I would be interacting with a webpage. So, using that information, I loaded up a local web server to observe the challenge.
```
python -m http.server
```
Using this command, I was able to open up http://localhost:8000/ in my web browser and interact with the page.


### The Challenge
Upon loading the webpage, you'll notice it is a replica of the popular game Wordle, but instead this version only accepts 21-letter words as guesses. 
Deeming this essentially impossible to solve regulary since I know about zero 21-letter words, I figured the first place to look was obvious: the source code.

### The Source Code




