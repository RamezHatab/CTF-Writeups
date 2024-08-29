# Flare-On 9
## Challenge 01: Flaredle
For these challenges, I will be using REMnux and FlareVM as my environments

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
The files that stuck out instantly were words.js and script.js, as they others were css and html files meaning they were just in charge of formatting and visualizing the website. The js files however contained actual code for tha games. So, first I inspected
words.js and saw it was simply a list of 21-letter words that the game would use to select the secret word and validate your guesses. Realizing that wouldn't be insanely helpful, I then moved to script.js, and this is where all the magic happened.

Analyzing the code at face value, I first went and read each of the function names, looking for the function that was in charge of validating the guess as that was the entire challenge: try and reverse engineer the correct 21-letter word. After finding the logic for
checking the guess in the checkGuess() function, I noticed it used a global variable named rightGuessString. Going back up to the top of the file, I saw that the variable was set by grabbing index 57 from the words.js array.

With this discovery, I understood that since it was a global variable it was never going to change between runs, and that all I had to do was go to the words.js file and find the word at index 57. The word at that index was 'flareonisallaboutcats', and after testing that
in the flaredle, it was the correct answer! I successfully reverse engineered the solution to the wordle challenge by inspecting the source code!

### Challenge Complete!



