
### [[Trapping Ba137]]
We have been trapping very consistently over this week: here are the logs:

```
27th 

Metadata,Ionization Freq.:541.433339:THz,Cooling Freq.:607.432020:THz,Repump Freq.:461.312240:THz,Window Start:160.000:us,Window Width:55.000:us,Cooling Sweeps Num:5:,Pulses Per Attempt: 10
11:00:19	Trapped after 39 attempts, Ion counts: 87.0,     0.0 std, Neutral counts: 1.7

Metadata,Ionization Freq.:541.433339:THz,Cooling Freq.:607.432020:THz,Repump Freq.:461.311340:THz,Window Start:160.000:us,Window Width:55.000:us,Cooling Sweeps Num:5:,Pulses Per Attempt: 10
15:44:07	Trapped after 47 attempts, Ion counts: 99.3,     0.0 std, Neutral counts: 1.5

28th 

Metadata,Ionization Freq.:541.433339:THz,Cooling Freq.:607.432020:THz,Repump Freq.:461.311340:THz,Window Start:160.000:us,Window Width:55.000:us,Cooling Sweeps Num:5:,Pulses Per Attempt: 10
14:18:37	Trapped after 10 attempts, Ion counts: 113.9,     0.0 std, Neutral counts: 1.9

Metadata,Ionization Freq.:541.433339:THz,Cooling Freq.:607.432020:THz,Repump Freq.:461.311340:THz,Window Start:160.000:us,Window Width:55.000:us,Cooling Sweeps Num:5:,Pulses Per Attempt: 10
14:45:10	Trapped after 32 attempts, Ion counts: 109.9,     0.0 std, Neutral counts: 0.2

29th 

Metadata,Ionization Freq.:541.433315:THz,Cooling Freq.:607.432020:THz,Repump Freq.:461.311340:THz,Window Start:160.000:us,Window Width:55.000:us,Cooling Sweeps Num:5:,Pulses Per Attempt: 10
12:01:03	Trapped after 27 attempts, Ion counts: 112.0,     0.0 std, Neutral counts: 0.7

Metadata,Ionization Freq.:541.433315:THz,Cooling Freq.:607.432020:THz,Repump Freq.:461.311340:THz,Window Start:160.000:us,Window Width:55.000:us,Cooling Sweeps Num:5:,Pulses Per Attempt: 10
15:23:38	Trapped after 1 attempts, Ion counts: 139.5,     0.0 std, Neutral counts: 0.4

30th 

Metadata,Ionization Freq.:541.433315:THz,Cooling Freq.:607.432020:THz,Repump Freq.:461.311340:THz,Window Start:160.000:us,Window Width:55.000:us,Cooling Sweeps Num:5:,Pulses Per Attempt: 10
06:40:44	Trapped after 12 attempts, Ion counts: 156.0,     0.0 std, Neutral counts: 1.0

Metadata,Ionization Freq.:541.433315:THz,Cooling Freq.:607.432020:THz,Repump Freq.:461.311340:THz,Window Start:160.000:us,Window Width:55.000:us,Cooling Sweeps Num:5:,Pulses Per Attempt: 10
22:17:23	Trapped after 40 attempts, Ion counts: 89.1,     0.0 std, Neutral counts: 1.3

```

I measured the Ba137 ion brightness at different powers of 493 and 650 for Quantum Ion. 

| PMT counts over 100ms | 493nm power <br>($\mu$W) | 650nm power<br>($\mu$W) | Background PMT Counts |
| --------------------- | ------------------------ | ----------------------- | --------------------- |
| 550                   | 53                       | 267                     | 58                    |
| 450                   | 41                       | 267                     | -                     |
| 350                   | 30                       | 267                     | -                     |
| 200                   | 15.7                     | 267                     | -                     |
| 150                   | 9.6                      | 267                     | 15                    |
| 525                   | 52                       | 259                     | 58                    |
| 450                   | 52                       | 144                     | -                     |
| 280                   | 52                       | 51                      | -                     |
| 150                   | 52                       | 13                      | -                     |
| 80                    | 9.5                      | 17.2                    | 15                    |
### QuDit [[Randomized Benchmarking]]

Nico has been collecting quDit Randomized Benchmarking data for his thesis. 

### Bussed Qubit Ramsey Phase scans

We have collecting data for the Line Signal Compensation paper paper for last couple of days. The plan was to and still is to collect data 100 experiments at a time and aggregate the data before fitting anything. These are the results for sensitive transition:

