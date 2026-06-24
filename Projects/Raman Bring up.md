# Raman bring up
This page will contain the details and status about the Raman bring up project. This project naturally divides into three parts:
1. **Theory** - We have scripts for solving the Hyperfine Hamiltonian any level in Ba137. From that we will have an estimate for energy levels and magnetic field sensitivities. What we need to work on is relative transition strengths for Raman transitions. 
2. **Laser bring up** - We have a [[Coherent Paladin Laser.pdf]] we will need to understand how to control the frequency and phase of the laser. And Bring up RFSoC 4x2 to do so. As well as understand how Repetition rate stabilization works. 
3. **Optical path** - We will need to work on optical path from laser to to ion position and alignment strategies.  

With these objectives under control we will be in good shape to work with Raman transitions. 

### Goals

1. For the first goal to see rabi oscillations and with Raman transition this is the experiment we will be targeting. 
```tikz

```
```tikz
```
```tikz
\begin{document}
\begin{tikzpicture}[
    x=1cm,
    y=1cm,
    every node/.style={font=\large},
    state/.style={line width=2.2pt, line cap=butt}
]

% mF sublevels
\foreach \x/\mf in {0/-2, 1.1/-1, 2.2/0, 3.3/+1, 4.4/+2} {
    \draw[state] (\x-0.25,0) -- (\x+0.25,0);
    %\node[below] at (\x,-0.18) {$\mf$};
}
% left labels
\node[left] at (-0.65,0) {$F=2$};
%\node[left] at (-0.65,-0.43) {$m_F=$};

% manifold label
\node[right] at (4.95,0.02) {$6S_{1/2}$};

\draw[state] (1.75,4.2) -- (2.65,4.2);  
\node[right] at (2.75,4.2) {$6P_{1/2}$};
% 5D5/2 level on the right, midway between 6S1/2 and 6P1/2
\draw[state] (6.2,3.1) -- (7.1,3.1);
\node[right] at (7.2,3.1) {$5D_{5/2}$};

% 1762 nm shelving transition from |0> to 5D5/2
\draw[<->, line width=1.5pt, gray] (4.4,0.08) -- (6.65,3.02)
    node[midway, above, sloped, black] {$1762\,\mathrm{nm}$};

% dashed virtual level below 6P1/2
\draw[state, dashed] (1.75,3.5) -- (2.65,3.5);

% detuning Delta between virtual level and 6P1/2
\draw[<->] (1.45,3.5) -- (1.45,4.2)
    node[midway, left] {$\Delta$};

% 532 nm Raman legs to the virtual level
\draw[<->, line width=1.5pt, green!60!black] (2.2,0.08) -- (2.08,3.48);
\draw[<->, line width=1.5pt, green!60!black] (4.4,0.08) -- (2.32,3.48);

% label for the Raman beams
\node[green!60!black] at (1.35,1.9) {$532\,\mathrm{nm}$};

% qubit labels
\node[above] at (2.2,-0.58) {$|1\rangle$};   % above mF = 0
\node[above] at (4.4,-0.58) {$|0\rangle$};   % above mF = +2

\end{tikzpicture}
\end{document}
```



2. Ones we have that under control, we can start calibration rabi frequencies and hyperfine frequencies. 
3. And finally have gate sets and algorithms with Raman transitions. 
## Theory
### Effective two level Hamiltonian 
Consider a three level system: 
```tikz
\begin{document}
\begin{tikzpicture}[
    x=1cm,
    y=1cm,
    every node/.style={font=\Large},
    state/.style={line width=2.2pt, line cap=butt}
]

% lower state |1>
\draw[state] (-0.9,0) -- (0.3,0);
\node[left] at (-0.9,0) {$|a\rangle$};

% higher state |b>
\draw[state] (0.9,0.7) -- (2.1,0.7);
\node[right] at (2.1,0.7) {$|b\rangle$};

% far-detuned excited state |e> shown dashed
\draw[state] (-0.9,3.9) -- (2.1,3.9);
\node[right] at (2.1,3.9) {$|e\rangle$};

% far-detuned excited state |e> shown dashed
\draw[state, dashed] (-0.3,3.2) -- (1.5,3.2);
%\node[right] at (2.1,3.2) {$|e\rangle$};

% Raman legs
\draw[<->,very thick, green!60!black] (-0.3,0.1) -- (0.6,3.12)
	node[midway, left] {$\omega_A$};
\draw[<->,very thick, green!60!black] (1.5,0.8) -- (0.6,3.12)
	node[midway,right] {$\omega_B$};

% energy spacing between |a> and |b>
\draw[|-|] (0.6,0) -- (0.6,0.7)
    node[midway, right] {$\omega_q$};

% energy spacing between |e> and |>
\draw[|-|] (0.6,3.3) -- (0.6,3.8)
    node[midway, right] {$\Delta$};

% energy spacing between |a> and |e>
\draw[|-|] (-0.7,0.1) -- (-0.7,3.8)
    node[midway, left] {$\omega_e$};

% energy spacing between |b> and |e>
\draw[|-|] (1.9,0.8) -- (1.9,3.8)
    node[midway, right] {$\omega_b$};

\end{tikzpicture}
\end{document}
```
here, $E_a = 0$, $E_b = \hbar\omega_q$, and $E_e = \hbar \omega_e$. 

And the Hamiltonian of this system is given by:
$$H_\text{ion} = 0\ket{a}\bra{a} + \hbar\omega_q\ket{b}\bra{b} + \hbar\omega_e\ket{e}\bra{e} \tag{1.2.1} $$
Now Consider incoming laser fields:
$$H_\text{Laser}

=

\hbar G_A

\cos(\omega_A t-k_A x+\phi_A)

\left(

|e\rangle\langle a|+|a\rangle\langle e|

\right) + \hbar G_B

\cos(\omega_B t-k_B x+\phi_B)

\left(

|e\rangle\langle b|+|b\rangle\langle e|

\right). \tag{1.2.2} $$
This incoming laser field can also be written as:
$$H_\text{Laser}
=
\hbar g_A
\left[
e^{i(\omega_A t-k_A x+\phi_A)}
+
e^{-i(\omega_A t-k_A x+\phi_A)}
\right]
\left(
|e\rangle\langle a|+|a\rangle\langle e|
\right) +
\hbar g_B
\left[
e^{i(\omega_B t-k_B x+\phi_B)}
+
e^{-i(\omega_B t-k_B x+\phi_B)}
\right]
\left(
|e\rangle\langle b|+|b\rangle\langle e|
\right) \tag{1.2.3} $$
Total Hamiltonian of the system being:
$$H_\text{Total} = H_\text{ion} + H_\text{laser} \tag{1.2.4} $$

Now we move to interaction picture with reference frame of $H_0 = \hbar\omega_q\ket{b}\bra{b} + \hbar\omega_A\ket{e}\bra{e}$
$$U_0(t)=e^{-iH_0t/\hbar}. \tag{1.2.5} $$
$$U_0(t)=\ket{a}\bra{a}+e^{-i\omega_qt}\ket{b}\bra{b}+e^{-i\omega_At}\ket{e}\bra{e}. \tag{1.2.6} $$
So the interaction Hamiltonian is:

$$H_\text{int} = U_0^\dagger(H_\text{Total} - H_\text{0})U_0 \tag{1.2.7} $$
We will simplify this in two parts:
First the ion Hamiltonian:
$$H_\text{ion}^{\text{int}} = U_0^\dagger(H_\text{ion} - H_\text{0})U_0 = U_0^\dagger(\hbar(\omega_e - \omega_A)\ket{e}\bra{e})U_0 = \hbar \Delta \ket{e}\bra{e} \tag{1.2.8} $$
Here we define $\Delta = \omega_e - \omega_A$. And Laser Hamiltonian in interaction picture:
$$H_\text{laser}^{\text{int}}
=
U_0^\dagger H_L U_0. \tag{1.2.9} $$
in this equation:
$$U_0^\dagger |e\rangle\langle a| U_0
=
e^{i\omega_A t}|e\rangle\langle a|, \tag{1.2.10} $$
$$U_0^\dagger |a\rangle\langle e| U_0
=
e^{-i\omega_A t}|a\rangle\langle e|, \tag{1.2.11} $$
$$U_0^\dagger |e\rangle\langle b| U_0
=
e^{i(\omega_A-\omega_q)t}|e\rangle\langle b|, \tag{1.2.12} $$
$$U_0^\dagger |b\rangle\langle e| U_0
=
e^{-i(\omega_A-\omega_q)t}|b\rangle\langle e|. \tag{1.2.13} $$
Now if we expand the interaction Hamiltonian, there will be some fast oscillating terms, here we can use rotating wave approximation to ignore those terms. For instance the term $\ket{e}\bra{a}$ becomes 
$$e^{-i(\omega_A t-k_A x+\phi_A)}|e\rangle\langle a| \longrightarrow e^{-i(\omega_A t-k_A x+\phi_A)}
e^{i\omega_A t} |e\rangle\langle a|. \tag{1.2.14} $$

$$e^{-i(\omega_A t-k_A x+\phi_A)}
e^{i\omega_A t}
=
e^{i k_A x}
e^{-i\phi_A}. \tag{1.2.15} $$
If the laser is close to the optical transition this is slowly rotating term and we can keep it. But the counter rotating term:
$$e^{i(\omega_A t-k_A x+\phi_A)}|e\rangle\langle a| \longrightarrow e^{i(\omega_A t-k_A x+\phi_A)}
e^{i\omega_A t}
|e\rangle\langle a| = e^{i(\omega_A+\omega_A)t}
e^{-i k_A x}
e^{i\phi_A} \ket{e}\bra{a}. \tag{1.2.16} $$
oscillates fast and can be dropped using RWA. 
So surviving term for laser arm $A$ are $\hbar g_a \left[e^{i k_a x-i\phi_a} |e\rangle\langle a| + e^{-i k_a x+i\phi_a} |a\rangle\langle e| \right].$
And similarly for laser arm $B$, the term $\ket{e}\bra{b}$ becomes
$$e^{-i(\omega_B t-k_B x+\phi_B)}|e\rangle\langle b| \longrightarrow e^{-i(\omega_B t-k_B x+\phi_B)}
e^{i(\omega_A-\omega_q)t}
|e\rangle\langle b|. \tag{1.2.17} $$
where we define 
$$
\begin{equation}
\delta = \omega_A-\omega_q-\omega_B.
\end{equation} \tag{1.2.18}
$$
So in this case the surviving terms are:
$$\hbar g_B
\left[
e^{i\delta t}
e^{i k_B x-i\phi_B}
|e\rangle\langle b|
+
e^{-i\delta t}
e^{-i k_B x+i\phi_B}
|b\rangle\langle e|
\right]. \tag{1.2.19} $$
$\therefore$ the interaction Laser Hamiltonian after RWA is:
$$H_\text{Laser}^{\text{int}}
=
\hbar g_A \left[e^{i k_A x-i\phi_A} |e\rangle\langle a| + e^{-i k_A x+i\phi_A} |a\rangle\langle e| \right] +
\hbar g_B \left[ e^{i\delta t} e^{i k_B x-i\phi_B} |e\rangle\langle b| + e^{-i\delta t} e^{-i k_B x+i\phi_B} |b\rangle\langle e|\right] \tag{1.2.20} $$
So total Hamiltonian in interaction picture is:
$$H_\text{int} = \hbar g_A \left[e^{i k_A x-i\phi_A} |e\rangle\langle a| + e^{-i k_A x+i\phi_A} |a\rangle\langle e| \right] +
\hbar g_B
\left[
e^{i\delta t}
e^{i k_B x-i\phi_B}
|e\rangle\langle b|
+
e^{-i\delta t}
e^{-i k_B x+i\phi_B}
|b\rangle\langle e|
\right] + \hbar \Delta \ket{e}\bra{e} \tag{1.2.21} $$
In matrix form this can be written as:

$$H_{\text{int}}
=
\hbar
\begin{pmatrix}
0 & 0 & g_A e^{-i(k_A x - \phi_A)}\\
0 & 0 & g_B e^{-i(\delta t + k_B x - \phi_B)}\\
g_A e^{i(k_A x-\phi_A)} & g_B e^{i(\delta t + k_B x-\phi_B)} & \Delta
\end{pmatrix}. \tag{1.2.22}
$$
Now putting this equation in Schrodinger equation:

$$i\hbar \frac{d}{dt}|\psi(t)\rangle=H_{\text{int}}|\psi(t)\rangle. \tag{1.2.23} $$
here we can define $\ket{\psi(t)} = C_a(t)\ket{a}+C_b(t)\ket{b}+C_e(t)\ket{e}$. 
Or in matrix form:
$$|\psi(t)\rangle
=
\begin{pmatrix}
C_a(t)\\
C_b(t)\\
C_e(t)
\end{pmatrix}. \tag{1.2.24} $$
So, 
$$i\hbar
\frac{d}{dt}
\begin{pmatrix}
C_a(t)\\
C_b(t)\\
C_e(t)
\end{pmatrix}
=
\hbar
\begin{pmatrix}
0 & 0 & g_A e^{-i(k_A x - \phi_A)}\\
0 & 0 & g_B e^{-i(\delta t + k_B x - \phi_B)}\\
g_A e^{i(k_A x-\phi_A)} & g_B e^{i(\delta t + k_B x-\phi_B)} & \Delta
\end{pmatrix}
\begin{pmatrix}
C_a(t)\\
C_b(t)\\
C_e(t)
\end{pmatrix}. \tag{1.2.25} $$
From this we get three coupled equations:
$$i\dot C_a
=
g_A e^{-i(k_Ax-\phi_A)}C_e, \tag{1.2.26} $$
$$i\dot C_b
=
g_B e^{-i(\delta t+k_Bx-\phi_B)}C_e \tag{1.2.27} $$
$$
i\dot C_e
=
g_A e^{i(k_Ax-\phi_A)}C_a
+
g_B e^{i(\delta t+k_Bx-\phi_B)}C_b
+
\Delta C_e. \tag{1.2.28}
$$
We know that detuning from the excited state is much larger than the rabi-frequency of transition. i.e $\Delta \gg g_A, g_B$ so we set the $\dot{C}_e \approx 0$. 
So for the third coupled equation $0 = g_A e^{i(k_Ax-\phi_A)}C_a+g_B e^{i(\delta t+k_Bx-\phi_B)}C_b+\Delta C_e$. So
$$C_e=-\frac{1}{\Delta}\left[g_A e^{i(k_Ax-\phi_A)}C_a+g_B e^{i(\delta t+k_Bx-\phi_B)}C_b\right] \tag{1.2.29} $$
We then substitute this equation back into the $C_a$ and $C_b$ equation to get the **effective two level Raman Hamiltonian:**
$$i\dot C_a
=
g_A e^{-i(k_Ax-\phi_A)}
\left[
-\frac{1}{\Delta}
\left(
g_A e^{i(k_Ax-\phi_A)}C_a
+
g_B e^{i(\delta t+k_Bx-\phi_B)}C_b
\right)
\right] \tag{1.2.30} $$
So, 
$$i\dot C_a
=
-\frac{g_A^2}{\Delta}C_a
-
\frac{g_Ag_B}{\Delta}
e^{i[\delta t+(k_B-k_A)x+(\phi_A-\phi_B)]}
C_b, \tag{1.2.31} $$
$$i\dot C_b=-\frac{g_Ag_B}{\Delta}e^{-i[\delta t+(k_B-k_A)x+(\phi_A-\phi_B)]}C_a-\frac{g_B^2}{\Delta}C_b. \tag{1.2.32} $$
So in the matrix form, 
$$
i\frac{d}{dt}
\begin{pmatrix}
C_a\\
C_b
\end{pmatrix}
=
-\begin{pmatrix}
\dfrac{g_A^2}{\Delta} &
\dfrac{g_Ag_B}{\Delta}
e^{i[\delta t+(k_B-k_A)x+(\phi_A-\phi_B)]}\\
\dfrac{g_Ag_B}{\Delta}
e^{-i[\delta t+(k_B-k_A)x+(\phi_A-\phi_B)]}&
\dfrac{g_B^2}{\Delta}
\end{pmatrix}
\begin{pmatrix}
C_a\\
C_b
\end{pmatrix}. \tag{1.2.33}
$$
The effective Raman Hamiltonian is:
$$H_{\text{eff}} = -\hbar
\begin{pmatrix}
\dfrac{g_A^2}{\Delta} &
\dfrac{g_Ag_B}{\Delta}
e^{i[\delta t+(k_B-k_A)x+(\phi_A-\phi_B)]}\\
\dfrac{g_Ag_B}{\Delta}
e^{-i[\delta t+(k_B-k_A)x+(\phi_A-\phi_B)]}&
\dfrac{g_B^2}{\Delta}
\end{pmatrix} \tag{1.2.34}
$$
The diagonal terms are the AC stark shifts of $\ket{a}$ and $\ket{b}$
$$-{g_A^2\over \Delta},\, -{g_B^2\over \Delta}. \tag{1.2.35} $$
And the off-diagonal terms are the Raman coupling terms between $\ket{a}$ and $\ket{b}.$ 

## Repetition Rate Math
```tikz
\begin{document}
\begin{tikzpicture}[
    x=1cm,
    y=1cm,
    every node/.style={font=\Large},
    state/.style={line width=2.2pt, line cap=butt}
]

% lower state |1>
\draw[state] (-0.9,0) -- (0.3,0);
\node[left] at (-0.9,0) {$|a\rangle$};

% higher state |b>
\draw[state] (0.9,0.7) -- (2.1,0.7);
\node[right] at (2.1,0.7) {$|b\rangle$};

% far-detuned excited state |e> shown dashed
\draw[state] (-0.9,3.9) -- (2.1,3.9);
\node[right] at (2.1,3.9) {$|e\rangle$};

% far-detuned excited state |e> shown dashed
\draw[state, dashed] (-0.3,3.2) -- (1.5,3.2);
%\node[right] at (2.1,3.2) {$|e\rangle$};

% Raman legs
\draw[<->,very thick, green!60!black] (-0.3,0.1) -- (0.6,3.12)
	node[midway, left] {$\omega_A$};
\draw[<->,very thick, green!60!black] (1.5,0.8) -- (0.6,3.12)
	node[midway,right] {$\omega_B$};

% energy spacing between |a> and |b>
\draw[|-|] (0.6,0) -- (0.6,0.7)
    node[midway, right] {$\omega_q$};

% energy spacing between |e> and |>
\draw[|-|] (0.6,3.3) -- (0.6,3.8)
    node[midway, right] {$\Delta$};

% energy spacing between |a> and |e>
\draw[|-|] (-0.7,0.1) -- (-0.7,3.8)
    node[midway, left] {$\omega_e$};

% energy spacing between |b> and |e>
\draw[|-|] (1.9,0.8) -- (1.9,3.8)
    node[midway, right] {$\omega_e - \omega_q$};

\end{tikzpicture}
\end{document}
```
For a single frequency CW Raman system we just need:
$$\omega_A - \omega_B = \omega_q \tag{1.3.1} $$
For a mode-locked laser, however, each Raman arm contains a whole comb of teeth spaced by repetition rate. So $k^{\text{th}}$ comb-tooth is at frequency:
$$\omega_k = \omega_{\text{CEO}} + k\cdot\omega_{\text{rep}} \tag{1.3.2} $$
where, CEO stands for carrier envelope offset (this is frequency offset imparted on entire frequency comb due to fast oscillation of carrier @ THz frequency). So if we shift arms A and B with AOMs, we get teeth as 
$$
\begin{align}
\omega_{A,k} &= \omega_{\text{CEO}}+k\cdot\omega_{\text{rep}} + \omega_1 \\
\omega_{B,l} &= \omega_{\text{CEO}}+l\cdot\omega_{\text{rep}} + \omega_2
\end{align} \tag{1.3.3}
$$
where $\omega_1$ and $\omega_2$ are AOM shifts on ARM A and B respectively.  Taking the difference gives us:
$$\omega_{A,k} - \omega_{B,l} = (k-l)\cdot\omega_{\text{rep}} + (\omega_1 - \omega_2). \tag{1.3.4} $$
There are many comb-tooth pairs that satisfy the same Raman difference frequency, they all contribute to the Raman coupling.

