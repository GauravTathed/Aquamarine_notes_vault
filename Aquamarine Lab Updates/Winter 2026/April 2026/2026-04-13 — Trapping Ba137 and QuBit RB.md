### [[Trapping Ba137]]

Trapped in about 75 attempts?

Pi times are very long today - I will align the laser. 
![[Pasted image 20260413115333.png]]

### Troubleshooting [[Randomized Benchmarking]]Ion Control Script

Turns out that switching the Line Signal to the most recent measurement might have solved the issue. 
We were using the measurement from 2 months ago. I am updating it with measurement done on [[2026-04-01, 02, 03 — Trapping Ba137 and Arbitrary dimension BVA Results and Simulation]]

![[Pasted image 20260413172238.png]]

This is what the difference looks like between the two:
![[Pasted image 20260413172325.png]]

This did not solve the issue. 
##### **Summary** 
Here's the summary of what the issue is:
On [[2026-03-11 — 1762 Alignment and RB data collection]] and [[2026-03-12 — RB data collection]] we collected qubit Randomized benchmarking data for [0, 3, 2] transition. And this was the result we got:
![[Pasted image 20260414085119.png]]

We also took bunch of other qubit transitions on those days. But recently we went back to the same script to collect the same data for qubit RB and we were seeing:
- The overall gate fidelity seems to have decreased slightly
- AND Line signal compensation doesn't seem to make a difference. 
This was the first data set collected which showed issues:
its a [0, 4, -2] Qubit — sensitivity: -3.42MHz —  num_sets: 40
![[Pasted image 20260414092111.png]]

There was a theory that maybe Line Signal itself is introducing noise. Idk how we would test this, we did a BVA with and without compensation and the results looked as expected. 

Here's a result for [0, 2, 0] Randomized Benchmarking with and without compensation. 

![[Pasted image 20260414085430.png]]

We switched to using the most recently measured Line Signal and here's the result.  
![[Pasted image 20260414084549.png]]

Collecting Finer data for Bussed QuBit Ramsey - so we don't have to relay on wrapping and theory reference as much. 

Data collected so far:

| Transitions                      | Date Time                                                                        |
| -------------------------------- | -------------------------------------------------------------------------------- |
| [0, 3, 2]<br>LT_comp = False<br> | ['20260413_2156', '20260413_2236']<br>That's up to 1900us with 100 us step size. |

