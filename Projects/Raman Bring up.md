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

## Relative Transition Strengths Calculations

We consider Raman transitions between hyperfine--Zeeman states in the $6S_{1/2}$ ground-state manifold of $^{137}\mathrm{Ba}^{+}$. The Raman coupling is mediated by the excited-state manifolds $6P_{1/2}$ and $6P_{3/2}$.
The initial and final ground states are denoted by $\lvert i\rangle$ and $\lvert f\rangle$, respectively. The intermediate excited states are denoted by $\lvert e\rangle$.
### Overview

#### Choice of basis 
The nuclear spin of $^{137}\mathrm{Ba}^{+}$ is
$$
\begin{equation}
I=\frac{3}{2}.
\end{equation} \tag{1.4.1}
$$
For a particular electronic fine-structure manifold with angular momentum $J$, there are two useful angular-momentum bases.

##### Uncoupled Basis
The uncoupled basis is
$$
\begin{equation}
\mathcal{B}_{\mathrm{u}}
=
\left\{
\lvert I,m_I;J,m_J\rangle
\right\},
\end{equation} \tag{1.4.2}
$$
where
$$
\begin{equation}
m_I=-I,-I+1,\ldots,I
\end{equation} \tag{1.4.3}
$$
and
$$
\begin{equation}
m_J=-J,-J+1,\ldots,J.
\end{equation} \tag{1.4.4}
$$
In this basis, the nuclear and electronic angular momenta have separately defined projections along the quantization axis. The dimension of the Hilbert space is
$$
\begin{equation}
N=(2I+1)(2J+1).
\end{equation} \tag{1.4.5}
$$
The uncoupled basis is particularly convenient for constructing the Zeeman Hamiltonian because the operators $I_z$ and $J_z$ are diagonal in this basis. It is also convenient for calculating electric-dipole matrix elements, since the electric-dipole operator acts on the electronic coordinates and leaves the nuclear spin projection unchanged.

##### Coupled Basis
The coupled basis is
$$
\begin{equation}
\mathcal{B}_{\mathrm{c}}
=
\left\{
\lvert F,m_F\rangle
\right\},
\end{equation} \tag{1.4.6}
$$
where the total angular momentum is
$$
\begin{equation}
F = I + J,
\end{equation} \tag{1.4.7}
$$
and
$$
\begin{equation}
F=|I-J|,|I-J|+1,\ldots,I+J.
\end{equation} \tag{1.4.8}
$$
The projection quantum number is
$$
\begin{equation}
m_F=-F,-F+1,\ldots,F.
\end{equation} \tag{1.4.9}
$$
At intermediate magnetic field, the hyperfine Hamiltonian is diagonal in this coupled basis, and $F$ and $m_F$ are good quantum numbers.

##### Changing between coupled and uncoupled basis
The coupled and uncoupled bases are related by Clebsch—Gordan coefficients. A coupled state is expanded in the uncoupled basis as
$$
\begin{equation}
\lvert F,m_F\rangle
=
\sum_{m_I,m_J}
\langle I,m_I;J,m_J|F,m_F\rangle
\lvert I,m_I;J,m_J\rangle
.
\end{equation} \tag{1.4.10}
$$
Only terms satisfying
$$
\begin{equation}
m_F=m_I+m_J
\end{equation} \tag{1.4.11}
$$
are nonzero. The inverse transformation is
$$
\begin{equation}
    \lvert I,m_I;J,m_J\rangle
    =
    \sum_{F,m_F}
    \langle I,m_I;J,m_J|F,m_F\rangle^{*}
    \lvert F,m_F\rangle
    .
\end{equation}
$$

#### Polarization in the spherical basis

<<<<<<< HEAD
Let the quantization axis be in $+ \hat{z}$, and the laser propagation direction is assumed to lie in $x-z$ plane and can be written as:

$$\hat{k} = \left( \sin\theta_k, 0, \cos \theta_k \right),$$
where $\theta_k$ is angle between the beam and the quantization axis. The two orthogonal vectors perpendicular to $\hat{k}$ are chosen as:

$$\hat{e}_a = \left( \cos\theta_k, 0, -\sin \theta_k \right),$$
$$\hat{e}_b = (0,1,0).$$
We will define the polarization ellipse by an orientation angle $\psi$ and ellipticity angle $\chi$, the jones vector components in $(\hat{e}_a, \hat{e}_b)$ basis are:
$$\epsilon_a = \cos\psi \cos\chi - i \sin\psi \sin\chi,$$
$$\epsilon_b = \sin\psi\cos\chi + i \cos \psi \sin \chi.$$
The Cartesian polarization vector is therefore

$$\hat{\epsilon} = \epsilon_a \hat{e}_a + \epsilon_b \hat{e}_b.$$
The Cartesian polarization is converted to mathematical spherical components using

$$
\begin{align}
\sigma_+&: \epsilon_{+1} &= -{\epsilon_x + i \epsilon_y \over \sqrt{2}},\\
\pi&:& \epsilon_{0} = \epsilon_z,\\
\sigma_-&: \epsilon_{-1} &= {\epsilon_x - i \epsilon_y \over \sqrt{2}}.
\end{align}
$$
$$
\boxed{
\begin{aligned}
\epsilon_{+1}
&=
-\frac{
\cos\psi
\left(
\cos\theta_k\cos\chi-\sin\chi
\right)
+
i\sin\psi
\left(
\cos\chi-\cos\theta_k\sin\chi
\right)
}{
\sqrt2
},\\
\epsilon_0
&=
-\sin\theta_k
\left(
\cos\psi\cos\chi
-i\sin\psi\sin\chi
\right),\\
\epsilon_{-1}
&=
\frac{
\cos\psi
\left(
\cos\theta_k\cos\chi+\sin\chi
\right)
-
i\sin\psi
\left(
\cos\chi+\cos\theta_k\sin\chi
\right)
}{
\sqrt2
}.

\end{aligned}

}

$$
Let the external magnetic field define the quantization axis,

$$
\hat{\mathbf z}\parallel\mathbf B.  
$$

The propagation direction of a laser beam is written as $\hat{\mathbf k}$. In the present geometry, the beam is restricted to the (xz)-plane, such that

$$
\hat{\mathbf k}

\sin\theta_k,\hat{\mathbf x}  
+  
\cos\theta_k,\hat{\mathbf z},  
$$

where $\theta_k$ is the angle between the beam and the quantization axis. A convenient orthonormal basis for the plane transverse to $\hat{\mathbf k}$ is

$$ 
\hat{\boldsymbol\theta}
=
\cos\theta_k,\hat{\mathbf x}
-
\sin\theta_k,\hat{\mathbf z},  
$$

and

$$
\hat{\boldsymbol\phi}
=
\hat{\mathbf y}.  
$$
These vectors satisfy

$$
\hat{\boldsymbol\theta}\cdot\hat{\mathbf k}

= 
\hat{\boldsymbol\phi}\cdot\hat{\mathbf k}
=
0,  
\qquad  
\hat{\boldsymbol\theta}\cdot\hat{\boldsymbol\phi}=0.  
$$

They correspond to the linear-polarization basis of the beam, or helicity, frame used in Ref. [Campbell], where $\hat{\mathbf e}_{x'}=\hat{\boldsymbol\theta}$ and $\hat{\mathbf e}_{y'}=\hat{\boldsymbol\phi}$. ([arXiv](https://arxiv.org/pdf/2510.07451 "Angular Geometry of Atomic Multipole Transitions"))

The positive-frequency component of the electric field is written as

$$
\mathbf E^{(+)}(\mathbf r,t)

\frac{E_0}{2}  
\hat{\boldsymbol\epsilon},  
e^{i(\mathbf k\cdot\mathbf r-\omega t+\varphi_L)},  
$$

where $E_0$ is the field amplitude, $\varphi_L$ is the optical phase, and $\hat{\boldsymbol\epsilon}$ is a normalized complex polarization vector satisfying

$$
\hat{\boldsymbol\epsilon}^{*}  
\cdot  
\hat{\boldsymbol\epsilon}  
=1,  
\qquad  
\hat{\mathbf k}\cdot\hat{\boldsymbol\epsilon}=0.  
$$

Because the field is transverse, the polarization vector has only two independent components and may be expressed as

$$
\hat{\boldsymbol\epsilon}
=
\epsilon_\theta\hat{\boldsymbol\theta}  
+  
\epsilon_\phi\hat{\boldsymbol\phi}.  
$$

This is the Jones-vector representation in the beam frame,

$$
\mathbf J
=
\begin{pmatrix}  
	\epsilon_\theta \\
	\epsilon_\phi  
\end{pmatrix}.  
$$

The use of the transverse beam-frame components as a Jones vector follows the convention adopted in Ref. [Campbell]. ([arXiv](https://arxiv.org/pdf/2510.07451 "Angular Geometry of Atomic Multipole Transitions"))
An arbitrary pure polarization state can be parameterized by two angles:

- the polarization angle $\psi$, which gives the orientation of the major axis of the polarization ellipse;
- the ellipticity angle $\chi$, which gives the ratio of the minor and major axes and the handedness of the polarization.

Define the orthogonal ellipse-axis vectors

$$
\hat{\mathbf a}
=
\cos\psi\,\hat{\boldsymbol\theta}  
+  
\sin\psi\,\hat{\boldsymbol\phi},  
$$

and

$$
\hat{\mathbf b}
=
-\sin\psi\,\hat{\boldsymbol\theta}  
+  
\cos\psi\,\hat{\boldsymbol\phi}.  
$$

Here, $\hat{\mathbf a}$ points along the major axis and $\hat{\mathbf b}$ points along the minor axis. The normalized complex polarization vector is then defined as

$$ 
\boxed{  
\hat{\boldsymbol\epsilon}
=
\cos\chi\,\hat{\mathbf a}  
+  
i\sin\chi\,\hat{\mathbf b}  
}  
$$

with

$$
-\frac{\pi}{4}  
\leq  
\chi  
\leq  
\frac{\pi}{4}.  
$$

At a fixed position, and after absorbing the optical phase into the definition of time, the real electric field becomes

$$
\frac{\mathbf E(t)}{E_0}
=
\cos\chi\,  
\hat{\mathbf a}\,\cos\omega t  
+  
\sin\chi\,  
\hat{\mathbf b}\,\sin\omega t.  
$$

Equation (11) explicitly describes an ellipse with semiaxes

$$
a=\cos\chi,  
\qquad  
b=|\sin\chi|.  
$$

Consequently,

$$
\frac{b}{a}
=
|\tan\chi|.  
$$

The important limiting cases are
$$
\chi=0  
\quad\Longrightarrow\quad  
\text{linear polarization},  
$$

and
$$
|\chi|=\frac{\pi}{4}  
\quad\Longrightarrow\quad  
\text{circular polarization}.  
$$

For intermediate values of $\chi$, the polarization is elliptical. The sign of $\chi$ determines the sense of rotation of the electric field under the phase convention in Eq. (4).

Substituting Eqs. (8) and (9) into Eq. (10) gives the Jones components
$$
\boxed{  
\epsilon_\theta

= \cos\psi\cos\chi

i\sin\psi\sin\chi  
}  
$$

and

$$
\boxed{  
\epsilon_\phi
=
\sin\psi\cos\chi  
+  
i\cos\psi\sin\chi.  
}  
$$

The Jones vector is therefore

$$
\boxed{  
\mathbf J(\psi,\chi)
=
\begin{pmatrix}  
\cos\psi\cos\chi-i\sin\psi\sin\chi \\  
\sin\psi\cos\chi+i\cos\psi\sin\chi  
\end{pmatrix}.  
}  
$$

It is straightforward to verify that

$$
|\epsilon_\theta|^2+|\epsilon_\phi|^2=1.  
$$

A general Jones vector contains two complex numbers and therefore initially contains four real parameters. However, normalization removes one real degree of freedom, while a common phase multiplying the entire vector,

$$
\mathbf J\rightarrow e^{i\gamma}\mathbf J,  
$$

does not change the polarization ellipse and removes one additional degree of freedom. A normalized pure polarization state therefore contains $4-1-1=2$ physically independent real parameters. These may be chosen to be the ellipse orientation $\psi$ and ellipticity $\chi$.

The omitted global phase is distinct from the relative phase between two Raman beams. Although it does not affect the polarization of an individual beam, the optical phase difference between the two beams determines the phase of the two-photon Raman matrix element and must be included separately.

For the geometry of Eqs. (1)–(3), the polarization vector is

$$
\hat{\boldsymbol\epsilon}
=
\epsilon_\theta  
\left(  
\cos\theta_k\,\hat{\mathbf x}
-
\sin\theta_k\,\hat{\mathbf z}  
\right)  
+  
\epsilon_\phi\hat{\mathbf y}.  
$$

Its Cartesian components are therefore

$$ 
\epsilon_x
=
\epsilon_\theta\cos\theta_k,  
$$

$$ 
\epsilon_y
=
\epsilon_\phi,  
$$

and

$$
\epsilon_z=
=
-\epsilon_\theta\sin\theta_k.  
$$

Relative to the quantization axis, the spherical unit vectors are defined as

$$
\hat{\mathbf e}_{+1}

= -\frac{  
\hat{\mathbf x}  
+i\hat{\mathbf y}  
}{\sqrt{2}},  
\qquad  
\hat{\mathbf e}_0

= \hat{\mathbf z},  
\qquad  
\hat{\mathbf e}_{-1}
=
\frac{  
\hat{\mathbf x}  
-i\hat{\mathbf y}  
}{\sqrt{2}}.  
$$

The corresponding spherical components of the polarization vector are

$$
\epsilon_{+1}
=
-\frac{  
\epsilon_x+i\epsilon_y  
}{\sqrt{2}},  
$$

$$ 
\epsilon_0=\epsilon_z,  
$$

and

$$
\epsilon_{-1}
=
\frac{  
\epsilon_x-i\epsilon_y  
}{\sqrt{2}}.  
$$

Using Eqs. (21)–(23), these become

$$
\boxed{  
\epsilon_{+1}
=
-\frac{  
\epsilon_\theta\cos\theta_k  
+i\epsilon_\phi  
}{\sqrt{2}}  
}  
$$

$$
\boxed{  
\epsilon_0
=
-\epsilon_\theta\sin\theta_k  
}  
$$

and

$$ 
\boxed{  
\epsilon_{-1}
=
\frac{  
\epsilon_\theta\cos\theta_k  
-i\epsilon_\phi  
}{\sqrt{2}}.  
}  
$$

Substituting Eqs. (15) and (16) gives the full dependence on the experimental parameters $\theta_k$, $\psi$, and $\chi$:

$$
\epsilon_{+1}
=
-\frac{1}{\sqrt{2}}  
\left[  
\cos\theta_k  
\left(  
\cos\psi\cos\chi  
-i\sin\psi\sin\chi  
\right)  
+  
i\left(  
\sin\psi\cos\chi  
+i\cos\psi\sin\chi  
\right)  
\right],  
$$

$$ 
\epsilon_0
=
-\sin\theta_k  
\left(  
\cos\psi\cos\chi  
-i\sin\psi\sin\chi  
\right),  
$$

and

$$
\epsilon_{-1}
=
\frac{1}{\sqrt{2}}  
\left[  
\cos\theta_k  
\left(  
\cos\psi\cos\chi  
-i\sin\psi\sin\chi  
\right)

i\left(  
\sin\psi\cos\chi  
+i\cos\psi\sin\chi  
\right)  
\right].  
$$

Normalization of the polarization vector implies
$$
|\epsilon_{+1}|^2  
+  
|\epsilon_0|^2  
+  
|\epsilon_{-1}|^2  
=1.  
$$

For an electric-dipole interaction,

$$
-\mathbf d\cdot\mathbf E
=
-\sum_{q=-1}^{+1}  
(-1)^q d_q E_{-q}.  
$$

The atomic dipole component (d_q) connects states satisfying
$$
\Delta m=q.  
$$
It follows that a transition with (\Delta m=q) is driven by the field component (\epsilon_{-q}). This is also the index structure appearing in the E1 limit of the geometric Rabi-frequency expression in Ref. [Campbell]. ([arXiv](https://arxiv.org/pdf/2510.07451 "Angular Geometry of Atomic Multipole Transitions"))

The physical transition amplitudes are therefore identified as

$$
\boxed{  
\epsilon_{\sigma^-}  
\equiv  
\epsilon_{+1}  
}  
\qquad  
(\Delta m=-1),  
$$
$$
\boxed{  
\epsilon_{\pi}  
\equiv  
\epsilon_0  
}  
\qquad  
(\Delta m=0),  
$$

and
$$
\boxed{  
\epsilon_{\sigma^+}  
\equiv  
\epsilon_{-1}  
}  
\qquad  
(\Delta m=+1).  
$$

Thus, the polarization amplitudes used in the transition-strength calculation are

$$ 
\boxed{  
\epsilon_{\sigma^-}
=
-\frac{  
\epsilon_\theta\cos\theta_k+i\epsilon_\phi  
}{\sqrt{2}},  
}  
$$

$$
\boxed{  
\epsilon_{\pi}
=
-\epsilon_\theta\sin\theta_k,  
}  
$$

and
$$
\boxed{  
\epsilon_{\sigma^+}
=
\frac{  
\epsilon_\theta\cos\theta_k-i\epsilon_\phi  
}{\sqrt{2}}.  
}  
$$

The complex amplitudes in Eqs. (40)–(42), rather than only their squared magnitudes, must be retained for Raman transitions because amplitudes associated with different intermediate states and different fine-structure manifolds are added coherently.
#### Optical detuning
### Single-photon electric-dipole amplitude

### Two-photon Raman amplitude

