### [[Trapping Ba137]]
Trapping Ba137 - were trapping pretty fast but the ion life time was around 3 hrs.  
![[Screenshot 2026-04-01 172643.png]]
![[Screenshot 2026-04-01 231629.png]]
![[Screenshot 2026-04-02 130620.png]]

The old [[Ablation spot]] was dying so I found a new one in the same "good region". 
![[Pasted image 20260403132500.png]]
And the following is the [[554nm scan]] there. 
![[Screenshot 2026-04-02 161556.png]]

Good peak at 541.43306THz. 
The Trapping Life-time seems to be solved after reducing 493nm power from ~77uW to 47~uW. 

These are the [[Laser Powers]] we should be using 

| Laser (nm) | Power             |
| ---------- | ----------------- |
| 493        | $\sim$ 49$\mu$W   |
| 650        | $\sim$ 250$\mu$W  |
| 554        | $\sim$ 14$\mu$W   |
| ablation   | $\sim$ 90$\mu$J   |
| 614        | $\sim$ 6$\mu$W    |
| 389        | $\sim$ 1700$\mu$W |

---
### [[BVA]]
These are the QuDit BVA results so far:
![[Pasted image 20260403133827.png]]

And this is how it compares to simulation:
![[Pasted image 20260403133850.png]]

It is important to note that we are using Full QFT decomposition here for both U1 and U3. But we know that we can get away with using short U1 (by making use of the fact that the initial state is always going to be $\ket{0}$). This short U1 will have $d-1$ couplings to get equal superpositions transferred to each state. 

Today (3rd April) I added short U1 unitary to make the initial superposition before the oracle is applied. Here is the simulation result from using and not using short U1

![[Pasted image 20260403143843.png]]

And here is the error breakdown

![[Pasted image 20260403143903.png]]

Data so far
![[Pasted image 20260403202935.png]]