This page contains procedure that was followed on [[2026-03-11 — 1762 Alignment and RB data collection]] to align 1762nm laser to an Ion. 
The following assumptions are made:
- You are able to trap Barium isotopes (138, 137 or others)
- You have a basic optical path laid out for 1762nm. 

### Coarse alignment
1. Feed in 650nm light to the 1762 nm path. 
2. Before starting to align to the ion position make sure the following:
	1. There are only reflective elements ([ideally silver coated mirrors](https://www.thorlabs.com/protected-silver-mirrors?tabName=Overview)) in the Beam path. 
		 - This will make the alignment less dependent on wavelength of light - lenses will refract light differently based on wavelength. 
	2. The 650nm light is (near) centered on all the mirrors
		- This will give you full range of alignment later and minimize the clipping of the beam
		- It is import to note that Collimator will focus light based on wavelength - and that's OK. 
3. Now tweak the last and second last mirror to get the beam parallel to the optic table (and if possible to the ion height).
	- This will also ensure that your 650 is going through the viewport. 
4. Next align the 650 Beam so that its going through the middle of top and bottom rods and left and right mirrors. ![[Pasted image 20260311174548.png]]
	- From the CAD view above you can see that the trapping area that we see from viewport is much smaller than areas above and below the rods.
	- So you can sweep in vertical direction so you see the 650 coming out the other end of the chamber (starting from top going down). You expect to see one big region of  uninterrupted beam coming out $\rightarrow$ Beam will be interrupted by  the top rod, no beam will be seen out the other end. As the sweep downwards continues depending on weather or not you are horizontally aligned to be between the needles, you will see 650nm beam for small duration before being blocked by the needle, and you will see beam again for shot duration before being blocked by the bottom rods. And finally you will see a another long uninterrupted beam coming out.
	- You can average the position on the micrometer on the aligning mirror-holder so you are centered between the top and bottom rods. You can do the averaging on the horizontal direction to be in the middle of the Needles. 
5. If the Ion is Trapped you should see it fluoresce on the PMT. From here you can optimize the alignment to maximize the ion brightness on PMT.

### Fine Alignment 
Fine alignment procedure assumes that you have aligned with 650nm. 
1. After [[#Coarse alignment]] you should see 1762nm hitting the ion. You can confirm this by seeing the Ion flop (at 1762nm carrier or red-sideband) with 493nm ON, 650nm  ON and 614 nm OFF and 1762nm ON.
![[Screenshot 2026-03-11 181516.png|194]]![[Screenshot 2026-03-11 181434.png|405]]
2. From here you can you can do a frequency scan for with long (~3000 $\mu$s) pulse time, you should see a peak at resonant frequency. 
3. Park the 1762nm EOM at resonant frequency and do a Pulse Time Scan. Record the pi-Time and do a dummy variable scan at half of the pi-time. 
	- During the Dummy Variable scan you will tweak the alignment of 1762nm so minimize the PMT counts at half the pi-time. 
This should leave you with aligned 1762nm laser to the trapped ion. 