+++
Categories = ["Programming", "Game Projects"]
Description = ""
Keywords = []
Tags = []
date = "2010-03-01T11:40:00-07:00"
title = "New project"

+++

So I've decided that my new focus will be on creating (and finishing!) an [Xbox Live Indie Game](http://www.xbox.com/en-US/games/community/default.htm).
Since the launch of the XNA Game Studio, Microsoft has made vast improvements. When I first heard about it long ago, I didn't really think much of it.
From what I had heard you were very limited on what was actually possible to create. Now however, you can create almost full arcade games.
You can use networking, 2d or 3d, and even the Xbox Live Avatars. You can create games for Windows, Zune, and Xbox. In order to deploy your
project onto the Xbox360 you have to join the [Creators Club](http://www.xbox.com/en-US/games/community/default.htm).
This comes with a $99/yr price tag, but since you can actually publish and sell your games, it's worth it.
So without too much hesitation, I joined.

So now the task of deciding what I should create first. I want to create something simple; as simple of a game as I can bare to do.
It's always difficult to keep things from becoming epic grand scale games. The battle against feature creep is a great one :).
The Xbox Live Indie games scene is sort of known already for "games" that are just fireplace/screensavers or massaging programs
(they use the rumble feature of the controller) so I definitely want to go beyond that level. Looking at the list of games, there are a lot
on there that are pretty bad. Not just in graphics, but gameplay as well (of course gameplay matters most). There are quite a
few "diamonds in the rough" though, and that is what I would hope to create. I took the time to play some of
the [highly rated one](http://www.xnplay.co.uk/2010/01/23/the-best-xbox-indie-games-releases-2009/)s, such as
[Miner Dig Deep](http://marketplace.xbox.com/games/media/66acd000-77fe-1000-9115-d80258550182/), 
[I MAED A GAM3 W1TH Z0MB1ES!!!1](http://marketplace.xbox.com/en-GB/games/media/66acd000-77fe-1000-9115-d802585502a6/),
and [Solar](http://marketplace.xbox.com/games/media/66acd000-77fe-1000-9115-d802585501c9/).
I was pretty impressed with them, they are all simple, but end up being very fun with their unique gameplay. I ended up purchasing
Miner Dig Deep and Solar, as they are priced at 80 pts (about 1 dollar). With the audience available to the Xbox (about 10 million Live customers)
even having a game that sells for 1 dollar has the potential to make quite a bit of money.

One genre that I noticed was lacking in title variety is Strategy. There are plenty of Tower Defense variant games, but that's about it for
Strategy. So I think that I will make a simple turn based strategy game. Something along the lines of Warlords or Heroes of Might and Magic,
but to keep it even more simple, I'm going to try to just create the battle system. The battle system in those types of games is the most
important part, and possibly the most complex, so only after that is done should I consider expanding to the army building world map type
portion. Also, so that I don't get too hung up on graphics (like I always do) I am going to use the
[free tiles](http://molotov.nu/?page=graphics) from [roguelike](http://en.wikipedia.org/wiki/Roguelike) games.
![](/images/dg_people32.gif)
With the use of the various tutorials and examples found online, I was able to get started fairly quickly.
Here is a screenshot from a randomly generated "forest" map that I have working.

![](/images/tacticsRL_forest1-300x233.jpg)

This idea came from a [cave generation algorithm](http://pixelenvy.ca/wa/ca_cave.html).
It uses [cellular automata](http://en.wikipedia.org/wiki/Cellular_automaton) to "carve" out the caves.
Here I have used trees for the walls instead of rocks. First we randomly populate the grid with walls/trees. Then we make a few passes checking
each cell and it's neighbors and changing them to either a wall or a floor. It could still use some further refining, something to help
remove the isolated areas. I also plan to cut the map in half and do a mirror on it, since battle maps are typically mirrored for "fairness".

Here is the function that takes care of a single pass. I found that about 4 passes generates decent looking results.

```csharp
public void GenerateCaves(int wallTile, int floorTile, int fillProbability, int r1, int r2)
{
    for (int y = 1; y < height - 1; y++)
    {
        for (int x = 1; x < width - 1; x++)
        {
            int adjCountR1 = 0;
            int adjCountR2 = 0;

            for (int i = -1; i <= 1; i++)
            {
                for (int j = -1; j <= 1; j++)
                {
                    if (grid[x + j][y + i] != floorTile)
                        adjCountR1++;
                }
            }

            for (int i = y - 2; i <= y + 2; i++)
            {
                for (int j = x - 2; j < x + 2; j++)
                {
                    if (Math.Abs(i - y) == 2 && Math.Abs(j - x) == 2)
                        continue;

                    if (i < 0 || j < 0 || i >= width || j >= height)
                        continue;

                    if (grid[i][j] != floorTile)
                        adjCountR2++;
                }
            }

            if (adjCountR1 >= r1 || adjCountR2 <= r2)
            {
                grid[x][y] = wallTile;
            }
            else
            {
                grid[x][y] = floorTile;
            }
        }
    }
}
```

I've also been working on dungeon generation using Binary Trees and QuadTrees. Hopefully not going too far off topic, I plan to use these
to generate buildings and other important map areas (ruins). I'll go more into that later...
