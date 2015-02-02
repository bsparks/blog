+++
Categories = ["Programming"]
Description = ""
Keywords = []
Tags = ["game development", "xna", "tutorials"]
date = "2011-11-07T16:15:23-07:00"
title = "XNA Tutorial 1"

+++

Following along with the theme from Desert Code Camp, I have decided to start writing a series of XNA game development tutorials. This will be the first one, and will start at the very beginning.

## What is XNA?
XNA is a game development framework created by Microsoft that has access to various game APIs through .NET. It can be used to create games for the Xbox 360, Windows Phone 7, and Windows PC. The framework provides classes and utilities to access various areas of games such as 2D and 3D graphics, sound, networking, and live services. In order to develop for the Xbox 360 and / or distribute games created on Microsoft's App Hub, a $99/yr developer fee is required.

## Here's what you need to get started:
In order to develop using XNA you need to use Visual Studio 2010. Currently you can choose between C# and VB. The Express editions (free) will work fine for this. So first thing, <a href="http://www.microsoft.com/visualstudio/en-us/products/2010-editions/express" target="_blank">download</a> and install it. Personally, I prefer C# so (at least initially) all of my tutorials will be in that context. I doubt the translation between is terribly complex. Once that is installed you will then need to install the XNA Game Studio, which at the time of this writing is at version 4.0 Refresh. Here are the links, I've included the runtime distribution as well, for development, you need the first one.

XNA Game Studio 4.0 Refresh

Download page - <a href="http://www.microsoft.com/download/en/details.aspx?displaylang=en&amp;id=27599" target="_blank">http://www.microsoft.com/download/en/details.aspx?displaylang=en&amp;id=27599</a>
Direct download link - <a href="http://download.microsoft.com/download/E/C/6/EC68782D-872A-4D58-A8D3-87881995CDD4/XNAGS40_setup.exe" target="_blank">http://download.microsoft.com/download/E/C/6/EC68782D-872A-4D58-A8D3-87881995CDD4/XNAGS40_setup.exe</a>

XNA Framework Redistributable 4.0 Refresh

Download page - <a href="http://www.microsoft.com/download/en/details.aspx?displaylang=en&amp;id=27598" target="_blank">http://www.microsoft.com/download/en/details.aspx?displaylang=en&amp;id=27598</a>
Direct download link - <a href="http://download.microsoft.com/download/5/3/A/53A804C8-EC78-43CD-A0F0-2FB4D45603D3/xnafx40_redist.msi" target="_blank">http://download.microsoft.com/download/5/3/A/53A804C8-EC78-43CD-A0F0-2FB4D45603D3/xnafx40_redist.msi</a>
<h2>Creating your first project</h2>
Now that you've got the tools installed, it's time to fire up Visual Studio and create a new project.
<p style="text-align: center;"><a href="http://superiorcode.com/blog/wp-content/uploads/2011/11/VSP.jpg"><img class="size-medium wp-image-221 aligncenter" title="VSP" src="http://superiorcode.com/blog/wp-content/uploads/2011/11/VSP-300x207.jpg" alt="" width="300" height="207" /></a></p>
<p style="text-align: left;">You will notice that there are a few different project templates to choose from, for now go ahead and pick Windows Game 4.0. Even if you want to develop for Xbox 360 it's best to start with this type of project. You can develop most of the time in Windows, and then easily switch over to the Xbox when needed. Visual Studio will create for you a somewhat bare bones solution so that you can get started!</p>
<p style="text-align: left;">There are two types of projects that will be created, the first is the main game itself, and the second is a Content project. In the content project is where all of your games assets will go. Anything like images, sounds, 3d models, game levels, etc. are considered content and will go in there. First let's take a look at the code.</p>

<h2 style="text-align: left;">The Code</h2>
<p style="text-align: left;">You'll notice at the top that by default several libraries have been included:</p>

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Audio;
using Microsoft.Xna.Framework.Content;
using Microsoft.Xna.Framework.GamerServices;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;
using Microsoft.Xna.Framework.Media;
```

These are all of the major XNA libraries. Each one covering a major aspect of the game application. For the most part they should be self explainatory, but we'll go into them in more detail later. Next you'll see the basic layout of a game. If you are new to any kind of game development the concept of a game loop might also be new. Normally a program is executed from top to bottom, and then when it goes past the final instructions it exits. For a game, obviously this isn't desired behavior. So at some point we need to make sure that the program will keep running until the player decides to quit. In order to achieve this, you setup an infinite loop. At a basic level, it's something like this:

```
  while(true) {
    // play!
    if(done) {
        break;
    }
  }
```

Behind the scenes it's a little bit more complicated than that, but that is the general idea. In the base framework there is a Game class from which your game, Game1 is being derived. When the application starts an instance of your game class will be created. It runs the Initialize and LoadContent methods and then begins to loop. Every loop two methods are called: Update() and Draw(). Update by default will be called 60 times every second. Draw will be called as many times as possible. In order to ensure that Update maintains a consistent frame rate, sometimes the Draw call will be skipped. In order to have smooth animations and other timings you want to follow the names of the methods, update any of your entities in Update, and draw them in Draw. Here's what they look like:

```csharp
///
<summary> /// Allows the game to run logic such as updating the world,
/// checking for collisions, gathering input, and playing audio.
/// </summary>
///Provides a snapshot of timing values.
protected override void Update(GameTime gameTime)        {
    // Allows the game to exit
    if (GamePad.GetState(PlayerIndex.One).Buttons.Back == ButtonState.Pressed)
        this.Exit();

    // TODO: Add your update logic here
    base.Update(gameTime);
}

///
<summary> /// This is called when the game should draw itself.
/// </summary>
///Provides a snapshot of timing values.
protected override void Draw(GameTime gameTime)        {
    GraphicsDevice.Clear(Color.CornflowerBlue);
    // TODO: Add your drawing code here
    base.Draw(gameTime);
}
```

As you can see, they've placed some TODOs there for you to start coding away. In the Update method they are testing the input from the gamepad for the Back button being pressed, and if it is then exit the application. This is because if you were to send this application over to the Xbox it's much harder to exit an application if there isn't a provided button. You'd have to use the exit to dashboard feature to break out. In windows, you can simply click the standard window close button.

In the Draw application they are telling the graphics card to clear the screen to a lovely CornflowerBlue color. So every time, at least 60 times a second the game is checking to see if any buttons are pressed on the gamepad, and as many times as possible it is clearing the screen to blue. In order for things to render onto the screen they have to be drawn over and over. If you run this application, you should see this:

<a href="http://superiorcode.com/blog/wp-content/uploads/2011/11/first.jpg"><img class="aligncenter size-medium wp-image-232" title="first" src="http://superiorcode.com/blog/wp-content/uploads/2011/11/first-300x190.jpg" alt="" width="300" height="190" /></a>
<h2>Victory!</h2>
Well, there's your first game. It doesn't do anything, and it isn't very exciting (to most), but it is indeed a game nonetheless. From here you can start to add graphics and sounds and game logic until you have the game of your dreams.

That's it for the first tutorial, in the next one we'll cover rendering an image to the screen and making it move around!
