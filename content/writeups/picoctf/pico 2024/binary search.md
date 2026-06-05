---

title: "# binary search"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------
# Description:
Want to play a game? As you use more of the shell, you might be interested in how they work! 

Binary search is a classic algorithm used to quickly find an item in a sorted list. 
Can you find the flag? You'll have 1000 possibilities and only 10 guesses.

Cyber security often has a huge amount of data to look through - from logs, vulnerability reports, 
and forensics. Practicing the fundamentals manually might help you in the future when you have to 
write your own tools!

You can download the challenge files here:
challenge.zip

ssh -p 55705 ctf-player@atlas.picoctf.net
Using the password 83dcefb7. Accept the fingerprint with yes, and ls once connected to begin. 
Remember, in a shell, passwords are hidden!

## Hints:
1. Have you ever played hot or cold? Binary search is a bit like that.
2. You have a very limited number of guesses. Try larger jumps between numbers!
3. The program will randomly choose a new number each time you connect. You can always try again, 
   but you should start your binary search over from the beginning - try around 500. 
   Can you think of why?

# solution
```
ssh -p 62338 ctf-player@atlas.picoctf.net
enter number : 500
```

now try again and get  the flag on 463

```
Enter your guess: 500
Lower! Try again.
Enter your guess: 250
Higher! Try again.
Enter your guess: 390
Higher! Try again.
Enter your guess: 450
Higher! Try again.
Enter your guess: 480
Lower! Try again.
Enter your guess: 465
Lower! Try again.
Enter your guess: 454
Higher! Try again.
Enter your guess: 459
Higher! Try again.
Enter your guess: 460
Higher! Try again.
Enter your guess: 463
Congratulations! You guessed the correct number: 463
Here's your flag: picoCTF{g00d_gu355_de9570b0}
```