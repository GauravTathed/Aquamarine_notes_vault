### [[Trapping Ba137]]
Nico trapped 2 Ba137 ions in 5 attempts. 
Then trapped multiple times with dark ion. Finally trapped single ion after playing around with ablation and 554nm power:
![[Screenshot 2026-03-30 112405.png]]

#dailycalibrations 
![[Pasted image 20260330113540.png]]

| Freq       | pi times | transitions |
| ---------- | -------- | ----------- |
| 546.090835 | 98.346   | [0, 2, 0]   |
| 623.343681 | 24.871   | [0, 4, -2]  |
| 613.969584 | 26.882   | [-2, 3, -3] |
| 604.576268 | 27.478   | [2, 4, 3]   |
| 597.690696 | 33.053   | [2, 4, 4]   |

---
### Stable and Quite [[614nm]]
The standard deviation on 614nm laser seemed lower than usual so made a note about it

![[Pasted image 20260330202851.png]]
![[Screenshot 2026-03-30 202704.png]]
![[Pasted image 20260330203014.png|577]]

---
### [[BVA]] with arbitrary dimensions Simulations

| Mapping and exp info                                                                                                                                                                       | Results                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------ |
| `mappings = {0: '0', 1: '[3, -1]', 2: '[4, 1]', 3: '[2, 0]', 4: '[3, 0]', 5: '[4, 0]', 6: '[3, 1]', 7: '[4, -1]'}`<br><br>`initial_state = [[0,4,-2]]`<br><br>Line Signal Compensation OFF | ![[Pasted image 20260327152720.png]] |
| **SIMULATION** of above result                                                                                                                                                             | ![[Pasted image 20260330145714.png]] |
| **Simulation** with d=16 with Line Signal                                                                                                                                                  | ![[Pasted image 20260330151231.png]] |
| **Simulation** with d=16 without line signal                                                                                                                                               | ![[Pasted image 20260330153331.png]] |


![[Pasted image 20260330173341.png|697]]

We get d=16 BVA with 34.7% success probability. And again the most dominant source of noise after Line Signal is Laser Noise. 

That being said if we were able to reduce the pi-times (like we did when we focused the 1762nm to the ion) we can get about 50% success probability (And this without the short U1 pulse)

![[Pasted image 20260330175101.png]]

#### QuDit BVA data collection:
I made a new Algorithms script to run the BVA and Grover in Ion Control for arbitrary dimension of QuDit. The following is all the data we have so far:

![[Pasted image 20260331092722.png]]

![[Pasted image 20260331092745.png]]
