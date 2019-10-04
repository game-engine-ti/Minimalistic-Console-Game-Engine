# Minimalistic-Console-Game-Engine
# Introduction
This project is more for learning purposes. 
In this README, I want to explain, how to write a minimalistic game engine.

### Why TI-BASIC
Because it is simple!
If I would want to do this in c++, I would need to explain more advanced techniques, just to draw to the screen, even if I use the console. In TI-basic, I just need to write output(x, y, [text]). Also the rescources are limited. The program and the Variables are saved in the RAM, so I need to minimize both size of the program, and use as few variables as possible. I could've used python or so, to get access to bigger rescources, but imagine, that this wouldn't be a tiny project, but a huge engine for a large company.

### What do we want
- A movable character
- Objects, that kill the character
- Trigger, that can (de)activate objects
- Objects, affected by the Trigger
- Portals
- A graphical editor

## Tutorial

### A movable character
Let's start with the position. We want the engine to work in 2D, so our character should have an X-position and a Y-position. So let's declare X and Y. To start, let's give them the value 1. Also we should use ```ClrHome``` to make the screen blank:
```
:ClrHome
:1→X  
:1→Y
```
Now, we need to write the game-loop. For this, we use ```while 1=1```. This loop will go until it's interrupted(we will do that later), but by itself it will go forever. In every cycle of the loop, we will check for input via ```getKey``` and put it into the variable ```K```.
getKey returs the last input-button as an integer. In the TI-basic-dev-wiki there is a [graphic](http://tibasicdev.wdfiles.com/local--files/userinput/getkey.png) showing the number for every button on the calculator.  
If K (the variable, in which the last key is stored) equals the keyCode of an arrow button, X or Y should be incremented or decremented.
At the end, we output a character at position X/Y  
In code-form this is:  

    :ClrHome
    :1→X  
    :1→Y
    :
    :while 1=1
    :getKey->K
    :
    :­-(K=24)+(K=26)+Y→Y
    :­-(K=25)+(K=34)+X→X
    :
    :output(X, Y, "0"
    :end

The -(K=24)+(K=26)+Y→Y looks a little bit weird. But it all makes sense, if you remember how booleans are represented as integers, as zeros and ones. That means, that if K=24, -(K=24) becomes -1. This saves us much Ram space, and speeds the engine up, because the alternative would be  
  
    :if K=24  
    :X-1→X  
    :  
    :if K=26  
    :X+1→X  
    ...
If you run this code, you can already control a 0 through the screen. But it doesn't seem to move, it just creates a big line. To make the character seem to move, we need to ovverride it's last position. We could clear the screen with a clrHome, but, thats a slow solution, and when it comes to the real level, the calculator is to slow to write the whole level every frame. Thus we can only ovverride the last position. So here is the solution:  
At the begin of every loop cycle we write the X and Y coordinates in the variables U and V, and at the end, after the new X and Y coordinates are calculated, we write a whitespace to the U and V coordinates
(if the coordinates changed, but we can change that easier by using K≠0)

    :ClrHome
    :1→X  
    :1→Y
    :1→U
    :1→V
    :while 1=1
    :getKey->K
    :
    :X→U
    :Y→V
    :
    :­-(K=24)+(K=26)+Y→Y
    :­-(K=25)+(K=34)+X→X
    :
    :output(X, Y, "0"
    :
    :if K≠0
    :output(X, Y, " "
    :
    :end
Now you can move the 0 through the screen.  
Now let's start with the level-matrix.
To save the level, we're using a 10x27(the screen size is 10x26, the additional column is for infos about the level, but you can also use a 10x26 matrix instead)
Now we need to specify, what the data in the matrix stands for. A Matrix holds signed integer values, that means, data in the Matrix could be positive and negative integer values.  
We now need to think of objects, we want to implement in the game-engine.
For this project, I want to implement these objects, with name, what it does, and the numerical representation in the matrix.
- Collision object, player dies and goes back to start location when object and player position are the same, 1
- Start position, the start position in the level, -1
- End position, if End position and player position are equeal show a "Well done" message, -2
- Trigger, if touched, triggered_blocks go away, and negative_triggered_blocks activate, -3
- Triggered_block, only shown when triggered, player can get through, if triggered, -4
- Negative_triggered_block, only shown when not triggered, player can get through, if not triggered, -5
- Portal, there need to be two portals, if player stands on portal, new playerposition = second portal position, -6

Before the Game loop, we want to draw the whole level to the screen. For this, we use two for loops reading the whole matrix and drawing the data to the screen. The character drawn is determined by if-conditions testing if the matrix data equals one of the objects listed above.  
Code    
  
    :ClrHome  
    :1→X  
    :1→Y
    :1→U
    :1→V
    :for(S, 1, 10
    :for(L, 1, 26
    :
    :If [A](S,L)=1
    :output(S, L, "π" //Collider
    :
    :If [A](S,L)=-1
    :output(S, L, "S" //Start position
    :
    :If [A](S,L)=-3
    :output(S, L, "E"
    :
    :If [A](S,L)=-4
    :
    :
    :
    :end
    :end
    :
    :while 1=1
    :getKey->K
    :
    :X→U
    :Y→V
    :
    :­-(K=24)+(K=26)+Y→Y
    :­-(K=25)+(K=34)+X→X
    :
    :output(X, Y, "0"
    :
    :if K≠0
    :output(X, Y, " "
    :
    :end






