INVADERS HACK BY MARTIN SKARREGAARD
-

####OVERVIEW OVER CHANGES
- Color of all sprites have been changed from white to red
- Invaders turns invisible at random intervals (adding difficulty)
- Jump function added for the spaceship (no real purpose at the moment)
- Gravity physics
- Ship collision detection on the bottom border
- Invader collision detection on the bottom border
- Invaders becomes faster the fewer there are
- Invaders formation is randomly selected from a series of csv files that includes the 
formations (current maximum invaders is 50)
- Game is now over when all invaders in the random formation is 'dead'


####IN INVADERS_APP.H


######Sprite (Invaders) Alpha
In the "sprite" class I added a "bool alpha" to control the alpha value of the sprites. 
I have specifically used it for controlling the alpha of of the invader sprites.

In the sprite class' "void render()" function (shader render), all textures' alpha 
value are set to be the initial value of the "alpha" variable mentioned above. 
The value is "true", meaning that all textures are visible at this stage.

Further down in the sprite class, a simple set_alpha(bool newAlpha) function has been added, 
so it is possible to set a new alpha value outside the sprite class.
![Image SetAlph function](https://github.com/skarregaard89/octet/blob/master/octet/src/examples/example_invaderers/Images%20for%20Readme/SetAlpha.png)
*image_caption*

The "set_alpha" function is called in the "void invisibleInvaders()" function, which is 
inside the invaders_app class. The "invisbleInvaders" function calculates a 'random' number 
between 1 and 10. Depending on the calculated number, it sets all invaders' alpha to either
true or flase (visible or invisible)
![Image invisibleInvaders function](https://github.com/skarregaard89/octet/blob/master/octet/src/examples/example_invaderers/Images%20for%20Readme/invisibleInvaders.png)

The "invisibleInvaders" function is finally called inside the game loop "void 
simulate()" function in the "invaders_app" class. This means that a 'random' number will be
calculated at each frame, which decides whether the invaders should be visible or invisible 
in that specific frame. 
![Image invaders become invisible](https://github.com/skarregaard89/octet/blob/master/octet/src/examples/example_invaderers/Images%20for%20Readme/InvadersGoInvisible.png)


######Getting the spaceship position
A "vec2 get_pos()" function was created in the sprite class. This function returns the
position of an object/sprite. The "get_pos()" function is called inside the game loop "void 
simulate()" function in the "invaders_app" class. It is called to get both the x and y 
position of the spaceship each frame. The two positions are stored in a "ship_position" 
structure. 
![Image Get position code](https://github.com/skarregaard89/octet/blob/master/octet/src/examples/example_invaderers/Images%20for%20Readme/getPosition.png)


######Invaders collides with the bottom border
In the "invaders-app" class' "bool invaders_collide(sprite &border)" function I have added
an if statement, which checks if any of the invaders collides with the bottom border. If
this happens, the game ends.



######Gravity and jumping
In the "invaders_app" class I have created the "void gravity()" function. This function
continually translates the ship negatively in the y-position (pulling the ship down). The 
function includes an if statement that detects the collision with the lower border and 
ensures that the ship cannot go through the border. Also, the if statement sets the variable 
"height_limit" to false. 
As long as the "height_limit" is false it enables jumping in the "void move_ship()" function.
This means that whenever the up key is pressed, the ship is moved positively in the 
y-position by using "jump_force" and neutralizing the gravity. When the ship reaches a 
y-position at "2", then the positive translation in the y-direction is disabled, until the 
ship collides with the lower border again.



######Spawning invaders
To spawn the invaders I created 6 different CSV-files with different spawning formations for
the invaders. When the game starts is 'randomly' chooses one of the CSV-files to use. This  
all happens in the "int outputCSV()" function in the "invaders_app" class. The function 
starts by calculating a 'random' number between 1 and 6. The 'random' number is then 
converted into a string, using the "std::string IntToString(int number)" function. The 
converting is done, so the 'random' number can be used as a part of a string, when the CSV-
file are looked up. 
When the 'random' chosen CSV-file has been loaded, then the "void readCSV(std::istream 
&input, std::vector < std::vector <std::string> > &output)" function is used to read the
file and pass data into the "csvVector csvData" variable, by using pass by reference.
"csvData" is then used in a double for loop, which goes through all elements in each row and 
column of the 2 dimensional vector. If the element is 1, then an invader is spawned.
If the element is 0, then it just continues to the next element. The for loops uses the 
following thre variables: "int row", "int col", "int numberOfInvaderSpawned". The first two
variable keep track of the current row and column. So if the element is 0 and nothing is 
spawned, the row and column count will still change, so the invaders are spawned in the 
correct place. In this way the invaders are not just spawned right next to each other. 
The third variable "numberOfInvadersSpawned" helps ensure that the code is not trying to
spawn more invaders when maximum limited has been reached ("num_invaders"), which in this 
case is 50. This could lead to a potential data breach. A certain range in the sprite array 
have been assigned invader sprites. By counting the spawned sprites/invaders it can be
ensured that the range is not exceeded. Furthrmore the "numberOfInvadersSpawned" variable is
used to figure out when all invaders have been killed and the game should end. This happens 
in the game loop.

The whole csv-file function mentioned above is called in the "void app_init()" function, 
which is called once when OpenGL is initialized. The "void app_init()" function is the 
function responsible for drawing the game elements. 



######Invaders moves faster
In the "invaders-app" class' "void on_hit_invaderer()" function I have added some extra else
if statements. These statements check how many invaders there are left and the speed of the
invaders are multiplied by two every time.


####IN INVADERS_APP.H


######Making stuff red 
To make all sprites red I have simply created the "vec4 color = vec4(1,0,0,alpha)" variable.
The red value of this vector is set to 1, while the green and blue value have been set to
0. "color" is then multiplied with the texture variable/vector to create the final shader.
In this case it results in everything being red. 



######prite (Invaders) Alpha
To be able to change the alpha value for the sprites (in this case the invaders), a new 
uniform variable had to be created in the fragment shader. Including the uniform in the
"void render(const mat4t &modelToProjection, int sampler, bool alpha)", makes it possible 
to access the alpha value and change it in the "invaders_app.h"
