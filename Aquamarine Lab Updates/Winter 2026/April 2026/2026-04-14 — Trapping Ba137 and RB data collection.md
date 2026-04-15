### [[Trapping Ba137]]

![[Screenshot 2026-04-14 090550.png]]

We trapped 2 ions - the first attempt you can see checked for Ba138 and it returned as not positive - the script tried to sweep the 493 and i saw that Ba137 fluoresced after the check but on the third sweep it did not come back up. So when the Script checked for Ba137 in the trap it didn't detect any. And it continued trapping. 
Trapped in 4 attempts after that. 

![[Pasted image 20260414140638.png]]

![[Screenshot 2026-04-14 090603.png]]

### Qubit [[Randomized Benchmarking]] data collection

We found the issue with the script - it was when putting together a list to feed into ion control, we were overwriting the row 3 and row 4 and not writing on row 5 and 6. This caused the row 5 and 6 to use the previously set values for them. 

After fixing that here are the results from quick check:

![[Pasted image 20260414141349.png]]

Where green and the red one are new data. They match the old good results well. 

We are now collecting data for [0, 3, 2] Compensated and uncompensated with sets of  40 unitaries. 

### Line Signal Compensation paper figure:
Today in group meeting I showed a version of first figure today this is what it looked like 
![[Line_signal_Compensation_Figure1_explanation.pdf]]
