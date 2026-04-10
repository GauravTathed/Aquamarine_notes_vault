### [[Power Interruptions]]

Today Morning Collin found that all the instruments attached to UPS turned off - Including the Ion Control VM and 1762nm Cavity Temperature Controller. So no meaning measurements can be taken today. I am starting new Procedures page called [[Lab Bring-up]]...

Time to Trap
### [[Trapping Ba137]]
Trapped Ba138 manually and checked that all the lasers were aligned. 
Trapped Ba137 in two attempts... 
We are on point for trapping. 

This is when we noticed the unstable brightness of the ion
![[Screenshot 2026-04-09 120336.png]]
This is concerning since there are a lot of things that that could be going wrong here. 
- ~~RF Signal to EOMs and AOMs~~
	- Checked the RF Signals on the Spectrum analyzer for stability
	- None of the RF Signals were fluctuating 
- ~~Powers of 493nm and 650nm lasers~~
	- We don't see powers fluctuating on the power meter (maybe Power meter isn't sampling fast enough but power seems stable for now since when the ion goes dark there is still significant background indicating 493 power being stable). 
- Excess heating
	- This looks viable ![[Pasted image 20260409122658.png]]
	- But the way ion goes dark on camera has not been observed before.  ![[Unstable_ion_vid.mp4]]
	- Also this is happening for Ba138 - seems 
- Dark Ions?
	-  We have Trapped Ba137 twice and this happened for both of them
	- We also trapped Ba138 and this was still happening
- Excess Micro-motion
	- Seems very unlikely because the current Ion was trapped yesterday noon. So we can retain the ion. 
Not even Kidding - The brightness issue fixed itself. I am currently doing daily calibrations at the following frequencies and powers:


| Wavelength | Frequency     | Power         |
| ---------- | ------------- | ------------- |
| 493nm      | 607.43203 THz | $\sim$ 47 uW  |
| 650nm      | 461.31134 THz | $\sim$ 250 uW |
| 554nm      |               |               |
Checking the [[micro-motion compensation]]

| ![[Pasted image 20260409143228.png]] | ![[Screenshot 2026-04-09 143252.png]] |
| ------------------------------------ | ------------------------------------- |
#dailycalibrations 
![[Pasted image 20260409143705.png]]

Lost the Ion after Calibration. 

Did a [[554nm scan]] just to check the peak because it was taking a while to trap, here's the scan:

![[Screenshot 2026-04-09 at 6.12.20 PM.png]]

Trapped after 39 attempts after the scan. 
### [[Beam Drift Camera and actuators]]
Worked on Motor mounts for actuators
![[Pasted image 20260409131704.png]]

I am modelling for New-Port mirror mounts. 
### [[Laser Noise]] Characterization


| Date and Time          | Plots                                |
| ---------------------- | ------------------------------------ |
| 20260409<br>1835, 1921 | ![[Pasted image 20260409193337.png]] |
| 20260409<br>1930, 2019 | ![[Pasted image 20260410093927.png]] |
| 20260409<br>2032       | ![[Pasted image 20260410093958.png]] |


