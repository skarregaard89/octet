-------------------------------------------------------
----------INVADERS HACK BY MARTIN SKARREGAARD----------
-------------------------------------------------------

----------OVERVIEW OVER CHANGES----------

- Color of all sprites have been changed from white to red
- Invaders turns invisible at random intervals (adding dificulty)
- Jumpfunction added for the spaceship (no real purpose at the moment)
- Gravity physics
- Collission detection on the bottom border
- Invaders formation is randomly selected from a series of csv files that includes the 
formations (current maximum invaders is 50)
- Game is now over when all invaders in the formation is dead

----------IN INVADERS_APP.H----------

>>>>Sprite (Invaders) Alpha<<<<

In the "sprite" class I added a "bool alpha" to control the alpha value of the sprites. 
I have specifically used it for controlling the alpha of of the invader sprites.

In the sprite class' "void render()" function (shader render), all textures' alpha 
value are set to be the initial value of the "alpha" variable mentioned above. 
The value is "true", meaning that all textures are visible at this stage.

Further down in the sprite class, a simple set_alpha(bool newAlpha) function has been added, 
so it is possible to set a new alpha value outside the sprite class.
The "set_alpha" function is called in the "void invisibleInvaders()" function, which is 
inside the invaders_app class. The "invisbleInvaders" function calculates a 'random' number 
between 1 and 10. Depending on the calculated number, it sets all invaders' alpha to either
true or flase (visible or invisible)

The "invisibleInvaders" function is finally called inside the game loop "void 
simulate()" function in the "invaders_app" class. This means that a 'random' number will be
calculated at each frame, which decides whether the invaders should be visible or invisible 
in that specific frame. 




>>>>Getting the spaceship position<<<<<

A "vec2 get_pos()" function was created in the sprite class. This function returns the
position of an object/sprite. The "get_pos()" function is called inside the game loop "void 
simulate()" function in the "invaders_app" class. It is called to get both the x and y 
position of the spaceship each frame.


 