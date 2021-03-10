---
title: "Making a ZX Spectrum Game - Part 1 - My Development Setup"
date: 2020-11-10T14:59:23+01:00
draft: false
categories:
- Personal
tags: 
- 8-bit
- speccy
- tutorial
- zx-spectrum
---

With side-projects like this one, I tend to leave the project unchanged for weeks. Therefore, I have found it good practice to write down the explicit details about my development setup; it makes it a lot easier to return to a project set on pause for a while.

As a general rule-of-thumb, my development setup has to be as platform-agnostic as possible. I think this is good practice. It allows a broader audience to follow-along when doing tutorials like this one, and it makes me independent from whether my next primary computer would be a Mac or a Windows PC. As a side note, I used to change jobs every 2-3 years, where my next computer was decided for me by the company “default” and not by my preferences.

And Yes, I will be coding primarily (if not only) on my main development machine, which is a MacBook Pro (2019). I have tried writing some BASIC code natively on the ZX Spectrum’s “chiclet”-keyboard but used the majority of the time hunting for the BASIC keywords, so that is not going to work.

## Cross-Compiler

Here is what I imagine it would be like, creating a game for the Speccy in 1982. The game developer, or “bedroom coder,” would make an initial prototype directly in Sinclair BASIC to verify certain aspects of the gameplay and then re-implement the game in assembler afterward.

My idea is to do something similar. I will create the initial prototype in Sinclair BASIC, and I will then port this version to be cross-compiled using [ZX BASIC][1]; more on this in a later blog post. I like the concept of testing something out natively in Sinclair BASIC (although being tremendously slow) and then easily port it to an advanced version of BASIC and compile it to assembler, rather than to rewrite the entire thing directly in assembler.

I also considered writing the game using the z88dk cross-compiler that allows development for the ZX Spectrum in C but eventually decided not to. Both the ZX BASIC – and [z88dk][2] compiler allow writing parts of the code in assembly language, which is ideal in situations where you want to optimize the code for performance or memory consumption. I will be covering mixing assembler – with BASIC code in this tutorial as well.

### The Installation

ZX BASIC is written in Python (which I love), and all you need to do is download and uncompress it in any directory and set a path to it. I believe binary executables exist for all platforms, but I run directly on top of the Python-version. Python 3 is a requirement.

I have placed mine in /usr/local/bin/zxbasic and made a path for it in ~/.zshrc. 

```sh
export PATH=/usr/local/bin/zxbasic:$PATH
```

Also, purely for convenience, I have made an executable of zxbc.py, so I do not need to type python3 before calling it.

```sh
chmod +x zxbc.py
```

You can naturally skip this step if you have downloaded the binary executables instead.

## Emulator

To my knowledge, [Fuse][3] is the only ZX Spectrum emulator that runs on both Mac, Linux, and Windows PCs. I have only tried the [macOS port][4] and seems to be the chosen ZX Spectrum emulator for Mac, and I have been told it is pretty accurate to the real hardware. Fuse can emulate a series of different versions of the ZX Spectrum, including but not limited to the 16K, +2, +3, and Timex variants of the Spectrum. I will be emulating the 48k version for this tutorial.

![Screenshot of Fuse machine preferences](/images/posts/fuse-emulator-machines.png "Screenshot of Fuse machine preferences")

With Fuse, it is possible to make a memory dump, which is handy, e.g., to dump the entire screen memory to file. Fuse can also act as the debugger with disassembler and stack trace. I will, however, not cover the debugging abilities of Fuse as part of this tutorial.

I will do the majority of testing using the emulator.

IDE
My IDE of choice is [Visual Studio Code][5]. I have installed the [ZX-BASIC extension][6] to Visual Studio Code, which adds support for all reserved keywords from Sinclair BASIC to ZX BASIC.

I have also added the path to Visual Studio Code into ~/.zshrc.

```sh
export PATH="/Applications/Visual Studio Code.app/Contents/Resources/app/bin":$PATH
```

It allows me to navigate to and open any development project with Visual Studio Code from the terminal.

```sh
code .
```

## Various Tools
In addition to the compiler, IDE, and emulator, I might also need a simple image editor to help make the graphics and some tools to convert those to BASIC code.

A compression tool might also come in handy. ZX-Basic natively supports MegaLZ, so I might need some tooling to compress using the MegaLZ compressor.

I will explore these tools individually in upcoming blog posts.

In Summary
These are the different tools I will be using to make a game for the ZX Spectrum; it might be different for you. As long as you use the ZX BASIC cross-compiler, you should be able to follow along with this tutorial.

Here is the complete list of downloads:

- [Boriel ZX Basic][1]
- [z88dk – the development kit for Z80 computers][2]
- [Fuse – the Free Unix Spectrum Emulator][3]
- [Fuse for macOS][4]
- [Visual Studio Code][5]
- [ZX-BASIC extension for Visual Studio Code][6]

[1]: https://github.com/boriel/zxbasic
[2]: https://github.com/z88dk/z88dk
[3]: http://fuse-emulator.sourceforge.net/
[4]: https://fuse-for-macosx.sourceforge.io/
[5]: https://code.visualstudio.com/
[6]: https://marketplace.visualstudio.com/items?itemName=jsjlogin.zxbasic