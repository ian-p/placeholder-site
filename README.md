Oh, you're the source-checking kind. Welcome, Stranger!

The way it's generated is pretty simple: Create a vector field using Perlin noise, then randomly put particles in it and let them flow, saving their trails to an array. Reject those that overlap with others. They are rendered using SVG circles, placed along each trail.

Here are some more details if you'd like to modify my code or write your own:


## Vector field

This article: https://www.tylerxhobbs.com/words/fidenza is a great starting point. The author pretty much explains everything and mentions collision detection, without many details though.
Trails

Each of the lines you see is a trail of a particle that went through the vector field. To fit them nicely on the canvas, first the thickest are drawn, then medium-sized, and then the smallest. There are a few presets (sizeConfigs) describing the amount and thickness of each group. One is randomly picked at the beginning.


## Collision detection

Adding a new particle trail works like this: start at a random point for 500 steps: move along the vector field if the newly added segment would collide(1) with any existing ones, break if the trail is shorter than X, discard it and start over(2) calculate AABB and add to trails

(1) Checking for collision first checks AABB (see https://noonat.github.io/intersect/) so it doesn't take ages, and if AABB collides, does precise checks against all existing segments. Note that the precise checks are done using constant size (trail.size) along the whole path and don't take the size changes that happen during rendering into account. It's easy to precalculate and store radii, but visually there wasn't much difference, so I didn’t do so.  
(2) Most of the trails are discarded; check the console. Example for a wide-screen run: 1986 added, 20535 discarded.


## Rendering

At the beginning, one of the palette presets is randomly selected. Trails are colored using colors from this palette.

Each trail is rendered using a series of SVG circles. The radius goes from 0 to trail.size and back to 0 using a sinus function. There's some gradient added as well.


## Other

There are a thousand things you can do from here. Feel free to steal this code and have fun!  
  
-- *Jan*