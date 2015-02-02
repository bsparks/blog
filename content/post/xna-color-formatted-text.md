+++
date = "2010-05-16T09:17:44-07:00"

title = "XNA Color Formatted Text"
tags = ["c#", "programming", "dotNet", "xna", "game development"]
categories = ["Game Development"]
+++

I recently have been working on a simple "old school" rpg game for XNA. One of the features that I want to have is scrolling text for combat and other messages that are given to the player. What I am talking about might look like this:
<blockquote>
<h3>You swing at the <strong><span style="color: #ff0000;">goblin</span></strong><span style="color: #ff0000;"> </span>and hit for <strong><span style="color: #00ccff;">8</span></strong> damage.</h3>
</blockquote>
Well, there isn't any built in method to draw text to the screen and have the colors change in one string. Pretty much the only way (or at least the easiest way) to accomplish this is to use multiple DrawString calls on the SpriteBatch. When thinking about that I didn't think it would be a terribly difficult task, but I decided to do a google search for any already made solutions. I found an article on the <a href="http://www.xnawiki.com">XNAWiki</a> called <a href="http://www.xnawiki.com/index.php?title=Inline_Color_Code_Processing">Inline Color Code Processing</a> that claims to (I haven't tried it) do the trick. When I examined it though, I found that I couldn't be happy with their implementation. They chose to define a set number of colors and then reference them in the string using an integer index. That to me seemed very limited, and even though my project is simple, I'd rather not hard code in a bunch of colors. What I wanted is to be able to type in a color in the string and have it parse out to the color I wanted, much like HTML or BBCODE would work. The best way I could think of to do this is using HEX values, just like HTML or CSS would work. Unfortunately, getting a color from HEX is another thing that isn't built into XNA. So, back to google and I found <a title="The Dead Pixel Society" href="http://www.thedeadpixelsociety.com/2010/04/hex-colors/" target="_blank">this blog</a> that had the extensions I needed already worked out! Ok, fantastic, now the final piece, I just needed to write up a function to display the text. It actually ended up being pretty easy and perhaps it's not the best implementation, but it works perfectly. To use it, you setup your string like so: "You attack the [color:#FFFF0000]goblin[/color] and hit for [color:#FF00FF00]8[/color] damage." Of course, just as the extension is converting the HEX to actual colors, you can convert the colors to HEX when you are making the string and use some of the built in colors like Color.Red.ToHex(true).

```csharp
public static void DrawColorFormattedText(SpriteBatch spriteBatch, SpriteFont font, Vector2 position, string text)
{
    Color defaultColor = Color.White;

    // only bother if we have color commands involved
    if (text.Contains("[color:"))
    {
        // how far in x to offset from position
        int currentOffset = 0;

        // example:
        // string.Format("You attempt to hit the [color:#FFFF0000]{0}[/color] but [color:{1}]MISS[/color]!",
        // currentMonster.Name, Color.Red.ToHex(true));
        string[] splits = text.Split(new string[] { "[color:" }, StringSplitOptions.RemoveEmptyEntries);
        foreach (var str in splits)
        {
            // if this section starts with a color
            if (str.StartsWith("#"))
            {
                // #AARRGGBB
                // #FFFFFFFFF
                // #123456789
                string color = str.Substring(0, 9);

                // any subsequent msgs after the [/color] tag are defaultColor
                string[] msgs = str.Substring(10).Split(new string[] { "[/color]" }, StringSplitOptions.RemoveEmptyEntries);

                // always draw [0] there should be at least one
                spriteBatch.DrawString(font, msgs[0], position + new Vector2(currentOffset, 0), color.ToColor());
                currentOffset += (int)font.MeasureString(msgs[0]).X;

                // there should only ever be one other string or none
                if(msgs.Length == 2)
                {
                    spriteBatch.DrawString(font, msgs[1], position + new Vector2(currentOffset, 0), defaultColor);
                    currentOffset += (int)font.MeasureString(msgs[1]).X;
                }
            }
            else
            {
                spriteBatch.DrawString(font, str, position + new Vector2(currentOffset, 0), defaultColor);
                currentOffset += (int)font.MeasureString(str).X;
            }
        }
    }
    else
    {
        // just draw the string as ordered
        spriteBatch.DrawString(font, text, position, defaultColor);
    }
}
```

And here it is, pics or it didn't happen :)

[caption id="attachment_147" align="alignleft" width="384" caption="Colored Rpg Text"]<a href="http://superiorcode.com/blog/wp-content/uploads/2010/05/coloredTextEx.jpg"><img class="size-full wp-image-147" title="coloredTextEx" src="http://superiorcode.com/blog/wp-content/uploads/2010/05/coloredTextEx.jpg" alt="" width="384" height="191" /></a>[/caption]
