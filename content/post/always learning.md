+++
Categories = []
Description = ""
Keywords = []
Tags = []
date = "2009-09-18T14:40:00-07:00"
title = "Always learning"

+++

I am reminded recently in my work with the [DungeonHack](http://dungeonhack.sourceforge.net) project that I am always learning. Just when I
started to think that I was maybe more skilled in c++ than I would rate myself, I run into some major blocks. Basically having fun with
pointers. So, for instance, I want the player to cast a ray out from his head that checks for collisions with other game objects. When
it hits an object, it sets the player target to the object, and fires a callback event on the object.

The problem is: I have Class Player, it has a pointer to class GameEntity (target). If the object that the pointer points to gets deleted, there
is no way to inform the Player to clear out the pointer reference. At least, not automatically.

So what I think I need to do, and will be working on, is have the GameEntity class track each item that references it, and when it gets
deleted, notify the items that have a reference. Hopefully this won't add too much overhead, but I think it may be the only way to really
get this done.

According to wikipedia, this is a "[Dangling Pointer](http://en.wikipedia.org/wiki/Dangling_pointer)" issue that I have run into, and
as I suspected: "A popular technique to avoid dangling pointers is to use [smart pointers](http://en.wikipedia.org/wiki/Smart_pointer "Smart pointer").
A smart pointer typically uses [reference counting](http://en.wikipedia.org/wiki/Reference_counting "Reference counting") to
reclaim objects."

Interestingly, working with the DungeonHack source has greatly improved my understanding of things. I have owned the Torque game engine for
many years now, yet I have never been able to wrap my head around the engine. Now that I am working with a much less complete code base
I really am starting to follow it. When I run into problems like this, I look inside a working engine and see how they did it. Now I can
see that they have created a smart pointer for their game entities, it tracks a list of references, and notifies them upon deletion. It
just feels good to actually understand what I am looking at , and to look at a huge game engine and think "that's really not that complex
after all, it's just a well done compilation of many simple ideas". Anyway, just thought I'd share that it is neat to see the nuts
and bolts where I could only see the whole engine before, it's like when Neo can **see** the code of the matrix.
