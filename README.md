# 🎮 Cub3D

**Cub3D** is a **portal game** built **entirely in C**, using the **MLX (minilibX)** library. It renders everything on the **CPU only**, and supports **multithreading**.

---

## 🚀 Basic Concept

This project leverages the same rendering technique as **Wolfenstein 3D** and the early **Doom** games: **ray-casting**.  
The idea is to calculate the distance between the player and every wall in their field of view, and then create a fake 3D projection on the screen.

👉 For each vertical pixel column (**x coordinate**), the engine fires a ray from the player's position in the direction dictated by that column’s angle.  
The ray advances until it hits a wall, and the **distance** to that wall determines how **tall** it will appear on the projection.  
Closer walls appear taller, while distant walls seem shorter.

Once the height is calculated, the engine selects the correct texture and renders it at the appropriate spot, creating the illusion of 3D depth.

---

## 🌀 Portal Feature / Ray Teleportation

The **portal** aspect involves teleporting rays when they hit walls marked as portals (right and left click, just like in the original Portal game!).  
A key challenge was rendering **portals that face each other**, which can reflect rays infinitely.

👉 For each **ray** (each screen column), we keep track of the distance traveled every time it crosses a portal.  
We capped the number of portal crossings at **7**, since the portal texture filter makes things too blurry beyond that.  
Every time a ray crosses a portal, the distance it traveled is added to a global distance, then it’s **teleported** to the other portal and continues.

When it comes time to render, portals are drawn in reverse order — starting with the **furthest crossed portal** — so that closer portals are always rendered in front.

🔑 For player movement, we distinguish between the **viewing angle** and the **movement angle** (which depends on the keys pressed).  
This ensures smooth portal traversal, even if the player is moving backwards or sideways relative to their view angle.  
Without this distinction, the player wouldn’t be able to walk through a portal correctly when moving backwards!

---

## 📚 The Library We Used

We used the **minilibX (MLX)** library for graphical interfacing, which works on **macOS** and **Linux**.  
This is the only part of the project we didn’t write ourselves — for macOS, some parts of MLX are written in Swift and Objective-C.

✅ MLX helped us:
- Load `.xpm` files and convert them into easy-to-use texture arrays.  
- Capture **keyboard** and **mouse inputs**.  
- Create a window of dimensions **<width, height>** and set pixel colors at **<x, y>** coordinates.

Apart from that, **the entire project** (rendering, logic, portals) is fully written in C!

---

## 🔧 A Homegrown Game Engine

In the end, we built a **complete game engine**: a program that takes user input and dynamically renders a world to the screen.  
You can load custom maps (in a proprietary format, of course!), launch the engine, walk around, and teleport through portals.  
That’s the very definition of a game engine, isn’t it?

---

👉 **PS:** Can you find the hidden Easter Eggs?

---

###### ✏️ Created by **Basile Jeannot** and **Nino Faust**, for the Common Core at **42 school**.
