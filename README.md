# SCOTS Controller Synthesis

This repo contains the SCOTS package used as well as images and notes from my experiments with SCOTS to perform automated controller synthesis.

Details about the SCOTS package can be found [here](https://www.hyconsys.com/software/scots/)

## Generic Unicycle Model

The dynamics I chose to work with are just a simple unicycle, with three state variables, and two input variables which I show discretization for later. More details about the model can be found in the paper.

![Unicycle Model](/img/unicycle-model.PNG "Unicycle Model")

## Unicycle Model Growth Bound

All of our state variables update every timestep according to a function plus some noise. The timestep tau must be positive. Our goal is to find a parameterized matrix, which in code will look like the bottom left.

![Growth Bound](/img/growth-bound.PNG "Growth Bound")

## Unicycle Model Discretization Choices

Tau is 0.3 as mentioned earlier. State space is [0,10]x[0,10]x[-pi,pi] (plus some extra). I chose to slice it into 0.2 sized segments, though this could be an issue for radians because 0.2 radians is 11.45 degrees which is quite a bit. However, due to computation time, I found this to be reasonable for experiments

![Discretization](/img/discretization.PNG "Discretization")

## Comparing Reach vs Reach Avoid

If you remove the polytopes from the allowed state space, it has the same effect on the trajectory as adding the obstacles in that same area. This leads to better computation time, because you are evaluating less space and doing less collision checks. However, if you want to go back and make changes to obstacles, this is not feasible, which is why reach avoid exists.

![Reach vs Reach Avoid](/img/reach-vs-reachavoid.PNG "Reach vs Reach Avoid")

## Results Table

Reach with obstacles took significantly less time than reachAvoid with obstacles. This is because the obstacles, which are similar to those in the example, take up a lot of the state space. By eliminating them, it reduces a lot of computations.

![Results Table](/img/reach-vs-reachavoid-two.PNG "Results")

## Observing Three D Boundaries in 2D

I wanted to look at the domain of the controller in this 3D space a bit more and visualize that.

![3D Boundary](/img/three-d-boundary.PNG "Growth Bound")

## Examples of Restricting Theta to an Allowable Region

Below are two examples where the angle, theta, was restricted to only be within the angle green-zone as shown in the image. You can see it is able to go up and down, or left and right, because it can also go forwards and backwards.

![Finding Theta](/img/finding-theta-three.PNG "Finding Theta Three")

![Finding Theta](/img/finding-theta-four.PNG "Finding Theta Four")

## Plotting x,Theta and x,y on the X,Y 2D Axis

The red line indicates x versus theta, and the blue line represents the x,y coordinates of the unicycle. We add obstacles in the statespace across the entirety of the x,y axes, but restrict theta so the unicycle can only move horizontally during the first part of the trajectory, then can move freely, then is restricted to only move vertically at the end of the trajectory, as represented by the large blue boxes from -2pi to 2pi on the Y axis. You can see the blue line (x,y) follow that initial horizontal path, trying to go upwards but being blocked, until it is forced to take an up/down trajectory at the end. Because the red line which represents x,theta never intersected with the boundaries, we are successfully able to satsify the reachability specifications and navigate to the goal state.

![3D Vis](/img/three-d-final.PNG "Three D Final")

## Extra: Tips to Setup

I found working on a Cloud VM for this purpose to be useful for several reasons. First, it allows you to parallelize your experiments. Second, it provides a reproducible environment. And finally, it can satisfy various hardware requirements, and can be set to whatever specification required to perform computations in the desired timeframe

![Cloud VM](/img/cloud-vm.PNG "Cloud VM")
