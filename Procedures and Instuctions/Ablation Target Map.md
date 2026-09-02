This page will detail how we characterize our ablation target and make a map of it with X and Y coordinates. 

For this you need access to IR camera set under the trap. This camera can be accessed by running a server on Raspberry pi and accessing that on the remain windows PC. 

_add details here_

Ones we have that you will put the ablation laser in low power mode. And flip the switch at the back to the internal trigger source instead of external. 

_add pictures here_

This way you will always have a visual of where the ablation laser lands on the target. 
Ones this setup is complete you will scan across the target to find the edges of it. And record the coordinates of the edges - eddies can be found by looking looking at the ablation laser on the target, finding where the laser drops off the target. 

Change the X and Y coordinates until you have at least 8 points.  There is a python notebook called `Ablation_target_mapping.ipynb` where you can enter these coordinates and get a plot of elliptical projection as well as de-wrapped circular projection. 


| ![[Pasted image 20260901212628.png\|483]] | ![[Pasted image 20260901212639.png\|477]] |
| ----------------------------------------- | ----------------------------------------- |
