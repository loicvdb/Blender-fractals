# Blender-fractals
Custom OSL shaders to create fractals in Blender

# Instructions:
To make a fractal, open a new 1.79 blender file and keep the default cube, the fractal will be inside of it. Change the engine to cycles, put your CPU as your rendering device and tick the "Open Shading Language" box just under it. Then edit the material of the cube, add two script nodes, choose the "external" type and select the juliabulb and coloring shader. You'll then need to connect a combineXYZ to the "C" input of the juliabulb shader if you want to change the values (the default ones are (.5, .5, .5)).

Here's an example of a correctly set up material:
![alt text](https://i.imgur.com/FYhRpR4.png)



# Documentation:
**the juliabulb shader:**
 - Power : changes the shape of the fractal
 - Iterations : more iterations give more details but makes the shader slower
 - Bailout : the radius of the sphere containing the fractal, values lower than
 - Scale : the scale of the fractal, if the haze surrounding the fractal is too big for the fractal, just lower the scale
 - C : changes the shape of the fractal (connect it to a combineXYZ to tweak the values)
 - DistanceDelta : it moves the bound of the fractal outside or inside, keep this value at 0 to get the most details
 - *output* : The distance to the fractal

**the coloring shader:**
 - Distance : the distance to the fractal (should be linked to the output of the juliabulb shader)
 - Density : the density of the volume inside of the fractal (more dense = more opaque)
 - ColorIn : the color of the volume inside the fractal (is reversed when using high density, blue is yellow, green is violet etc..)
 - HazeColor : the color of the haze arround the fractal, if you don't want haze just put HazeAlpha to 0
 - HazeClosener : if you want to keep the haze close to the fractal, raise this value
 - HazeAlpha : the density of the haze
 - Anisotropy : anisotropy for the volume shader
 - *output* : A volume shader
