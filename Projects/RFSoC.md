# RFSoC 

### Goals: 
The Goal of this project is to replace the Agilent 81180B AWG as our 1762nm EOM frequency source. The central difference between the two sources will be parameterizing the waveforms instead of uploading the entire waveform sampled at 4 Giga samples per second. This full upload procedure with AWG takes a long time (only increasing with longer operation times) and fills up AWG memory fast (memory limits us to uploading 3ms of waveform).

RFSoC will take take in parameterized waveform (parameters like frequency, duration, times etc.) Making uploads much faster and memory limited my bus transfer limit. 
## Work with ZCU111



## Experimental Verification

