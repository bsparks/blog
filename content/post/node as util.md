+++
Categories = ["Programming"]
Description = ""
Keywords = []
Tags = ["javascript", "node.js", "tutorials"]
date = "2012-07-18T16:27:03-07:00"
title = "Doing stuff with node.js other than running a server"

+++

Just about everybody who's doing something with [node.js](http://nodejs.org/) is creating a server. In fact, node.js is often referred to
as a "server-side" javascript.  In fact here's a great getting started tutorial on
[net.tuts+](http://net.tutsplus.com/tutorials/javascript-ajax/learning-serverside-javascript-with-node-js/). That tutorial covers
a couple of things beyond the "hello world", creating a simple HTTP server, and creating a live Twitter stream.
Anyway, I think that shows my point that most tutorials and things that people are going to do about node.js
involve either running a server or hooking up to some internet API. While I realize that today's world is online,
and I certainly don't think that it isn't short of awesome to do these things, I think that doing some every day
offline tasks using javascript is *also* awesome. So this will be the first article in possibly a
series of "Doing stuff with node.js other than running a server".

I'm going to start by going over some real world problems that I have actually run into at work, and how to solve them using node.js.

### The Problem
The design team has created several icons of various sizes for use as teacher avatars throughout the site. Upon opening the teacher folder, I see there are several sub folders and it is clear that the icons are to be organized by the dimensions of the image. There are 29x29, 40x40, 85x85, and 112x112 size folders. Great, ok so I immediately think when I'm using them in the website, we can just store the name of the image, and then change folders to get the size depending on where we are in the site. I open the 29x29 folder and see images like "teacher_apple.jpg" and "teacher_male.jpg", I open the 40x40 folder and see things like "apple_40x40.jpg" and "male_40x40.jpg". Way to be consistent guys! Ok, so each folder has only about 14 images inside, and I could go through one by one and rename them, but as a programmer that sounds hideously boring and lame! So here we go, even if it takes longer to write a javascript program to do it for me, it'll be more fun! Plus the next time they do it, I'll be ready for them!

### The Solution
Ok, so the first thing that you're gonna want to do is have the node [docs](http://nodejs.org/api/) open, I can't remember all the commands,
if you can, good for you ;) We want to make modifications to the file system, so we'll be using commands from that library.
Also we'll include the path library to make the file paths easier to work with. So first thing let's just read one of the
folders contents and make sure we have that down. I'm creating a file called cleanup.js in the same folder as I have
the "teacher" images folder from the designers.

```javascript
var fs = require("fs"),
    p = require("path");

var path = "teacher/40x40";

fs.readdir(p.join(process.cwd(), path), function(err, files) {
    if(!err) {
        for(var i=0, len = files.length;i<len;i++) {
            console.log(files[i]);
        }
    }
});
```

For the most part I think the docs should explain what all that does, so next let's rename the files instead of just printing
them out. To see this in action simply type: **node cleanup.js**

```javascript
var fs = require("fs"),
    p = require("path");

var path = "teacher/40x40",
    folder = p.join(process.cwd(), path);

fs.readdir(folder, function(err, files) {
    if(!err) {
        for(var i=0, len = files.length;i<len;i++) {
            var original = files[i],
                modified = original.replace("_40x40", ""),
                callback = (function(o, m) { return function(err) {
                    if(!err) {
                        console.log(o + " --> " + m);
                    } else {
                        console.log("error", err);
                    }
                }; }(original, modified));

            fs.rename(p.join(folder, original), p.join(folder, modified), callback);
        }
    }
});
```

Ok, great so that works. I've already realized that hard coding the path in there isn't going to be useful for long.
At the very least, each folder I want to run this on I have to change the path, and change the string that I want to
replace. Instead it'd be cool if I could take in parameters from the command line. While you can do this with just
the built in libraries, I happen to know and love another helper library for this
called [Commander](https://github.com/visionmedia/commander.js/). If you don't already know how to use NPM to install
packages, you'll want to read up on that, but for now, just type: ***npm install commander*** in the same folder as the
script. Commander lets you specify option flags as well as commands and comes with built in help. So now we'll modify the program to
be a little more useful.

```javascript
var fs = require("fs"),
    p = require("path"),
    app = require("commander");

function renameX(folder, find, replaceWith) {
    var flags, pattern, regex;

    // change to the defined cwd
    process.chdir(app.cwd);

    // default to empty string for optional replacement
    if(!replaceWith) {
        replaceWith = "";
    }

    // regex from string credit to "Anonymous" http://stackoverflow.com/a/874742
    if(find[0] === '/') {
        flags = find.replace(/.*\/([gimy]*)$/, '$1');
        pattern = find.replace(new RegExp('^/(.*?)/'+flags+'$'), '$1');
        regex = new RegExp(pattern, flags);
    } else {
        regex = find; // just simple string
    }

    fs.readdir(folder, function(err, files) {
        if(!err) {
            for(var i=0, len = files.length;i<len;i++) {
                var original = files[i],
                    modified = original.replace(regex, replaceWith),
                    callback = (function(o, m) { return function(err) {
                        if(!err) {
                            console.log(o + " --> " + m);
                        } else {
                            console.log("error", err);
                        }
                    }; }(original, modified));

                fs.rename(p.join(folder, original), p.join(folder, modified), callback);
            }
        }
    });
}

// optionally allow us to run from another starting point
app.option("-w, --cwd [dir]", "set the working path", process.cwd());

// setup the main renaming command
app
    .command("rx <folder> <find> [replaceWith]")
    .description("rename all files in a folder using a regex replace")
    .action(renameX);

// parse the arguments and follow through
app.parse(process.argv);
```

I wrapped the original logic inside a function, the params of the function determine what to do. The arguments passed into Commander's syntax are what gets passed into the function. The &lt;&gt; means required, and the [] is optional. I also created one option so that it can be executed in the context of another directory. That too is optional, and the final argument in that is the default (process.cwd()). So now we can type: <strong>node cleanup.js rx "./teacher/40x40" "_40x40"</strong> ; I've left off the final argument <strong>replaceWith</strong> because it's optional, and I've defaulted it to "" in the function. This should give you output something like this, depending on your filenames.

```

C:\Users\Ben\Documents\node\useful things>node cleanup.js rx "./teacher/40x40" "_40x40"

apple_40x40.jpg --> apple.jpg
arrow_40x40.jpg --> arrow.jpg
greenstar_40x40.jpg --> greenstar.jpg
fire_40x40.jpg --> fire.jpg
earth_40x40.jpg --> earth.jpg
mountain_40x40.jpg --> mountain.jpg
brain_40x40.jpg --> brain.jpg
stones_40x40.jpg --> stones.jpg
teacher_female_icon_40x40.png --> teacher_female_icon.png
polarbear_40x40.jpg --> polarbear.jpg
moon_40x40.jpg --> moon.jpg
water_40x40.jpg --> water.jpg
teacher_male_icon_40x40.png --> teacher_male_icon.png
umbrella_40x40.jpg --> umbrella.jpg
```

So there we go, mission accomplished, it didn't take too much longer than manually renaming them. But just think, if there had been
 100 images in each folder, how much of a pain in the ass would that have been? Now we can just run the command on each folder,
  and bulk cleanup the files.
As I'm finishing this up I can think of a few things that still need hammering out, but I'll save them for later.

* fs.readdir returns an array of files, technically even folders are also files, so they'd be affected by the rename as well
* we should create a package.json file so that npm can install our dependencies for us
* consider installing it globally using the "bin" option so that we can just call "cleanup" from anywhere without having to
call node or have the js file in the path

There could be other things I haven't thought of, bugs, tests, who knows. It is only a quick utility script after all.
If anyone has any suggestions leave a comment!
