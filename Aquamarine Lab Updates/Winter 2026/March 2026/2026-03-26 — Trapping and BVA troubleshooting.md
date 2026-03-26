### [[Trapping Ba137]]
Yesterday I started the Ba137 trapping script before going to bed - I trapped in 11 attempts and the ion was in trap all night when i found it in the morning. 
![[Screenshot 2026-03-26 092039.png]]

So I checked for [[micro-motion compensation]] again and these were the results ![[Screenshot 2026-03-26 095402.png]]

I am only seeing the flat lines at basically all the voltages? The ion itself was moving in responce to changing the rod voltages but the micromotion pulse time scan wasn't changing at all. While we were doing this we lost the ion. 
(An ion that stayed in trap all night at micro-motion compensated voltages). 
I will leave the voltages the same as before and leave micromotion alone for now.
Started the trapping script and trapped in:
![[Screenshot 2026-03-26 100718.png]]

At least the trapping is on point. 
#dailycalibrations 
![[Pasted image 20260326101729.png]]

### [[BVA]]

Last time I got some weird results for 8 level BVA. So I will replace the Hadamard with the one from TAQR and try again. But first just to see everything is fine i will do a 4 level Hadamard with and without the Line signal compensation. 


| Mapping                                                                                                                            | Result                               |
| ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| ```mappings = {0: '0', 1: '[3, -1]', 2: '[4, 1]', 3: '[2, 0]'}<br>initial_state = [[0,4,0]]```<br><br>This is insensitive mapping. | ![[Pasted image 20260326103508.png]] |
|                                                                                                                                    |                                      |
|                                                                                                                                    |                                      |
