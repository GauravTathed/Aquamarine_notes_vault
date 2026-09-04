---
tags:
  - dailycalibrations
---
### [[Trapping Ba137]]
Yesterday we were having issues with isotope selectivity. So today we started the day with [[554nm scan]] to find the Ba138 ionization frequency at the spot that is giving very high fluence. 

The following are the results:

![[Screenshot 2026-03-25 114308.png]]

So it seems that actual peak for Ba138 at the high fluence region is 160 MHz higher. 
I tried parking at that frequency and trapping Ba138 with following parameters:
`Ablation energy = 70 uJ`
`554nm power = 11uW`

And I trapped multiple Ba138 in single ablation pulse, and i tried trapping with ionization frequencies turned Off and I wasn't able to trap. So these parameters should be low enough to not direct ion load. 

We trapped in 10 attempts with no dark ions:
![[Screenshot 2026-03-25 121218.png]]

This is the [[Ablation spot]] in question:

![[Pasted image 20260325123215.png]]
Here is the summery of parameters:
```
Ablation energy = 76uJ
554nm power = 19uW
Ba137 First Ionization Frequency = 541.433485 THz
std in 554nm frequency = 126 MHz
493nm power = 64uW
650nm power = 263uW
Ablation spot:
	x = 7.118
	y = 6.089
```

Lost the ion in $\sim$ 20 mins
I wasn't able to trap the ion after 200 more attempts. 
So I changed the spot and region. 
Now I am at 
```
x = 7.15
y = 6.068
```

And i am using Ablation power of 144uJ. But the big change I made is relocking the 554nm to be much more stable:
![[Pasted image 20260325204208.png]]
And this is what the scan looks like, much better than noisy crap we were seeing before. 
![[Pasted image 20260325204227.png]]

But trying to trap at this spot is a lost cause, so with this stabilized 554nm, I will go back to "good region"
```
x = 7.12
y = 6.09
```

This is what the 554nm frequency looks like 
![[Pasted image 20260325211407.png]]
and this is what the actual scans look like
![[Pasted image 20260325211452.png]]

This looks very clean. I will attempt to trap here. 
Trapped in $\sim$ 10 attempts after tuning the 650nm to sweep red. ![[Pasted image 20260326095643.jpg]]
The ion position looks shifted to the right (at least it not vertically OFF)
![[Pasted image 20260326095732.jpg]]

Unfortunately there was a dark ion in the trap which wasn't Ba138. So had to dump it and trap again. 
Trapped a single one in 20 attempts:

![[Pasted image 20260326095918.jpg]]
![[Pasted image 20260326095928.jpg]]

Checked the \[2, 4, 3] transition and [[Micromotion Compensation]]:
![[Pasted image 20260326100027.jpg|374]]

And did the 5 Freq Calibrations #dailycalibrations ![[Pasted image 20260326100106.jpg]]
Lost this ion in 20 mins again. 
So started the trapping script and went to bed. 