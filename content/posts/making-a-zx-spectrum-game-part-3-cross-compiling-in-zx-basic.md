---
title: "Making a ZX Spectrum Game - Part 3 - Cross Compiling in ZX Basic"
date: 2020-11-24T10:22:22+01:00
draft: false
categories:
- Personal
tags: 
- 8-bit
- speccy
- tutorial
- zx-spectrum
---

The time has come to look into cross compiling my code for the ZX Spectrum. As I alluded to in [part 1][1] of the series, I intend to use [ZXBASIC][2] for cross compiling, which also means I can basically take (no pun intended) the prototype already writen in Sinclair BASIC and rewrite it in ZXBASIC. 

The advantage of ZXBASIC, besides compiling to assembler, is that it is also a modern take or dialect on BASIC (especially when compared to Sinclair BASIC), where it tries to keep some of the original Sinclair BASIC features, which definitely eases the transition from one language to the other, but it also offers features of a cross compiled language, such as data types and the possibility to do inline assembly code.

Let's have a look at the prototype code:

```basic
100 REM Init Game
101 LET x = 16
102 LET y = 11
103 LET s = 0
110 BORDER 0
 
150 REM Generate Lemons
151 FOR l=1 to 5
152 PRINT AT INT(RND*20)+1,INT(RND*31);"@"
153 NEXT l
 
200 REM Main Game Loop
201 PRINT AT y,x;"A"
202 PRINT AT 0,0;"Score: ";s
 
210 LET vx = 0
211 LET vy = 0
212 LET k$ = INKEY$
213 IF k$ = "q" THEN LET vy=-1
214 IF k$ = "a" THEN LET vy=1
215 IF k$ = "o" THEN LET vx=-1
216 IF k$ = "p" THEN LET vx=1
 
250 IF vx = 0 AND vy = 0 THEN GOTO 210
 
260 PRINT AT y,x;" "
 
270 LET x = x+vx
271 IF x < 0 THEN LET x = 0 272 IF x > 31 THEN LET x = 31
 
280 LET y = y+vy
281 IF y < 1 THEN LET y = 1 282 IF y > 21 THEN LET y = 21
 
290 IF SCREEN$(y,x) = "@" THEN LET s = s+1:PRINT AT INT(RND*20)+1,INT(RND*31);"@"
 
300 GOTO 200
```

My super simple game play in only 29 lines of code. Alright so let's try cross-compiling the existing prototype code with ZXBASIC.

## Porting the Code to ZXBASIC

To start with, let's just try to compile the prototype code as is with the ZXBASIC compiler.

```sh
zxbc.py prototype.bas
```

This gives the following output in the terminal:

```sh
prototype.bas:2: warning: [W100] Using default implicit type 'ubyte' for 'x'
prototype.bas:3: warning: [W100] Using default implicit type 'ubyte' for 'y'
prototype.bas:4: warning: [W100] Using default implicit type 'ubyte' for 's'
prototype.bas:17: warning: [W100] Using default implicit type 'ubyte' for 'vx'
prototype.bas:18: warning: [W100] Using default implicit type 'ubyte' for 'vy'
prototype.bas:37: error: Undeclared function "SCREEN$"
```

Based on the compiler warnings, it seems that the global variables needs to be cast to a specific type, by default they are cast as ubyte. Unfortunately, I was unsuccesful in compiling the code due to the error of an undeclared function "SCREEN$". The latter is something that can easily be fixed by referencing the screen library function using the following include statement.

```basic
# include <screen.bas>
```

In addition, let's set x,y,s,vx, and vy to relevant types. In this case I'll use Integer for all of them.

```basic
DIM x,y,s,vx,vy AS INTEGER
```

I've made these changes in a new file called prototype-2.bas, let's try to compile it.

```sh
zxbc.py prototype-2.bas
```

I now get a few compiler warnings that parameters 'row' and 'col' is never used from the included screen.bas library, but other than that the compiler outputs a prototype-2.bin file. By adding the compiler instructions "-t -B -a", I tell the compiler to respectively create a .tap file, to include a BASIC loader which loads the rest of the code, and sets the program to run once loaded.

```sh
zxbc.py prototype-2.bas -t -B -a
```

After loading the game into the emulator, it is apparent that something is not functioning as it should.

![Screenshot of the game after compilation with ZXBASIC](/images/posts/awakeman-prototype-2.png "Screenshot of the game after compilation with ZXBASIC")

Besides the "Bytes: Prototype-" message on screen, our characters moves  crazy fast around on screen. We can fix the former issue just by calling ```CLS```, which basically clears the screen before rendering the character and "limes", and we can slow down the character movements, by directly calling the assembly halt command.

```basic
CLS

Asm
    halt
    halt
End Asm
```
## Refactoring the Code

As you may have noticed, I have not been using any line numbers in the additions to the code, I have just made. This is because ZXBASIC does not require to write any line numbers. Line numbers are allowed, but are rather treated as labels. I would like to go through the code and remove all line numbers, and thereby also replacing goto loops with appropriate while loops and adding subroutines and possibly functions to clean up the code a bit.

Here is the refactored code:

```basic
# include <screen.bas>

'Global Variables
DIM x,y,s,vx,vy AS INTEGER

LET x = 16
LET y = 11
LET s = 0
LET vx = 0
LET vy = 0

Sub updateScore(n AS INTEGER)
    LET s = s+n:
    PRINT AT 0,0;"Score: ";s
End Sub

Sub generateLemons(n AS INTEGER)
    INK 6
    FOR l=1 to n
        PRINT AT INT(RND*20)+1,INT(RND*31);"@"
    NEXT l
End Sub

Sub initScreen()
    BORDER 0
    PAPER 0
    INK 7
    BRIGHT 0
    CLS
    PRINT AT 23,0;"Q/A/O/P - up/down/left/right"
    updateScore(0)
    generateLemons(5)
End Sub

Sub movePlayer()
    LET vx = 0
    LET vy = 0

    LET k$ = INKEY$
    IF k$ = "q" THEN LET vy=-1
    IF k$ = "a" THEN LET vy=1
    IF k$ = "o" THEN LET vx=-1
    IF k$ = "p" THEN LET vx=1

    IF vx <> 0 OR vy <> 0 THEN
        PRINT AT y,x;" "

        LET x = x+vx
        IF x < 0 THEN LET x = 0
        IF x > 31 THEN LET x = 31
        LET y = y+vy
        IF y < 1 THEN LET y = 1
        IF y > 21 THEN LET y = 21

        IF SCREEN$(y,x) = "@" THEN
            updateScore(1)
            generateLemons(1)
        END IF
    END IF
End Sub

initScreen()

'Main Game Loop
While 1
    'Draw Player
    INK 7
    PRINT AT y,x;"A"

    Asm
        halt
        halt
    End Asm

    movePlayer()
    
End While
```

I created four subroutines to better organize the code; one to generate lemons - ```generateLemons(0)```, one to update the score - ```updateScore(0)```, one to initialize the screen - ```initScreen()```, and finally one to update the player movement based on keyboard input - ```movePlayer()```. 

BASIC is case-insensitive, so it does not really matter whether I write e.g. "IF" or "if". At the moment it is a mix of everything, but I think I'll eventually settle on camel case for my own subroutines and pascal case for language identifiers (variable names, function names, subroutine names, and labels); this will be a job for future me.

In addition, I've added a description text in the bottom of the screen, and I've also added a bit more styling to the game; the background is now black and the player is white and the lemons are yellow.

![Screenshot of the game after refactoring](/images/posts/awakeman-prototype-3.png "Screenshot of the game after refactoring")

Next, I will investigate how to add some graphics and possibly animations to the prototype.

[1]: https://boblbobl.com/2020/11/10/making-a-zx-spectrum-game-part-1-my-development-setup/
[2]: https://github.com/boriel/zxbasic