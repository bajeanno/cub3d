# Cub3D
A portal game made **entirely in C**, using **MLX library (minilibX)**, rendering on **CPU only** with **multithreading enabled**.

## The basics
This game is made with the same render technique as **wolfenstein3D** and the first doom, also known as "ray-casting", this technique is about knowing the distance between the player and every wall of the map in their field of view to represent on the screen a fake 3D projection.
All is about the height of each wall, as more the wall is close, more it will seem high for the player and vice versa, so for each pixel in x coordinates, the game will shoot a ray from the player in the direction told by the x coordinate (little trigonometry notions are needed here) until it touches a wall, there the game gets the distance to this wall and computes the height of the wall on the projection.
Now that the game knows the height it needs to display for each x coordinate, it's about to render the right pattern of the right texture for this coordinate at the right height for this pixel column.

## The Portal part / ray shooting through portals
The Portal part has been about teleporting rays from a wall marked as "portal" to the other wall marked as "portal" by the player during the game (right and left click as the real portal game).
A difficulty we encountered was how to display multiple portals when two portals face each other. 
Well, for each x coordinate, and subsequently each ray, we needed to record the distance between the ray and the player every time it crosses a portal. We managed to limit the number of portal crossed to 7 as the filter implemented on the portal texture make difficult to see beyond. However, each time a ray crosses a portal we add the distance it traveled to the global distance, then reset it, and teleport the ray.
Now that the game recorded all portals crossed we can process to render each portal as if they were walls starting from the last crossed, so the closest portal appears in front of the farthest.
For the player, the game will use the same method as a ray, except some verifications of moving versus viewing angles, as a ray has an angle property that is its moving angle, the player instead has a viewing angle, so from the viewing angle and the keys that are pressed, we're able to determine the real moving angle to know if it'should cross the portal or not.
For example, if you're facing back the portal and moving backward, and the game only uses the player's viewing angle, then you will not be able to cross the portal, even if you should by the way. by creating the "moving angle" the game is able to accurately know if you should pass through the portal or not.

## The library we used
The **MLX (minilibX)** is a graphical interfacing library that permit us to communicate with the window server (working on **macOS** and **linux**). This the only part we did not write ourselves, for macOS needs, one part is written in swift and objective-C, but the whole project other than window interfacing is done in C.
This library helped us read xmp files, translating them to double array of colors format making that really easier for us to put the texture you want on the walls, and that's all for the input.
it also helped us get the keyboard and mouse inputs.
The output part can be resumed as creating a window with dimensions **<width, height>** and putting a pixel of color **<color>** at coordinates **<x, y>** on the window, and the game is the one that has to **where** to put **what** color.

## We made a game engine
Well after all a game engine is just a program that takes input and shows anything on the screen related to those inputs. You can import maps (with a strongly closed proprietary file format, right ?), launch the "engine", and walk and teleport in the environnement you just created. Isn't it the definition of a game engine ?

PS : could you find the Easter eggs ?

###### Made by Basile Jeannot and Nino Faust for the common core of the 42 school
