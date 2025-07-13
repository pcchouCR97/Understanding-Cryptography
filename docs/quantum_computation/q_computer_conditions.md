# Conditions for quantum computation

## Physical Realizations of Qubits
A quantum computer must strike a balance between two conflicting needs:

1.  **Isolation** – to preserve quantum coherence.  
2.  **Accessibility** – to allow operations and readout.

> *A quantum computer has to be well isolated... but its qubits have to be accessible...*

These timescales determine how “good” a quantum system is:

1.  **$\tau_{Q}$**: Coherence time: how long a qubit remains quantum.  
2.  **$\tau_{op}$**: Operation time: time to apply a gate.  
3.  **$\lambda = \tau_{op} / \tau_{Q}$**: Noise strength (smaller is better).  
4.  **$n_{op} = \lambda^{-1}$**: Max ops before decoherence.

$$
\begin{array}{|l|c|c|c|}
\hline
\textbf{System} & \tau_Q & \tau_{op} & n_{op} = \lambda^{-1} \\
\hline
\text{Nuclear spin} & 10^{-2} \text{ to } 10^{8} & 10^{-3} \text{ to } 10^{-6} & 10^5 \text{ to } 10^{14} \\
\text{Electron spin} & 10^{-3} & 10^{-7} & 10^4 \\
\text{Ion trap (In}^+) & 10^{-1} & 10^{-14} & 10^{13} \\
\text{Electron – Au} & 10^{-8} & 10^{-14} & 10^6 \\
\text{Electron – GaAs} & 10^{-10} & 10^{-13} & 10^3 \\
\text{Quantum dot} & 10^{-6} & 10^{-9} & 10^3 \\
\text{Optical cavity} & 10^{-5} & 10^{-14} & 10^9 \\
\text{Microwave cavity} & 10^{0} & 10^{-4} & 10^4 \\
\hline
\end{array}
$$


## 4 basic requirements

1.  Robustly represent quantum infomation
2.  Perform a universal family of unitary transformations
3.  Perpare a fiducial initial state
4.  Measure the output result

## Representation of quantum infomation

Quantum computation is based on transforming quantum states. Qubits are two‐level systems that provide a convenient finite state space for computation.

1.  **Finite state space is crucial**  
    *   Continuous variables (e.g. position $x$) inhabit an infinite‐dimensional Hilbert space—unrealistic once noise is included.  
    *   Noise limits the number of distinguishable states to a finite set.
    
    For example, in a perfect world, the entire texts of Shakespeare could be stored in the infinite number of digits in the binary fraction $x = 0.010111011001...$. What happens in reality is that the presence of noise reduces the number of distinguishable states to a finite number.

2.  **Symmetry enforces finiteness**  
    *   It is generally desirable to have some aspect of symmetry dictate the finiteness of the state space, in order to minimize decoherence.
    *   A spin-$\tfrac12$ particle lives in the two‐dimensional space spanned by $|\!\!\uparrow\rangle$ and $|\!\!\downarrow\rangle$. When well isolated, this is an almost ideal qubit.

3.  **Poor representations lead to decoherence**  
    *   Example: a finite square well with exactly two bound states still couples to the continuum—transitions destroy superpositions.  
    *   Any leakage out of the two‐level subspace adds noise.

4.  **Figures of merit for single qubits**  
    *   $T_2$ (transverse relaxation time): minimum lifetime of arbitrary superpositions (best measure of coherence).  
    *   $T_1$ (longitudinal relaxation time): lifetime of the energy eigenstate $|1\rangle$ (a “classical” lifetime, usually $>T_2$).

> *“Anything which causes loss of quantum information is a noise process.”*

## Performance of unitary transformations

Closed quantum systems evolve under their Hamiltonian, but quantum computation requires the ability to **control** that Hamiltonian to implement any desired gate.

1. **Hamiltonian Control**  
    *   Evolution under  
        
        $$
        H = P_x(t)\,X + P_y(t)\,Y
        $$  

        where $P_{x,y}(t)$ are classical control parameters.
        
    *   By shaping $P_x$ and $P_y$, one can perform arbitrary single‐spin rotations.

2. **Universal Gate Set**  
    *   **Any unitary can be decomposed into single‐spin rotations + `CNOT` gates.**
    *   Requires **addressability**: Implicitly required also is the ability to address individual qubits, and to apply these gates to select qubits or pairs of qubits. e.g. in an ion trap you must focus a laser on individual ions (spaced $\geq$ one wavelength).

3. **Imperfections → Decoherence**  
    *   Unrecorded imperfections in unitary transfoms can lead to decoherence.
    *   **Random errors**: uncontrolled “kicks” (small rotations about $\hat z$) introduce random relative phases → loss of coherence.  
    *   **Systematic errors**: calibration drifts accumulate into irreversible noise if you lose the information needed to reverse them.  
    *   **Back‐action**: controls are quantum too. For example, a Jaynes–Cummings interaction  
        
        $$
        P_x(t)=\sum_k \omega_k(t)\,(a_k + a_k^\dagger)
        $$
        
        couples the qubit to the photon field, which can carry away state information.

4. **Figures of Merit**  
    *   **Fidelity** $\mathcal{F}$: minimum achievable fidelity of the target unitary. 
    *   **Operation time** $t_{op}$: maximum time needed for elementary gates (rotations, `CNOT`).

> *High‐precision control of $H$ and suppression of all error sources are key to achieving high‐fidelity quantum gates.*  


## Preparation of fiducial initial states

Quantum computation requires a reliable method to prepare a known input state. This is non-trivial for quantum systems.

1.  **Classical vs Quantum Input**  
    *   In classical computers, inputs are trivial (bit switches).  
    *   In quantum systems, preparing a known state (e.g. all spins in $|0\rangle$) is hard due to system-dependent constraints.

2.  **Sufficient Input**  
    *   Only one known pure state is needed (e.g. $|00\ldots0\rangle$), since unitary evolution can generate any other state.  
    *   Challenge: maintaining the state due to heating or noise.

3.  **Physical Realization**  
    *   Ions: prepared via **laser cooling** into their ground state.  
    *   Ensembles (e.g. NMR):  
        *   Each molecule is a qubit.  
        *   Many molecules needed for a measurable signal.  
        *   Hard to align all in the same quantum state.

    *   In thermal equilibrium:  
        
        $$
        \rho \approx \frac{e^{-\mathcal{H}/k_B T}}{\mathcal{Z}}
        $$
        
        where $\mathcal{Z}$ normalizes $\text{tr}(\rho) = 1$.

4.  **Figures of Merit**  
    *   **Fidelity**: accuracy of preparing a target state $\rho_{\text{in}}$.  
    *   **Entropy** of $\rho_{\text{in}}$:  
        *   Example: $\rho_{\text{in}} = I/2^n$ is easy to make, but useless (max entropy, fully mixed).  
        *   Ideal state = pure, zero entropy.

> *Good computation begins with a pure input. Noise in the input reduces information accessibility.*


## Measurement of output result
Quantum computation requires a way to extract classical results from quantum states.

1.  **Basic Measurement Model**  
    *   Couple qubit to classical system → observe final state.  
    *   Example:
        *   State $a|0\rangle + b|1\rangle$ measured via fluorescence:
            *   Detect light → collapse to $|1\rangle$ with prob. $|b|^2$
            *   No light → collapse to $|0\rangle$

2.  **Wavefunction Collapse**  
    *   Projective measurement maps quantum superposition to classical value.  
    *   In algorithms like Shor's, output is superposition over $c$ values; collapse gives random $c$ to infer period $r$.

3.  **Measurement Challenges**  
    *   **Noise**: Photon loss, amplifier thermal noise, inefficient detection.  
    *   **Strong measurements**: Require large, switchable coupling → technically hard and may introduce decoherence.  
    *   **Timing**: Measurement must not occur prematurely.

4.  **Weak & Ensemble Measurements**  
    *   Weak measurements: always-on, continuous coupling can work.  
    *   Ensemble readouts: large groups of qubits give a **macroscopic signal**, e.g. NMR systems.  
    *   But: ensemble returns $\langle c \rangle$, not discrete $c$ → averaging can break algorithms needing exact integers.  
    *   Solution: modify algorithm for ensemble-compatible readout.

5.  **Figure of Merit**  
    *   **Signal-to-Noise Ratio (SNR)**: Measures how distinguishable the output is despite inefficiencies or weak signals.

> *Measurement must be precise, controllable, and not disrupt the quantum state until the computation is complete.*

## References 

[1]. M. A. Nielsen and I. L. Chuang, *Quantum Computation and Quantum Information*, 10th Anniversary Ed., Cambridge: Cambridge University Press, 2010.