# Bernstein–Vazirani algorithm
## Theory
The Bernstein--Vazirani algorithm (BVA) is a quantum algorithm for identifying a hidden value encoded by an oracle. Its importance comes from the fact that the hidden information can be recovered with a single quantum oracle call, whereas a classical procedure generally requires multiple queries.
In the standard qubit version, the oracle encodes a hidden bit string $(s)$ . In our implementation, the idea is generalized to a single $d$-dimensional system, so the hidden value is an integer
$$s \in \{0,1,\dots,d-1\}.$$
The logical circuit for this algorithm is:
$$\ket{0}  
\;\xrightarrow{\,F_d\,}\;  
\frac{1}{\sqrt d}\sum_{x=0}^{d-1}\ket{x}  
\;\xrightarrow{\,O_s\,}\;  
\frac{1}{\sqrt d}\sum_{x=0}^{d-1}\omega^{s\cdot x}\ket{x}  
\;\xrightarrow{\,F_d^\dagger\,}\;  
\ket{s}.$$
The $d$-dimensional quantum Fourier transform, denoted $F_d$, is the unitary matrix  
$$
F_d = \frac{1}{\sqrt d}\sum_{j=0}^{d-1}\sum_{k=0}^{d-1}\omega^{jk}\ket{j}\bra{k},  
\qquad  
\omega = e^{2\pi i/d}.  
$$
  
Equivalently, its matrix elements are  
$$
(F_d)_{jk} = \frac{1}{\sqrt d}\omega^{jk},  
\qquad j,k \in \{0,\dots,d-1\}.  
$$
  
Its action on a computational basis state $\ket{x}$ is  
$$F_d\ket{x}  
=  
\frac{1}{\sqrt d}\sum_{y=0}^{d-1}\omega^{xy}\ket{y}.  $$
Thus, the QFT transforms a basis state into an equal superposition of all basis states, with phases determined by $x$. In the algorithm, this is the step that spreads the amplitude across the full computational basis so that the oracle can encode information through relative phases.

The oracle used here is a diagonal phase,  
$$O_s\ket{x} = \omega^{s\cdot x}\ket{x},$$
where $s$ is the hidden value to be identified. In matrix form,
$$O_s = \mathrm{diag}\!\left(\omega^{s\cdot 0},\omega^{s\cdot 1},\dots,\omega^{s(d-1)}\right).$$
The quantity $s \cdot x$ is evaluated modulo $d$ because powers of  
$$\omega = e^{2\pi i/d}$$

repeat every $d$ steps. Therefore,  
$$\omega^{sx} = \omega^{(s\cdot x \bmod d)}$$
