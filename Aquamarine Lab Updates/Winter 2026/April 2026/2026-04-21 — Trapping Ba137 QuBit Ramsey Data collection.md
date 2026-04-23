### [[Trapping Ba137]]

This morning we weren't able to trap a single Ba137 Ion in 200 attempts, after this morning's meeting we turned up the temperature of 554nm by around 0.4$^\circ$C. 

Then did a [[554nm scan]]:
![[Screenshot 2026-04-21 130902.png]]

Looks pretty good, time to trap. 
![[Screenshot 2026-04-21 135358 1.png]]
Trapped in 12 attempts with dark ions, then trapped in 37 attempts

### Qubit Ramsey Troubleshooting
Today in meeting we discussed the problem we are facing with with qubit Ramsey data collection. Crystal brought up a good point which is we are seeing chipping in the Ramsey fringes so the phases we fit to might be complete garbage. Like so:
![[Pasted image 20260422130034.png]]

![[Drawing 2026-04-22 13.11.02.excalidraw]]
Since wrapping depends on previous data point a sequence of bad runs through everything off course. 
So to address this we took repeated measurements of phase scans at the same Ramsey wait times so see how repeatable are they.
Here are the results:
![[Pasted image 20260422135241.png]]

Here each row shows a single Ramsey wait time - so in theory plots in each row should be the same. We see that this stops being the case of 5000us of wait time. Now if i aggregate all the data with same wait time into one plot and try to fit that data I get:

![[Pasted image 20260422171329.png]]


Where the amplitude decay looks like this:
![[Pasted image 20260422171442.png]]

### Qubit [[Randomized Benchmarking]] final plots:

![[Pasted image 20260422105851.png]]![[Pasted image 20260422105859.png]]