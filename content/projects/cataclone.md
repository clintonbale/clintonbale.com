+++
date = "2012-04-08"
description = "Desc"
meta_img = "images/cataclone/cata_menu.png"
tags = ["projects", "catacomb", "game", "c"]
title = "cataclone"
+++

[cataclone](https://github.com/clintonbale/cataclone "cataclone") is a project I started in 2012 that was intended to help me learn and understand some of the more advanced concepts of the C programming language, reverse engineering and [SDL](https://www.libsdl.org/ "SDL"). The goal of the project was to replicate the original [Catacomb](http://en.wikipedia.org/wiki/Catacomb_%28video_game%29 "Catacomb") game using original Catacomb assets and create a cross platform version built in pure C using SDL+OpenGL. 

{{< figure src="/images/cataclone/cata_menu.png" class="small" caption="The original Catacomb menu screen rendered in the cataclone engine." >}}

Development of the 2D engine and the code to load all of the Catacomb assets was a great challenge. The engine was built in pure OpenGL. SDL was only used to create and maintain a cross-platform window and provide sound API. I decided to write the engine in OpenGL because I thought It would be a good learning experience to see how things operate on a more deeper level.

### Engine

The cataclone engine is the first engine I've written in pure C. I tried to follow object orientated practices as best I could with the C language, taking examples from John Carmack's Quake engine and other public domain old DOS game code. The engine is a simple 2D tile-based engine that renders all tiles sequentially from the top left to bottom right based on the camera's position. 

{{< figure src="/images/cataclone/cata_cgavsega.png" caption="EGA render-mode on the left, CGA on the right." >}}

### Graphics

The graphics of the engine are the same as the original. The community over at [shikadi.net](http://www.shikadi.net/moddingwiki/Catacomb) was kind enough to provide some file offsets from the original catacomb executable where the graphics data was stored. Once the data was extracted from the uncompressed executable I was able to write a generic loader function with some collaborative work with a friend to load the EGA, CGA and PIC file information into a graphics format that SDL could recognize. Some of the [ground](http://www.shikadi.net/moddingwiki/Catacomb_Tileset_Format) [work](http://www.shikadi.net/moddingwiki/PIC_Format) for these file formats was discovered before I wrote the loader, but no complete solution was created. All of the loading and storing of the graphics files is done in memory just like the original game. The only files that are available on the disk are the level files, sound files and the high-score files.

### Levels

Catacomb levels are initially compressed using the [Keen RLE](http://www.shikadi.net/moddingwiki/Keen_1-3_RLE_compression) compression format. However once uncompressed the file format is just a simple sequential list of tiles in a 64x64 array. Each tile representing 1 byte of data, and each byte value means something different in the game. Levels are loaded from file and stored in memory when the game requests the level. 

{{< figure src="/images/cataclone/cata_lvl8.png" class="small" caption="A screenshot of level 8 rendered in the cataclone engine." >}}

### Sounds

Getting sounds to load and play successfully using the SDL audio mixer was by far the hardest part of the project. Sounds in Catacomb use the [Inverse Frequency Sound Format](http://www.shikadi.net/moddingwiki/Inverse_Frequency_Sound_format), converting this format from raw data into playable sounds was an extensive process. *Anders Gavare* originally decompiled the file format, using his [code to convert SND files to RAW](http://web.archive.org/web/20000818033701/http://www.dd.chalmers.se/~f98anga/projects/keen/sounds2raw.c) I was able to convert the files to playable audio in SDL on Windows.

### What's Left?

Basically converting this engine into a game. The monsters' AI and movement are still a mystery to me. There are two options I can take:

1. Decompile the original executable for Catacomb, figure out how the AI and monster pathing works based on this
2. Come up with my own design for AI and monster pathing

### Summary

My original goals with this project were to get more familiar with C and to provide reference code to future programmers interested in legacy file formats. I believe I accomplished both of these goals with the project, my C skills have improved dramatically and the code I wrote for the legacy file formats works well.