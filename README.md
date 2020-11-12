# **Asteravoid**
2D Asteroid-themed temple run type game. Made with MonoGame-framework in C# for better understanding of coding practices. 
Thís is a side project provided by my SD teacher Jakob Krarup @gitID.

## Scope
Your task is to implement a game where you navigate a spaceship through hostile space, while asteroids are trying to disintegrate your ship.
The game has asteroids floating down from the top of the screen, while you try to navigate your spaceship horizontally, left and right, along the bottom of the screen to avoid them.
For each asteroid you avoid (Asteravoid - get it? ;) ) you get a point!
The points are scored when the asteroid is fully off the bottom of the screen.

The game screen is locked at 1600x1200 pixel resolution for now.
The spaceship is controlled using left/right arrows on the keyboard.
If your spaceship touches an asteroid, you die and restart the game.

## Suggested progression of creation:

### Design
Identify the classes you need and what data they need to store (create a domain model if that helps you identify the initial state of all objects including data and relationships).
* Where are the objects on-screen?
* What is their direction and speed (movement on y-axis)?
* How does the game end - and how is it reported to the player?
* How does the player restart the game? (press Enter?) - and how do you reset all the objects in the game? Do you release all objects to the Garbage Collector or reuse them by resetting their values (position, speed, etc.)

### Implementation
Make spikes, to ensure you understand how the MonoGame framework and the algorithms work.

**Test automatic movement**
Create a class Sprite that has the properties:
* An "Image" Texture2D
* A "Position" (Point or Vector)
* A "Speed" (number of pixels moved on y-axis per update)
Add an Update() method to the Sprite class which updates the position
For each call to Game.Update() - call the Sprite class' Update metho
For each call to Game.Draw() - draw the Sprite's Image on screen in the updated position
Best practice: Draw game images centered on the Position of the object (drawn left of the "Position" by half of the Texture2D's width and above the "Position" by half the height).

**Test movement based on input**
Create a Sprite instance which holds the spaceship texture.
For each call to Update - test whether the left/right arrows have been pressed and update the spaceship's location on the X-axis. Ensure you don't let the spaceship move outside the game-boundaries.

**Test collision detection**
A simple collision test is to use Pytagoras to determine the distance between two objects.
Add an asteroid to the game, level with the spaceship but off to the side (left or right).
For every call to Game.Update() make a call to a separate method CheckForCollision(), where you determine the distance between the Position of the spaceship and the Position of the asteroid. If the distance is below half size of the spaceship plus half size of the asteroid, you have a collision.

**Add eyecandy**
if you add a parallax scrolling starfield (e.g. many small star sprites that scroll vertically with three different speeds), you can give the impression that you're zooming through space.

### Suggested best-practice for implementation

**Implement object pooling (instance reuse)**
Instead of releasing the asteroid objects to the Garbage Collector, when they have moved offscreen and then instantiating new ones above the top of the screen, reuse the asteroids: Instantiate all asteroids above the top of the screen, at different heights, so they arrive separately, and when they have moved fully below the bottom of the screen, update their y-axis, so they are "teleported" above the top of the screen again and set a random x-axis value between 0 and 1600.

**Encapsulate update/draw functionality in the Sprite class**
If you send the SpriteBatch (used to draw Texture2Ds to the screen) to the Sprite class in the constructor, you can have the Game.Draw() call the Sprite.Draw() method, and have the Sprite draw itself to the screen, instead of having the Game.Draw look up all Sprites' Positions and Images in order to be able to draw them.
