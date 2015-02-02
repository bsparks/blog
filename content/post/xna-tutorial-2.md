+++
Categories = ["Programming"]
Description = ""
Keywords = []
Tags = ["game development", "xna", "tutorials"]
date = "2011-11-10T08:51:23-07:00"
title = "XNA Tutorial 2"

+++
Welcome to the second tutorial in a series on XNA Game Development. In the <a title="XNA Tutorial 1" href="http://superiorcode.com/blog/?p=216">first tutorial</a> we learned how to get all of the tools needed and create the basic solution, as well as a little about game loops. Now we're going to do something much more interesting than just displaying a blue screen. We're going to display an image on the screen, and we'll make it move around.

So let's build off of the previous solution, or create a new one if you don't have that.
<h2>Loading an Image</h2>
![](/images/cat.png)

Ok, so first off we need an image. For the purposes of this tutorial we'll just grab any image from a quick search on Google. I have found a lovely image of a cartoon cat. That should do for now. Take your image and drag it over the <strong>Content</strong>project in your solution. It should appear there as part of that project now. In XNA images are called Textures. There are both 2D and 3D textures, this is a 2D texture. In order to use the image in a game we need a reference to a Texture2D object. In the code the Texture2D object keeps track of all of the pixels in an image file. In order to get the data from the file into the object, we need to open and load the file. The Game class that we have derived from comes with a content manager built in. In the LoadContent method is where we will want to get the information from the image file into our texture object.

```csharp
GraphicsDeviceManager graphics;
SpriteBatch spriteBatch;
Texture2D catTex;

public Game1()
{
    graphics = new GraphicsDeviceManager(this);
    Content.RootDirectory = "Content";
}
```

For now we have just created the Texture2D catTex as a private member of our Game class. Next in the LoadContent() method we will load the data.

```csharp
<summary>
/// LoadContent will be called once per game and is the place to load
/// all of your content.
///
</summary>
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);

    // TODO: use this.Content to load your game content here
    catTex = Content.Load("cat");
}
```

Notice that we do not need to provide the file extension. Because there is only one file in the Content folder named "cat" XNA just pick that one. If we had cat.png and cat.jpg both in there, then we would need to specify. We can also create folders inside the content project in order to better separate the various types of content out. For now we can keep things simple, but later we will want to do this. Once a project gets bigger managing your resources will become more important.
<h2>Rendering the Image</h2>
Ok, now that we have loaded the cat image into memory, lets display it on the screen. Everything that  we want to show on the screen goes in the Draw() method.

[code lang="csharp"]
///
<summary> /// This is called when the game should draw itself.
 /// </summary>
        ///Provides a snapshot of timing values.
        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            // TODO: Add your drawing code here
            spriteBatch.Begin();
            spriteBatch.Draw(catTex, Vector2.Zero, Color.White);
            spriteBatch.End();

            base.Draw(gameTime);
        }
[/code]

If you noticed before during the LoadContent() method that we instantiated a new instance of the spriteBatch and wondered what that was, now is the time to find out. The SpriteBatch is a special built in shader for XNA optimized for rendering textures to the screen. In game development when we render textures to the screen (and make them move about) we call them Sprites. A shader is a special program that gets executed on the graphics card telling it how to render. All modern graphics programming is done through the use of shaders. In the code above first we called spriteBatch.Begin(). Rather than drawing each instruction one at a time, you can batch together all of them into one call. We tell the spriteBatch to start paying attention, all of the following commands will be for the graphics card. When we are done, we tell it spriteBatch.End() so that it knows we have finished and can go ahead and send the instructions to the graphics card.

The Draw() method has several overloads, in this one the first thing it expects is a reference to a Texture2D object; i.e. which image to draw. The second parameter is the position on the screen that we want to draw the image. It is expecting a Vector2. A Vector2 represents a 2 dimensional vector, x and y coordinates. Beyond simply storing the x &amp; y, you can perform all sorts of vector mathematics on them. Vectors are extremely important and something that you will need to use a lot. For now just know that I put in 0,0 for the coordinates, which is the upper left corner of the screen. The third and final parameter of this overload is a Color. The color is used to tint the image. Telling the draw method to use White will render the image as it is.  It is like shining a light on the image in the real world, white light would reflect off of the surface back to us and we would see it as it was. If we had a red flashlight, the color would blend in with the colors of the image and only tint it red. Tinting can be a very useful technique, but for now we'll just render it as is.

Now that we have that code, go ahead and launch the program in debug mode and you should see your image rendered onto the screen!

<a href="http://superiorcode.com/blog/wp-content/uploads/2011/11/xna_tut2_cat.jpg"><img class="aligncenter size-medium wp-image-249" title="xna_tut2_cat" src="http://superiorcode.com/blog/wp-content/uploads/2011/11/xna_tut2_cat-300x190.jpg" alt="" width="300" height="190" /></a>
<h2>Moving it around</h2>
Ok, while that is exciting, I think that we'll want to do something a little more interesting. Games do much more than just render images to the screen. Let's make this cat move around! In order to do that we need to update the position that we draw it on the screen. First we'll add a Vector2 to our game class so that we can keep track of the position at a higher scope from the Draw method. We'll have it default to [0,0] still just like our old one, and then down in the Draw call we'll replace Vector2.Zero with our new position value.

[code lang="csharp"]
        GraphicsDeviceManager graphics;
        SpriteBatch spriteBatch;
        Texture2D catTex;
        Vector2 catPos = Vector2.Zero;

        public Game1()
...

spriteBatch.Draw(catTex, catPos, Color.White);
[/code]

The Update() method is where we want to perform any of our calculations that are not directly about drawing things on the screen. You'll notice that there is already a little bit of code in that method, it is testing the gamepad to see if the Back button has been pressed. When the back button is pressed, the game exits. XNA has built in classes that can be used to check various types of input devices, the keyboard, gamepad, mouse, etc. Just like they are checking the GamePadState, there is also the KeyboardState.

[code lang="csharp"]
        /// <summary>
        /// Allows the game to run logic such as updating the world,
        /// checking for collisions, gathering input, and playing audio.
        /// </summary>
        /// <param name="gameTime">Provides a snapshot of timing values.</param>
        protected override void Update(GameTime gameTime)
        {
            // Allows the game to exit
            if (GamePad.GetState(PlayerIndex.One).Buttons.Back == ButtonState.Pressed)
                this.Exit();

            // TODO: Add your update logic here
            KeyboardState kbstate = Keyboard.GetState(PlayerIndex.One);
            GamePadState gbstate = GamePad.GetState(PlayerIndex.One);

            if (kbstate.IsKeyDown(Keys.Left))
            {
                catPos.X -= 1;
            }
            if (kbstate.IsKeyDown(Keys.Right))
            {
                catPos.X += 1;
            }
            if (kbstate.IsKeyDown(Keys.Up))
            {
                catPos.Y -= 1;
            }
            if (kbstate.IsKeyDown(Keys.Down))
            {
                catPos.Y += 1;
            }

            catPos = Vector2.Add(catPos, gbstate.ThumbSticks.Left);

            base.Update(gameTime);
        }
[/code]

Since we want to eventually use this on the Xbox we'll get both the keyboard state and the gamepad state. By default Update() is called 60 times every second. Each time it is called we will check on the status of the input devices. For the keyboard we'll use the arrow keys. Left and right modify the X-Axis and Up and Down modify the Y-Axis. For the gamepad we will use the Left Analog stick. From the tests there you can see that we are modifying our cat's pos (paws?). Since Update() is called before Draw() it will change where the graphics card is told to render the image to the screen. Since every frame we are moving it slightly it will give the illusion of floating across the screen. In reality we're just drawing a still image over and over in a slightly different spot. It's also important to note that in screen coordinates the top of the screen is Y Zero and Y moves positively towards the bottom of the screen. So in order to move "up" we subtract from Y. On the X-Axis the left side of the screen is X Zero and X moves positively to the right. It's perfectly fine and possible to set our image to coordinates that are outside of the screen. If you run the program now you will see that using the arrow keys we can make the cat go off the screen or in any direction that we want.
<h2>Next Time...</h2>
So now we've taken our first steps into something that is much closer to a game than just clearing the screen blue, but from here the sky is the limit! In the next tutorial we'll take a look at rendering multiple sprites to the screen, utilizing tint, drawing text, basic collision detection, and some "AI". For this tutorial you can grab the all the files <a href="http://superiorcode.com/download/xna_tuts/XnaTutorial2.rar">HERE</a>.
