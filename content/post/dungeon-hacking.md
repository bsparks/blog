+++
date = "2009-07-15T09:01:44-07:00"

title = "Dungeon Hacking"
tags = ["c++", "programming", "3d", "ogre3d", "open source"]
categories = ["Game Development"]
+++

I've joined the open source project <a title="DungeonHack" href="http://dungeonhack.sourceforge.net" target="_blank">DungeonHack</a>, a FOSS game "Forged in the Spirit of Daggerfall". Apparently at one time they planned to remake Daggerfall but are now just trying to make a modern 3D rpg game that has some of the best qualities of Daggerfall.

It took me a bit to get it all compiled, but I was able to get it up and running. They are using Python for the scripting system. I've never used Python before, though I've heard of it and seen many projects choose to use it. I was really hoping to use Google's V8 engine for a scripting engine, but I suppose it wouldn't hurt to learn another language. The good thing is that since it's all open source I can mess around with it any way I like. I may even work on integrating the V8 engine in there at some point. (either on my own or convince them to use it).

My only concern with Python really is that I think that the end user has to install Python for the game to work. I suppose that isn't really different from having to have .NET framework installed or Java or any of those, but I think that there is something that most professional games do not do. Most of the commercial titles that I play don't need to install these, they usually have all their dependencies built in. Or maybe I just never noticed because I already had them installed.

There are several other open source libraries that the system uses: Ogre3D for graphics, Bullet for physics, OpenAL for sound, myGUI for a UI, and PagedGeometry.
