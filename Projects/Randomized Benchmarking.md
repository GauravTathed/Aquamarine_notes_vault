# Randomized Benchmarking
### Goals:
Randomized benchmarking (RB) is used to estimate the average error rate of a set of quantum gates while reducing sensitivity to state-preparation and measurement (SPAM) errors. Experimentally, the protocol consists of applying sequences of randomly chosen Clifford gates or Haar Random unitaries of varying length and measuring how the survival probability decays as the sequence length increases.
Goal is to perform and analyze Randomized Benchmarking on $^{137}\text{Ba}^+$ quDit. Check the feasibility of using RB as coherence metric and performance metric for Line Signal Compensation and other noise sources. 

## Theory work
I am sampling Haar Random unitaries in this manner:

```python
def haar_unitary(n, rng=None):
    rng = np.random.default_rng(rng)
    X = (rng.standard_normal((n, n)) + 1j * rng.standard_normal((n, n))) / np.sqrt(2)
    Q, R = np.linalg.qr(X)
    d = np.diag(R)
    D = d / np.abs(d)
    return Q * D

def haar_su(n, rng=None):
    U = haar_unitary(n, rng)
    phi = np.angle(np.linalg.det(U)) / n
    return U * np.exp(-1j * phi)
```

After Sampling we put it through [[Transition Aware QR decomposition algorithm]] to decompose the unitary into implementable pulses. 
For RB experiment in particular we don't are not implementing the exact unitary that was sampled Haar randomly, we have a phase on each of the final population that is different from the phase of intended unitary. 

The RB experiment itself is straightforward - 
For a sequence of length $m$, we generate a set of random unitaries
$$
U_1, U_2, \dots, U_m,
$$
where each $U_i$ is independently sampled from the Haar distribution. After applying this sequence, we append an inversion operation $U_{\mathrm{inv}}$ chosen such that the ideal total evolution is the identity:
$$
U_{\mathrm{inv}} U_m \cdots U_2 U_1 = I.
$$
Equivalently,
$$
U_{\mathrm{inv}} = \left(U_m \cdots U_2 U_1\right)^{-1}.
$$

Experimentally, the protocol proceeds as follows. The system is first prepared in the initial state $\ket{0}$. A randomly generated sequence of $m$ Haar-random unitaries is then applied, followed by the corresponding inversion operation. Finally, the population in the reference state is measured. In the absence of noise, the total sequence ideally maps the system back to the initial state with unit probability. In practice, control errors and decoherence reduce the probability of returning to the target state, and this survival probability decreases as the sequence length increases.

For each sequence length $m$, many independently generated random unitary sequences are measured, and each sequence is repeated multiple times in order to estimate the average survival probability. Averaging over the random realizations yields the mean survival probability at that sequence length. Repeating this procedure for several values of $m$ produces a decay curve that reflects the accumulation of errors under repeated application of random unitaries.

Assuming the noise is approximately Markovian and time independent, the measured survival probability can be fit to the exponential form
$$
P(m) = A p^m + B,
$$
where $A$ and $B$ account for state-preparation and measurement errors, and $p$ parameterizes the average decay per applied random unitary layer. The validity of this form relies on the assumption that errors are effectively memoryless and do not drift or vary systematically throughout the experiment. Under these conditions, the extracted decay constant provides a measure of the average error associated with the implemented operations.

This assumption can be violated by temporally correlated or explicitly time-dependent noise. For example, a periodic error source such as AC line noise can imprint coherent modulations on the applied operations, leading to departures from a purely exponential decay. In that case, the survival probability may exhibit oscillatory structure or other non-exponential behavior, indicating a breakdown of the simple Markovian error model. \

## Experimental Results
Working on Analysis Script