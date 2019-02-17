# Blender-fractals
Custom OSL shaders to create fractals in Blender with Cycles


# Instructions:
To make a fractal, open a new blender file and keep the default cube, the fractal will be inside of it. Change the engine to Cycles, put your CPU as your rendering device and tick the "Open Shading Language" box just under it. Then edit the material of the cube, add two script nodes, choose the "external" type and select the juliabulb and materials shader. Then connect two volume shaders to the "Haze" and "InsideVolume" inputs of the material shader (you can put only one of the two if you want, the volume will be transparent if unconnected). You'll then need to connect a combineXYZ to the "C" input of the juliabulb shader if you want to change the values (the default ones are (.5, .5, .5)).

Here's an example of a correctly set up material:
![alt text](https://i.imgur.com/e40uGbo.png)


# Documentation:
**the juliabulb shader:**
 - Power : changes the shape of the fractal
 - Iterations : more iterations give more details but makes the shader slower
 - Bailout : the radius of the sphere containing the fractal, values lower than
 - Scale : the scale of the fractal, if the fractal goes out of the bounds of the object, just lower the scale.
 - C : changes the shape of the fractal (connect it to a combineXYZ to tweak the values)
 - DistanceDelta : it moves the bound of the fractal outside or inside, keep this value at 0 to get the most details
 - *output* : The distance to the fractal

**the material shader:**
 - Distance : the distance to the fractal (should be linked to the output of the juliabulb shader)
 - HazeClosener : if you want to keep the haze close to the fractal, raise this value
 - InsideVolume : the volume that will be inside of the fractal
 - Haze : the volume that will be outside of the fractal
 - *output* : A volume shader
 
 

 # Performances advices:
Rendering fractals in a general raytracing engine require a massive amount of calculations, that's why I'll give you a few advices to optimize your rendering time.
- Try different step sizes : The step size parameter (Render -> Geometry -> Volume Sampling -> Step Size) has a huge impact on details and rendering time. If you lower the step size from .1 to .01, you'll get 10 times more details, but it will take 10 times longer to render. Be sure to choose the largest step size for the level of detail you're looking for. Note that a smaller step size will reduce the noise too so you'll need less samples to get a good result.
- Choose your input shaders appropriately : There are 3 types of shaders you can use as input:
  * The volume scatterer shader : The volume scatterer shader takes require a massive amount of calculations, it's a self shadowing volume which means shadow rays will be traced towards the light sources and raymarch through the fractal on each step. Added to that the amount of samples required to eliminate the grain (~512SPP for a correct result), you'll often need to render during multiple days to get a good result on an average computer. I'll also suggest setting the number of volume bounces to 0 (Render -> Light Paths -> Bounces -> Volume) else it's almost impossible to get rid of the noise.
  * The emission shader : This shader is really fast to render since it just traces through the fractal once and you'll need a remarkably small amount of sample (I usually put 4 for a final render with a small step size)
  * The volume absorption shader: Same as above, except it can be even faster to generate. If you put a very dense (~1000) white absorption shader, it will stop the raymarching as soon as the ray touches the fractal and save rendering time, very useful for a solid black fractal.
 - Scale the fractal : the rendering time depends on the volume of the object you set the fractal material to, so you can raise the scale of your fractal to get more details without spending more time rendering.
- Choose your object wisely : Same as above, rendering time depends on the volume of your object, so if your fractal is quite spherical (like the default one), choose a sphere. The shape of your object must be close to the shape of the fractal you're trying to render to keep the volume as low as possible.
- Denoiser? : I prefer not to use the denoiser, it does a terrible job at denoising volumes, let alone volumetric fractals. It gives a "heavely compressed image" feeling and obliterates all the details of the fractal. My advice for denoising would be to manually denoise the image in post if you really want a clean image.


# credit :
If you want to make anything creative with it, do it, that will seriously make my day. What would make me even happier is if you credit me ([u/loic_vdb](https://www.reddit.com/user/loic_vdb)) for these shaders if you publish your results (I mean I didn't invent fractals so I'll never notice if you don't). Also if you have a reddit account [send your results to me](https://www.reddit.com/message/compose/?to=loic_vdb) so I can see them :)

