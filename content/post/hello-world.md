+++
date = "2009-01-05T08:01:44-07:00"

title = "Hello World"
tags = ["javascript"]
categories = ["General"]
+++

Well, finally setup a blog. Wordpress seems to be pretty much the standard. Looks nice.

I'm not sure on the theme though.

So, in this blog I plan to note interesting things (to me).  Recently I've been very heavy into Javascript research, mostly because of YUI and Chrome. I got sick and tired of my work website not working in anything but IE. Even though I'm behind the times, I figure it's never too late to catch up. So I've been reading up a lot on Javascript techniques, YUI, Google Gears, and other nifty stuff.

I'm planning to write a web rpg at some point, problem is I can't decide on the server technology to use. (Already pretty set on YUI for my main js lib). It's either going to be classic ASP (I have tons of exp in this), Asp.Net (have medium exp in this) or PHP (for some reason I fight this one).

Anyway, I was thinking of doing a Roguelike, only a PBBG and maybe even MMO-ish. I found a javascript tile map engine that uses JQuery called gTile. I took a look at it, and while it does seem to run OK, it looks to me that it is far from optimised. For instance each tile loads an image, even if it's the same image that is previously loaded. So I was thinking of making a tile engine using CSS Sprites. What I don't know though is whether or not it'll be better to do a map that only renders the visible tiles, or render the whole thing and scroll the div. I'm guessing the lower rendering amount, and just change the sprite background position on the fly. Then the actual map data would be kept track of by the js engine. I'll have to try it out I guess, just get a simple panel with map cells that I can "pretend" to scroll. If that works, then simply have 2 or 3 layers of the same thing on top for some depth sorting. Makes me wonder, if *that* was fast, then could one in theory come up with a 3D engine? I'm sure someone has done 3D in js before...

Hmm.... this is my first real blog, I bet I'll have to go back and setup links for all those words up there :) If anyone manages to read this thing, HI! :)
