---
title: "Making a ZX Spectrum Game - Part 4 - Graphics"
date: 2020-12-01T10:11:10+01:00
draft: false
categories:
- Personal
tags: 
- 8-bit
- speccy
- tutorial
- zx-spectrum
---

I have now come to the point, where I would like to add a bit more graphics to the prototype. Instead of having basic characters act as standins for the graphics, why not do the actual graphics themselves. To start with, I would like to render a lime instead of the "@" character.

Before I can begin, I need to better understand the graphics capabilities of the ZX Spectrum and what options there are.

## A ZX Spectrum Graphics Primer

The ZX Spectrum had a screen resolution of 256×192 divided into 32×24 grid overlay where each 8x8 character block can contain only one foreground (Ink) - and one background (Paper) color. And speaking of colors, the ironically named ZX Spectrum only boost a color palette of 8 colors, or 16 if you also count the "bright" variants.

I wrote a small Sinclair BASIC program that showcase all the available colors of the ZX Spectrum.

![Screenshot of all the 16 colors of the ZX Spectrum](/images/posts/colours.bas.png "Screenshot of all the 16 colors of the ZX Spectrum")

The ZX Spectrum does not have any support for hardware sprites, but what it does have is the ability to create a user-defined graphics (UDG) characters, which in my case would work great for replacing the "@" character with an actual 8x8 graphic.

## Designing an 8x8 Lime

Designing an UDG character is easy; take a 8x8 pixel grid, mark the pixels that represent the foreground (Ink), and convert that binary to decimal.

![Illustration of creating a UDG character](/images/posts/udg-lime.png "Illustration of creating a UDG character")

Place those decimal numbers into a byte array and then reference that array from memory address 23675.

```basic
Dim udg (7) as uByte => {0, 60, 102, 255, 255, 126, 60, 0}
Poke uInteger 23675, @udg (0)
```

Instead of placing the "@" character, I can now instead place the CHR(144) which is the UDC characeter I just created:

```basic
PRINT AT INT(RND*20)+1,INT(RND*31);CHR$(144)
```

If I compile the prototype with these additions, I'll now see that the limes are rendered with the UDG character, I just created.

![Screenshot of the prototype using UDG characters for graphics](/images/posts/prototype-4.png "Screenshot of the prototype using UDG characters for graphics")

However, the player don't collect any points when passing the limes on the screen. This is because the current check is based on whether the "@" character is on screen at that location:

```basic
If SCREEN$(y,x) = "@" Then
    updateScore(1)
    generateLimes(1)
End If
```
I can however not make a check for ```CHR$(144)``` on ```SCREEN$``` position, instead I'll need to have a model of where the limes are located, which I've done using an array the size of the play area on the screen (30 x 23).

```basic
Dim limes(23,30) as uByte
```

When I generate a new lime and place it on screen, I also mark it in the array.

```basic
Sub generateLimes(n as uByte)
    Ink 6
    For l=1 To n
        Let limeY = INT(RND*20)+1
        Let limeX = INT(RND*31)
        limes(limeY, limeX) = 1
        Print At limeY,limeX;CHR$(144)
    Next l
End Sub
```

With this in place I can check if there is a lime where the player is currently standing and update everything accordingly.

```basic
If limes(y,x) = 1 Then
    limes(y,x) = 0
    updateScore(1)
    generateLimes(1)
End If
```
Great


[1]: https://www.mojontwins.com/juegos_mojonos/fourspriter-1-0/
[2]: https://bitbucket.org/zxbasic/mojon-twins/src/master/lib/fourspriter/
[3]: https://www.mojontwins.com/juegos_mojonos/fourspriter-1-0/tutorial-de-zx-basic-fourspriter/

[4]: http://oldmachinery.blogspot.com/2014/04/zx-sprites.html