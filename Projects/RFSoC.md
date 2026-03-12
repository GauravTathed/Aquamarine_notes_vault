# RFSoC 

### Goals: 
The Goal of this project is to find replacement for the Agilent 81180B AWG. The central difference between the two sources will be parameterizing the waveforms instead of upload the entire waveform sampled at 4 giga samples per second. This full upload procedure with AWG takes a long time (only increasing with longer wait times) and fills up AWG memory fast (memory limits us to uploading 3ms of waveform).

RFSoC will take take in parameterized waveform (parameters like frequency, duration, times etc.) Making uploads much faster and memory limited my bus transfer limit. 
## Work with ZCU111

## Experimental Verification

