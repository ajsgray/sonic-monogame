# sonic-monogame

#### Introduction
No idea what this project will become yet, but after poring over [Sonic Retro's Sonic Physics Guide](http://info.sonicretro.org/Sonic_Physics_Guide) for the last week or so I've decided to try my hand at recreating/emulating some of the rules they describe in their guide. Even so, reading about the techniques Sonic Team used to create the original games, given the old console's constraints, is fascinating.

I'm no game developer but have made lots of small demos and am most comfortable using Monogame, I use c# in my day job so that helps too.

For the first demo I'd like to implement the level renderer first, not sure if I will make it exactly as the original Sonic games worked, but close enough to make some small and testable demo levels.

#### Levels
From what I understand, the original games broke their levels down into a grid of chunks, 256x256 pixels in Sonic 1 and Sonic CD, and 128x128 pixels in Sonic 2, 3, & K. Each chunk was then split into 16x16 pixel 'tiles' which is how the collision detection magic happens. And then each of these tiles was split further into 8x8 pixel (blocks?).

So graphically speaking, the smallest element is an 8x8 pixel block, which is how the tiles are composed, then each chunk is composed of 16x16 tiles. Makes a lot of sense considering the storage constraints of the Mega Drive!

The tiles store metadata with them:
- Tile Id (figuring out where in the atlas it is)
- Pointer to a height map (vertical and horizontal it seems, also seems like height maps get reused across similar tiles)
- Whether the tile is flipped horizontally
- Whether the tile is flipped vertically
- Whether the tile is solid or not

I am assuming the tile solidity helps with creating secret areas, helps with creating parts of the loop, and potentially some foreground and background.

I don't feel like there is much to be learned from finding all the original artwork and trying to stitch together all these pieces to make a final level, so will create some basic tiles and make some chunks from them.

References:
- [Solid Tiles] http://info.sonicretro.org/SPG:Solid_Tiles
- [Slope Physics] http://info.sonicretro.org/SPG:Slope_Physics

#### Camera
All of the units used in the old games are pixels which isn't ideal today, however I think a good start would be to build the initial level engine around these values, and afterwards it can be converted to some sort of ratio of the screen so any resolution can be handled.

The screen size is 320x244 pixels
The left border is 144
The right border is 160

When Sonic is in the air the camera has a top border of 64 and a bottom border of 128. When he exceeds the border the camera will try and catch up at a maximum speed of 16 pixels per frame.

Different rules for knuckles but just focusing on Sonic at the moment. There are also other little rules here and there depending on Sonic's position, but this should be enough to start with.

---

Last updated 12th Feb 2021
