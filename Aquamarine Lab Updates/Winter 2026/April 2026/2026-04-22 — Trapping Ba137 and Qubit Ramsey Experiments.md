### [[Trapping Ba137]]

Today we trapped in 24 attempts and Collin noticed that ion brightness was intermittently fluctuating again. But after changing the needle voltage that intermittent decrystalization stopped. 

Aligned the 1762nm laser:

#dailycalibrations before alignment
![[Pasted image 20260422125957.png]]

#dailycalibrations After alignment
![[Screenshot 2026-04-22 134131.png]]

### 650nm Repump modulation scheme

In my thesis i mentioned the fluorescence laser modulation scheme:

![[Pasted image 20260422172100.png]]

And the updated sideband frequencies for 650nm after installing the DS instruments are:

```
Sideband1: 885 MHz (Drives D32 F=2 to P12 F=1)

Sideband2: 525 MHz (Drives D32 F=1 to P12 F=1)

Sideband3: 382.5 MHz (Drives D32 F=0 to P12 F=1)
```

Note the following [[Energy Level Structure]]:

![[Energy_level_structure_ba137.png]]

The $^{138}\text{Ba}^{+}$ frequency used in our lab is 461.31213THz. So to apply isotope switch so that we address $6P_{1/2}, F=2 \leftrightarrow 5D_{3/2}, F=3$ is +558MHz - 438MHz. So the carrier frequency for $^{137}Ba^+$ is 461.31223 THz. But we have been using an isotope shift of -810MHz which gives us 461.31133. Which is much closer to this modulation scheme:
![[Pasted image 20260422193843.png]]
Here our sidebands are at most 50MHz away from ideal frequencies. With our 650nm power being 1000s of times of saturation intensity we were able to fluoresce the ion. 
To go back to the modulation scheme described in my thesis I tried going to 461.31225 THz which looked too blue on the ion. So I parked at 461.31223 THz. With this carrier these are the energy level spacings for our sidebands:
![[Pasted image 20260422195408.png]]
Coincidentally these sideband frequency match our values quiet well. 

We will check if this new 650nm frequency is any good for cooling or bad for cooling by doing a [0,2,0] Ramsey scan. 

| 650nm Frequency           | Plots                                |
| ------------------------- | ------------------------------------ |
| 461.31223 THz             | ![[Pasted image 20260422202544.png]] |
| 461.31133 THz             | ![[Pasted image 20260422202646.png]] |
| 461.31223 THz<br>FNCS: ON | ![[Pasted image 20260422202739.png]] |

So it looks like it doesn't hurt to park the 650nm frequency here. 
### Qubit Ramsey Troubleshooting
Yesterday we saw that taking additional data for qubit Ramsey gave us much cleaner results. Today I will check if [[FNCS: Fiber Noise Cancelling system]] can give us cleaner and more reproducible data. 
Here are the results:

![[Pasted image 20260422201819.png]]
![[Pasted image 20260422201830.png]]![[Pasted image 20260422201836.png]]

we see about the same performance if not worse. So made an executive decision to keep the FNCS OFF. 
- Side note: We actually don't expect the FNCS to solve this problem because the laser phase drift that we see on the FNCS screen is very slow compared to experiment time, so fiber phase drifts are almost negligible for duration of one experiment.
So the data collection plan is the following - We will take multiple runs with 100 shots each and aggregate the data together before fitting the Ramsey fringe this way any slow drift in magnetic field which chirps our Ramsey fringe will cancel out and we will get true amplitude or behavior of our system at that wait time.

