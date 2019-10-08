# STLOrbiter
An orbit propagator for donuts, simple shapes, and custom STLs.


William Jun

Last Revised: 10/7/19


This app sets up and propagates known stable or custom orbits around donuts, simple shapes, or custom STLs. The custom STL function requires the Parallel Computing Toolbox and a supported GPU. The Toolbox is free for Georgia Tech students. (https://www.mathworks.com/products/parallel-computing.html)

## ------- INSTRUCTIONS -------

### Installation:
  1. Open MATLAB
  2. Migrate to a folder with "STL Orbiter.mlappinstall" inside and run the installer
  3. Install to MATLAB Apps
  4. Navigate to the Apps Tab in MATLAB and open "STL Orbiter"

### Donut Orbits:
  1. Choose a stable orbit from the dropdown menu (these will only be stable for the given donut size and mass)
  2. Press "Display Donut" to display the donut
  3. Press "Propagate" to see the orbit in action!
  4. To stop, press "Stop (Hold Down)"
  5. Have fun changing the donut's parameters, initial conditions, and integration time. See if you can find another stable orbit!!
  
### Simple STL:
  1. Choose a simple shape from the dropdown menu.
  2. Change the scaling / mass to your liking
  3. Choose a set of initial conditions
  4. Propagate and try to orbit the simple shapes!

### Custom STL:
  0. Make sure you have the Parallel Computing Toolbox AND a supported GPU (the app will check for both)
  1. Ensure that you have an STL file in the directory that MATLAB is looking at right now
  2. Type in the exact name of the STL file, including ".stl" at the end
  3. Change the scaling / mass to your liking
  4. Press "Display STL"
  5. Choose a set of initial conditions
  6. Propagate and try to orbit your STL! 
  
## ------- HOW THE PROPAGATOR WORKS -------
For the donut case, a ring of 500 point masses is created with the given radius, the mass of each point being the total mass / 500. The donut visualization is just that, a visualization, with the ring being concentric with the circular cross section of the donut.
  
For all cases except for the donut orbits (simple STL and custom STL), the propagator calculates the N-Body acceleration of the orbiter based on the vertices on the surface of the given shape. Each of these N vertices are given a mass of total mass / N. Therefore, this assumes the orbiter is orbiting a shell with uniform density. The variations in orbiters therefore come from the changes in surface topography.

The "custom STL" utilizes the GPU to speed up the orbit dynamics calculations. Some custom STLs can have upwards of 40,000 vertices and therefore the propagator will have to numerically integrate the N Body problem for 40,001 bodies. Becuase of this, all custom STLs are first reduced by a factor of 100 (vertices are removed evenly throughout the entire STL). The remaining vertices are fed into the propagator and converted to GPU arrays for quick processing. 

The orbits are propagated using ODE45 and some carefully crafted orbit dynamics functions (optimized for speed). However, the propagator is still accurate to within 1e-12 m. 

## ------- NOTES -------
  - Georgia Tech students can install the Parallel Computing Toolbox for free if you use your GT login!  (https://www.mathworks.com/products/parallel-computing.html)
  - The "Custom STL" feature will check if you have the Parallel Computing ToolBox AND if you have a supported GPU. If you don't have either of these, it will let you know and you will not be able to choose an STL.
  - The propagator does not currently have surface collision detection. If the orbiter falls into the shape, it will continue on orbiting. 
  - If you do fall into the shape, the app will slow down. This is because the propagator is dealing with much larger values of acceleration while still trying to be accurate within 1e-12. On the flip side, this means that the propagator will speed up when farther away from the shape. This is a bit unintuitive becuase it goes against what would really happen (speed up at periapsis and slow down at apoapsis). Therefore, keep in mind that the time aspect of the app may be warped. There is a tail that follows the orbiter and by default shows the past 20 time increments (increment size set in the app). This may provide a better understanding of time throughout the orbit. 
  - If you start with a large enough velocity to achieve escape velocity, the orbiter will just fly off at a near straight line. Try reducing your initial velocity
  - The "Stop (Hold Down)" button is sometimes unresponsive when the propagator is too busy chugging away. Try clicking and holding for 1 second multiple times. If it is still propagating, go ahead and close the app and restart.

## ------- FUN ORBITS TO TRY -------
  - Simple STL : Cone
      - r = [200;-500;400]
      - v = [0;30;0]
  - 
