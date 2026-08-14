
# RFSoC 

### Goals: 
The Goal of this project is to replace the Agilent 81180B AWG as our 1762nm EOM frequency source. The central difference between the two sources will be parameterizing the waveforms instead of uploading the entire waveform sampled at 4 Giga samples per second. This full upload procedure with AWG takes a long time (only increasing with longer operation times) and fills up AWG memory fast (memory limits us to uploading 3ms of waveform).

RFSoC will take take in parameterized waveform (parameters like frequency, duration, times etc.) Making uploads much faster and memory limited my bus transfer limit. 
## Work with ZCU111



## Experimental Verification


## Work with RFSoC 4x2
Setting the file path so the RFSoC4x2 is visible in board select run these commands in tcl console on welcome page

```
set_param board.repoPaths [list "C:/Users/iamga/Downloads/rfsoc4x2_board_files"]
get_param board.repoPaths
get_board_parts *rfsoc*
```

Available clock files:
```
xrfclk module path:
/usr/local/share/pynq-venv/lib/python3.10/site-packages/xrfclk

Detected available clock frequencies:

LMK:
  122.88 MHz
  245.76 MHz

LMX:
  102.4 MHz
  204.8 MHz
  409.6 MHz
  491.52 MHz
  737 MHz
```

Make a new clock file that takes in external 10MHz frequency:
[[LMK04828_245.76_external_ref.txt]]

To make this I used the TICS Pro program with LMK04828B chip and used/imported the default `xrfclk` clock file:
[[LMK04828_245.76.txt]]

And I only made changes to CLKin and PLLs section to make it look like this:
![[Pasted image 20260616213757.png]]

Then for the initial test on the spectrum Analyzer I used the 10MHz out port as an input to the RFSoC4x2 `CLK_IN` port and played a test frequency of 87.654321MHz and this is what i see:

![[Pasted image 20260616214719.jpg]]

### Measurement of Rep Rate
FFT is not an option - it will take too long to get the resolution in $\sim$Hz. 

The repetition-rate signal is measured using digital quadrature demodulation, also commonly called digital down-conversion or I/Q demodulation. The purpose of the method is to convert a high-frequency sinusoidal signal near the expected repetition rate into a slowly rotating complex baseband signal. The rotation rate of this baseband signal gives the frequency offset between the input signal and a digital local oscillator.

Let the digitized photodiode signal be

$$  
x(t) = A\cos(2\pi f_{\mathrm{in}}t + \phi_0),  
$$
where $f_{\mathrm{in}}$ is the input repetition-rate frequency. In the FPGA, a numerically controlled oscillator generates a reference signal at a nearby frequency $f_{\mathrm{LO}}$. The local oscillator produces two reference waveforms:
$$  
\cos(2\pi f_{\mathrm{LO}}t)  
$$

and

$$  
\sin(2\pi f_{\mathrm{LO}}t).  
$$
To down-convert the signal, the input is multiplied by two local oscillator waveforms in quadrature. Using the sign convention where a positive phase rotation corresponds to $f_{\mathrm{in}} > f_{\mathrm{LO}}$, the in-phase and quadrature mixer outputs are

$$  
I_{\mathrm{raw}}(t) = x(t)\cos(\omega_{\mathrm{LO}}t),  
$$

and

$$  
Q_{\mathrm{raw}}(t) = -x(t)\sin(\omega_{\mathrm{LO}}t).  
$$

The negative sign in the quadrature branch is a convention. It makes the resulting complex signal equivalent to multiplying the real input by $e^{-i\omega_{\mathrm{LO}}t}$. With this convention, a signal above the local oscillator frequency produces a positive phase rotation.
Substituting the input signal into the in-phase branch gives
$$
I_{\mathrm{raw}}(t) = A\cos(\omega_{\mathrm{in}}t+\phi_0)\cos(\omega_{\mathrm{LO}}t).  
$$

Using
$$
\cos(a)\cos(b) = \frac{1}{2}\cos(a-b) + \frac{1}{2}\cos(a+b),
$$

we obtain
$$
I_{\mathrm{raw}}(t) = \frac{A}{2}  
\cos\left((\omega_{\mathrm{in}}-\omega_{\mathrm{LO}})t+\phi_0\right)  
+  
\frac{A}{2}  
\cos\left((\omega_{\mathrm{in}}+\omega_{\mathrm{LO}})t+\phi_0\right).  
$$

Thus, the in-phase mixer output contains two terms: one at the difference frequency $f_{\mathrm{in}}-f_{\mathrm{LO}}$, and one at the sum frequency $f_{\mathrm{in}}+f_{\mathrm{LO}}$.

Now consider the quadrature branch:
$$
Q_{\mathrm{raw}}(t) = -A\cos(\omega_{\mathrm{in}}t+\phi_0)\sin(\omega_{\mathrm{LO}}t).  
$$

Using
$$
\cos(a)\sin(b) = \frac{1}{2}\sin(b+a)  
+  
\frac{1}{2}\sin(b-a),  
$$

we get
$$
Q_{\mathrm{raw}}(t) = -\frac{A}{2}  
\sin\left((\omega_{\mathrm{LO}}+\omega_{\mathrm{in}})t+\phi_0\right)  
+  
\frac{A}{2}  
\sin\left((\omega_{\mathrm{in}}-\omega_{\mathrm{LO}})t+\phi_0\right).  
$$

Again, there is a high-frequency sum term and a low-frequency difference term. After low-pass filtering or averaging, the high-frequency terms are suppressed. The remaining baseband components are therefore approximately
$$
I(t)\approx\frac{A}{2}  
\cos\left((\omega_{\mathrm{in}}-\omega_{\mathrm{LO}})t+\phi_0\right),  
$$

and
$$
Q(t)\approx\frac{A}{2}  
\sin\left((\omega_{\mathrm{in}}-\omega_{\mathrm{LO}})t+\phi_0\right).  
$$

The two components form a complex baseband signal

$$  
Z(t) = I(t) + iQ(t).  
$$
Substituting the expressions for (I(t)) and (Q(t)),
$$
Z(t) = \frac{A}{2}  
\left[  
\cos\left((\omega_{\mathrm{in}}-\omega_{\mathrm{LO}})t+\phi_0\right)  
+  
i\sin\left((\omega_{\mathrm{in}}-\omega_{\mathrm{LO}})t+\phi_0\right)  
\right].  
$$

Using Euler’s identity,

$$  
\cos(\theta) + i\sin(\theta) = e^{i\theta},  
$$
we get
$$
Z(t) = \frac{A}{2}  
e^{i\left((\omega_{\mathrm{in}}-\omega_{\mathrm{LO}})t+\phi_0\right)}.  
$$
This is the central result. After quadrature demodulation, the original high-frequency sinusoid becomes a complex phasor rotating at the difference frequency

$$  
f_{\mathrm{err}} = f_{\mathrm{in}} - f_{\mathrm{LO}}.  
$$
In the FPGA, the demodulated signal is integrated over a finite window rather than evaluated instantaneously. For a window of duration $T_{\mathrm{win}}$, the accumulated in-phase and quadrature values are
$$
I_K = \int_{t_k}^{t_k+T_{\mathrm{win}}}  
x(t)\cos(\omega_{\mathrm{LO}}t),dt,  
$$
and
$$
Q_k = -\int_{t_k}^{t_k+T_{\mathrm{win}}}  
x(t)\sin(\omega_{\mathrm{LO}}t),dt.  
$$
In discrete time, these are sums over ADC samples:
$$
I_k = \sum_{n \in k}  
x[n]\cos(\omega_{\mathrm{LO}}nT_s),  
$$

and
$$
Q_k = -\sum_{n \in k}  
x[n]\sin(\omega_{\mathrm{LO}}nT_s),  
$$

where $T_s$ is the ADC sampling period. These sums act as a rectangular-window low-pass filter. The high-frequency image near $f_{\mathrm{in}}+f_{\mathrm{LO}}$ oscillates rapidly and averages toward zero, while the low-frequency difference term remains.
The accumulated I/Q pair defines one complex phasor per measurement window:
$$  
Z_k = I_k + iQ_k.  
$$
For small detuning over the window, this phasor is approximately

$$  
Z_k  
\approx  
C  
e^{i\left(2\pi(f_{\mathrm{in}}-f_{\mathrm{LO}})t_k+\phi_0\right)},  
$$
where $C$ is a real amplitude factor determined by the signal amplitude and the integration window. The important point is that the phase of $Z_k$ advances linearly at the difference frequency.
The phase of the phasor is extracted from the I/Q values as

$$  
\phi_k = \arg(Z_k).  
$$

Since

$$  
Z_k = I_k + iQ_k,  
$$

the phase is the angle of the vector $(I_k,Q_k)$ in the complex plane. Mathematically, this angle is written as 
$$  
\phi_k = \operatorname{atan2}(Q_k,I_k).  
$$
The $\operatorname{atan2}$ function is the quadrant-aware inverse tangent. It returns the unique angle $\phi \in (-\pi,\pi]$ such that
$$  
\cos(\phi) = \frac{I_k}{\sqrt{I_k^2+Q_k^2}},  
$$
and
$$  
\sin(\phi) = \frac{Q_k}{\sqrt{I_k^2+Q_k^2}}.  
$$
$$
\operatorname{atan2}(Q,I) = 
\begin{cases}  
\tan^{-1}\left(\frac{Q}{I}\right), & I>0, \\[6pt]  
\tan^{-1}\left(\frac{Q}{I}\right)+\pi, & I<0,\ Q\ge 0, \\[6pt]  
\tan^{-1}\left(\frac{Q}{I}\right)-\pi, & I<0,\ Q<0, \\[6pt]  
+\frac{\pi}{2}, & I=0,\ Q>0, \\[6pt]  
-\frac{\pi}{2}, & I=0,\ Q<0.  
\end{cases}  
$$
This is why $\operatorname{atan2}$ is used instead of the ordinary inverse tangent. The ordinary expression $\tan^{-1}(Q/I)$ only determines the angle modulo $\pi$, while $\operatorname{atan2}(Q,I)$ gives the correct quadrant over the full circle.
Since the baseband phase is

$$
2\pi(f_{\mathrm{in}}-f_{\mathrm{LO}})t+\phi_0,  
$$

its slope is

$$2\pi(f_{\mathrm{in}}-f_{\mathrm{LO}}).$$
Therefore, the frequency error is obtained from the phase slope: $$
f_{\mathrm{in}}-f_{\mathrm{LO}}
=
\frac{1}{2\pi}  
\frac{d\phi}{dt}.  
$$
For discrete windows, the phase difference between successive phasors is
$$
\operatorname{wrap}_{-\pi,\pi}  
\left(  
\phi_k-\phi_{k-1}  
\right),  
$$
where the wrapping operation maps the phase difference back into the interval $(-\pi,\pi]$. The corresponding frequency error estimate is
$$f_{\text{err},k} = \frac{\Delta \phi_k}{2\pi T_{\mathrm{win}}}.$$

Finally, the measured input frequency is
$$
f_{\mathrm{LO}}  
+  
f_{\mathrm{err},k}.  
$$
The FPGA can also compute the phase difference directly from two consecutive I/Q vectors without separately computing two absolute phases. Let
$$  
Z_k = I_k + iQ_k,  
$$
and
$$  
Z_{k-1} = I_{k-1} + iQ_{k-1}.  
$$
Then the phase difference is the argument of
$$  
Z_k Z_{k-1}^{*} = (I_k+iQ_k)(I_{k-1}-iQ_{k-1}).  
$$
This gives
$$
Z_k Z_{k-1}^{*} = (I_k I_{k-1}+Q_k Q_{k-1})  
+  
i(Q_k I_{k-1}-I_k Q_{k-1}).  
$$
Define
$$  
D_k = I_k I_{k-1}+Q_k Q_{k-1},  
$$
and
$$  
C_k = Q_k I_{k-1}-I_k Q_{k-1}.  
$$

Here $D_k$ is the dot-product term and $C_k$ is the cross-product term. The phase step is then
$$
\Delta \phi_k = \operatorname{atan2}(C_k,D_k).  
$$

This form is useful in FPGA logic because it directly computes the rotation angle between two consecutive I/Q vectors. Once $\Delta \phi_k$ is known, the frequency error is again
$$
f_{\text{err},k} = \frac{\Delta \phi_k}{2\pi T_{\mathrm{win}}}.  
$$
Thus, the complete frequency measurement consists of four mathematical steps: mix the input with a complex local oscillator, average the I/Q components over a window, extract the phasor phase, and convert the phase slope into frequency. The resulting measurement is highly sensitive to small frequency differences because the carrier near $f_{\mathrm{LO}}$ has been translated to a slowly rotating baseband phasor.

|Target|Physical frequency|DDC LO frequency|`PHASE_INC`|
|---|--:|--:|--:|
|10 MHz|10.000000 MHz|10.000000 MHz|`5726623061333`|
|Fundamental|75.662550 MHz|75.662550 MHz|`43329090370929`|
|1st harmonic, 2x|151.325100 MHz|151.325100 MHz|`86658180741857`|
|2nd harmonic, 3x|226.987650 MHz|226.987650 MHz|`129987271112786`|
|3rd harmonic, 4x|302.650200 MHz|188.869800 MHz alias|`108158615226941`|


| $T_{\mathrm{win}}$ | `WINDOW_BEATS` | Update rate | Nyquist BW | Approx. averaging -3 dB BW | Unambiguous $f_{\mathrm{err}}$ range | Uncertainty, optimistic $1/T$ scaling | Uncertainty, white-noise $1/T^{3/2}$ scaling |
| -----------------: | -------------: | ----------: | ---------: | -------------------------: | -----------------------------------: | ------------------------------------: | -------------------------------------------: |
|               1 ms |          61440 |       1 kHz |     500 Hz |                     443 Hz |                              ±500 Hz |                             $1\times$ |                                    $1\times$ |
|             100 µs |           6144 |      10 kHz |      5 kHz |                   4.43 kHz |                               ±5 kHz |                            $10\times$ |                                 $31.6\times$ |
|              50 µs |           3072 |      20 kHz |     10 kHz |                   8.86 kHz |                              ±10 kHz |                            $20\times$ |                                 $89.4\times$ |
|            33.3 µs |           2048 |      30 kHz |     15 kHz |                   13.3 kHz |                              ±15 kHz |                            $30\times$ |                                  $164\times$ |


## PMOD TTL Breakout PCB design

```tikz
\usetikzlibrary{decorations.pathmorphing}

\begin{document}
\begin{tikzpicture}[
    x=1cm,
    y=1cm,
    font=\small,
    wire/.style={line width=0.8pt},
    block/.style={
        draw,
        line width=0.8pt,
        minimum width=2.8cm,
        minimum height=1.5cm,
        align=center
    },
    rblock/.style={
        draw,
        line width=0.8pt,
        minimum width=2.4cm,
        minimum height=1.4cm,
        align=center
    },
    resistor/.style={
        line width=0.8pt,
        decorate,
        decoration={zigzag, segment length=5pt, amplitude=2pt}
    }
]

% Ground helper
\newcommand{\ground}[2]{
    \draw[line width=0.8pt] (#1,#2) -- (#1,#2-0.12);
    \draw[line width=0.8pt] (#1-0.18,#2-0.12) -- (#1+0.18,#2-0.12);
    \draw[line width=0.8pt] (#1-0.12,#2-0.22) -- (#1+0.12,#2-0.22);
    \draw[line width=0.8pt] (#1-0.06,#2-0.32) -- (#1+0.06,#2-0.32);
}

% ---------------------------------------------------------
% Region boxes
% ---------------------------------------------------------
\draw[dashed, rounded corners, line width=0.7pt]
    (-2.0,-2.3) rectangle (7.85,3.55);

\node[anchor=north west]
    at (-0.30,3.40)
    {\textbf{External trigger-conditioning board}};

\draw[dashed, rounded corners, line width=0.7pt]
    (8.05,-2.3) rectangle (17.5,3.55);

\node[anchor=north west]
    at (8.25,3.40)
    {\textbf{RFSoC 4x2 onboard circuitry}};

% ---------------------------------------------------------
% External trigger
% ---------------------------------------------------------
\node[anchor=east] at (0,0) {5-V trigger};

\draw[wire] (0,0) -- (1.35,0);

\filldraw[fill=white, line width=0.8pt]
    (0,0) circle (2pt);

\coordinate (Vin) at (1.35,0);

\node[below] at (0.72,-0.12)
    {$0/5~\mathrm{V}$};

% ---------------------------------------------------------
% Input pull-down
% ---------------------------------------------------------
\draw[resistor] (Vin) -- (1.35,-1.55);

\node[right] at (1.62,-1.05)
    {$R_{\mathrm{PD}}=10~\mathrm{k}\Omega$};

\ground{1.35}{-1.55}

% ---------------------------------------------------------
% SN74LVC1G17 buffer
% ---------------------------------------------------------
\node[block] (U1) at (4.15,0) {
    SN74LVC1G17\\
    Schmitt buffer
};

\draw[wire] (Vin) -- (U1.west);

\node[above] at (2.55,0.16) {$A$};

% Buffer ground
\draw[wire] (4.15,-0.75) -- (4.15,-1.55);
\ground{4.15}{-1.55}

% ---------------------------------------------------------
% 3.3-V supply
% ---------------------------------------------------------
\node at (4.15,2.55)
    {$+3.3~\mathrm{V}$};

\draw[wire]
    (4.15,2.35) -- (4.15,0.75);

% ---------------------------------------------------------
% Decoupling capacitor
% ---------------------------------------------------------
\draw[wire]
    (4.15,1.85) -- (5.35,1.85);

\draw[wire]
    (5.35,1.85) -- (5.35,1.25);

% capacitor plates
\draw[line width=0.8pt]
    (5.05,1.25) -- (5.65,1.25);

\draw[line width=0.8pt]
    (5.05,1.08) -- (5.65,1.08);

\draw[wire]
    (5.35,1.08) -- (5.35,-1.55);

\node[anchor=west] at (5.55,1.45)
    {$C_{\mathrm{dec}}=100~\mathrm{nF}$};

\ground{5.35}{-1.55}

% ---------------------------------------------------------
% Buffer output
% ---------------------------------------------------------
\node[above right]
    at ([xshift=0.03cm,yshift=0.12cm]U1.east)
    {$Y$};

\draw[wire]
    (U1.east) -- (5.95,0);

% Series resistor
\draw[resistor]
    (5.95,0) -- (7.15,0);

\node[below] at (6.55,-0.42)
    {$R_s=100~\Omega$};

\draw[wire]
    (7.15,0) -- (8.45,0);

% ---------------------------------------------------------
% PMOD node
% ---------------------------------------------------------
\coordinate (Pmod) at (8.45,0);

\fill (Pmod) circle (2pt);

\node[below] at (8.75,-0.28)
    {PMOD};

\node[below] at (7.15,-0.78)
    {$0/3.3~\mathrm{V}$};

% ---------------------------------------------------------
% RFSoC onboard pull-up
% ---------------------------------------------------------
\draw[wire]
    (Pmod) -- (8.45,0.95);

\draw[resistor]
    (8.45,0.95) -- (8.45,2.15);

\node[anchor=west] at (8.85,1.55)
    {$R_{\mathrm{PU}}=1.15~\mathrm{k}\Omega$};

\node at (8.65,2.55)
    {$+3.3~\mathrm{V}$};

% ---------------------------------------------------------
% NVT2008 translator
% ---------------------------------------------------------
\node[rblock] (NVT) at (11.2,0) {
    NVT2008\\
    translator
};

\draw[wire]
    (Pmod) -- (NVT.west);

% ---------------------------------------------------------
% FPGA input
% ---------------------------------------------------------
\draw[wire]
    (NVT.east) -- (13.9,0);

\node[anchor=west, align=left]
    at (14.0,0.10)
    {RFSoC FPGA input\\
    $(1.8~\mathrm{V}$ domain)};

\node[below] at (13.25,-0.24)
    {$0/1.8~\mathrm{V}$};

\end{tikzpicture}
\end{document}
```


The external trigger is a nominal 0 to 5V logic signal. Each trigger is applied to the input of an SN74LVC1G17 non-inverting Schmitt-trigger buffer. The buffer is powered from the 3.3V supply available at the RFSoC PMOD connector. Although the device operates from a 3.3V supply in this application, its input is over-voltage tolerant to 5.5V, allowing a 5V external logic signal to be accepted without exposing the PMOD interface directly to the 5V level ([SN74LVC1G17 Single Schmitt-Trigger Buffer](https://www.ti.com/lit/ds/symlink/sn74lvc1g17.pdf?ts=1785959446302&ref_url=https%253A%252F%252Fwww.ti.com%252Fproduct%252FSN74LVC1G17)). Since the output logic level of the buffer is determined by its 3.3V supply, a logical high at the external input produces an approximately 3.3V output, and  a logical low produces an output close to ground.

![[Pasted image 20260812021148.png]]
