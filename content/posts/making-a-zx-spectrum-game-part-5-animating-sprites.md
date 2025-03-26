---
title: "Making a ZX Spectrum Game - Part 5 - Animating Sprites"
date: 2020-12-08T14:10:11+01:00
draft: true
categories:
- Personal
tags: 
- 8-bit
- speccy
- tutorial
- zx-spectrum
---

![Awakeman Sprite Sheet](/images/posts/awakeman-sprite.png "Awakeman Sprite Sheeet")

## Setting up Fourspriter

Add a reference to the Fourspriter library and the sprite sheet containing all the player walk animations.

```basic
#include once "fsp2.1.bas"
#include once "spritesheet.bas"
```

First off, I need to tell Fourspriter where to get the graphics from; this is done by calling ```fsp21SetGfxAddress``` with reference to the spritesheet as a parameter.

```basic
fsp21SetGfxAddress (@spritesheet (0))
```

The Fourspriter library will update the screen itself (when the update function is called), but it needs to know which graphics to use, their color, where to position it, and whether it is active or not.

```basic
fsp21SetGfxSprite(3, 0, 1, 2, 3)
fsp21ColourSprite(3, 7, 7, 7, 7)
```

Here I configure to use the third sprite on the sprite sheet with the color 71 (Ink + Bright*64) for all four 8x8 tiles that make up the sprite (identified as block 0, 1, 2, and 3).

I also need to define where to position the sprite; this is done using ```fsp21MoveSprite```.

```basic
fsp21MoveSprite(3, x, y)
fsp21DuplicateCoordinatesSprite(3)
```

This will move the sprite to the position indicated by the x and y position. ```fsp21DuplicateCoordinatesSprite()``` is only necessary to call the first time the sprite is placed on screen; this will correctly set two internal variables that store where the sprite was before moving it.

Finally, I need to activate the sprite using ```fsp21ActivateSprite``` and call ```fsp21InitSprites``` to capture the background behind each active sprite before animating it.

```basic
fsp21ActivateSprite(3)
fsp21InitSprites()
```

I have placed all of these Fourspriter routines in initScreen(); here is the final code:

```basic
Sub initScreen()
    Border 0: Paper 0: Ink 7: Bright 0: Cls
    Print At 23,0;"Q/A/O/P - up/down/left/right"
    Poke uInteger 23675, @udg (0)

    'Setup Fourspriter
    fsp21SetGfxAddress(@spritesheet (0))
    fsp21SetGfxSprite(3, 0, 1, 2, 3)
    fsp21ColourSprite(3, 7, 7, 7, 7)
    fsp21MoveSprite(3, x, y)
    fsp21DuplicateCoordinatesSprite(3)
    fsp21ActivateSprite(3)
    fsp21InitSprites()

    updateScore(0)
    generateLimes(5)
End Sub
```

One last thing is to replace the "A" character with the new sprite in the main game loop.

```basic
Do
    Asm
        halt
        halt
    End Asm

    movePlayer()

    'Draw Player
    fsp21MoveSprite(3, x, y)
    fsp21UpdateSprites()
Loop
```

When compiling and running the code in the emulator, I can now see a graphical representation of Awakeman, instead of the "A" character previously used, and I can move the player character around on the screen as before.

However, something isn't quite right. When the player passes the limes on screen, they are not being "picked up" although a new lime is generated and the score is increased by one, which only happens when the player passes the lime in the top/left corner of the sprite. In addition the "Score" counter is now in green (Ink 4) and the player can move off the screen to right. The latter is easily fixed by adding Ink 7 to the ```updateScore``` sub routine and have the player stop when 1 < x < 30 (instead of 0 and 31). To fix the former, I need to extend the collision detection.

Originally, the player was only represented by a single 8x8 character tile, with Fourspriter the player is now four 8x8 tiles, which combined make up a 16x16 sprite. Here is the revised collision detection, where I check for limes in every four corners of the character.

```basic
If limes(y,x) = 1 Then eatLimeAt(y,x)
If limes(y+1,x) = 1 Then eatLimeAt(y+1,x)
If limes(y,x+1) = 1 Then eatLimeAt(y,x+1)
If limes(y+1,x+1) = 1 Then eatLimeAt(y+1,x+1)
```

OK great, I got Fourspriter up-and-running and the gameplay is still intact, now it is time to animate when moving the player around the screen.

## Animating Awakeman

Adding animation to the prototype is fairly simple using the Fourspriter library, it is simply a matter of setting the correct sprites using ```fsp21SetGfxSprite``` based on current direction and animation cycle.

First off, I've created two variables, one to represent the current animation cycle (or step) and one to represent which direction the player is facing.

```basic
Dim pStep,pFacing as Byte
```

Next, I've created a sub routing that increment steps to indicate which frame in the animation cycle to use.

```basic
Sub doStep () 
	pStep = pStep + 1: If pStep = 4 Then pStep = 0: End If
End Sub
```





[1]: https://www.mojontwins.com/juegos_mojonos/fourspriter-1-0/
[2]: https://bitbucket.org/zxbasic/mojon-twins/src/master/lib/fourspriter/
[3]: https://www.mojontwins.com/juegos_mojonos/fourspriter-1-0/tutorial-de-zx-basic-fourspriter/

[4]: http://oldmachinery.blogspot.com/2014/04/zx-sprites.html