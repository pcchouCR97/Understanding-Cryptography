# Neutral atom quantum computer

## Introduction
A Neutral atom quantum computer is a new type of quantum computer which is made by *Rydberg atoms* (A Rydberg atom is an excited atom with one or more electrons that have a very high principal quantum number, $n$). It utailzes technologies such as *laser cooling*, *magneto-optical trapping* and *optical tweezers*. To perfrom computation, the atoms are first trapped in a *magneto-optical trap*. Qubits are then encoded in the *energy level* of the atoms. (how?). By manipulate laser on qubits, we can accomplish actions such as initialization and operations. The laser can accomplish arbitrary single qubi gates and a `CZ` gate for *universal quantum gates*. Measurement is enforced at the end of the computation with a camera that generates an image of the outcome by measueing the fluorescence of the atoms.

## Architecture
One of the architecture [[1]](../quantum_computation/q_computer_neutral_atom.md#references), an array of atoms is loaded into a laser cooled at micro-kelvin temperatures. In each of these atoms, two levels of hyperfine ground subspace are isolated. The qubits are prepared in some initial state using optical pumping. Logic gates are performed using optical or microwave frequency fields and the measurements are done using *resonance fluorescence*. 

>   Common atoms types: rubidium(Ru), caesium(Cs), ytterbium(Yb), and strontium(Sr) atoms.

## Single qubit gates
Global single qubit gates on all the atoms can be done either by applying a microwave field for qubits encoded in the [Hyperfine manifold (wiki)](https://en.wikipedia.org/wiki/Hyperfine_structure) such as Rb and Cs or by applying an RF magnetic field for qubits encoded in the nuclear spin such as Yb and Sr. Focused laser beams can be used to do single-site one qubit rotation using a lambda-type three level Raman scheme (see figure). In this scheme, the rotation between the qubit states is mediated by an intermediate excited state. Single qubit gate *fidelities* have been shown to be as high as .999 in state-of-the-art experiments.

## Entangling gate 
The first fast gate based on Rydberg states was proposed for charged atoms making use of the principle of Rydberg Blockade. The principle was later transferred and developed further for neutral atoms [[2]](../quantum_computation/q_computer_neutral_atom.md#references).

### Rydberg mediated gate
Atoms that have been excited to very large principal quantum number $n$ are known as Rydberg atoms. These highly excited atoms have several desirable properties including high decay lift-time and amplified couplings with electromagnetic fields.

> The basic principle for Rydberg mediated gates is called the Rydberg blockade[[3]]

Consider two neutral atoms in their respective ground states. When they close to each other, their interaction potential is dominated by van Der Waals force $V_{qq} \approx \frac{\mu_{B}^{2}}{R^{6}}$ where $\mu_{B}$ is the Bohr Magneton and $R$ is the distance between the atoms. This interaction is very weak, around $10^{-5}$ Hz for $R=10\mu m$. When one of the atoms is put into a Rydberg state (again, a state that has very principle quantum number $n$), the interaction between the two atoms is dominated by second order [dipole-dipole interaction (LibreTests Chemistry)](https://chem.libretexts.org/Bookshelves/Physical_and_Theoretical_Chemistry_Textbook_Maps/Supplemental_Modules_(Physical_and_Theoretical_Chemistry)/Physical_Properties_of_Matter/Atomic_and_Molecular_Properties/Intermolecular_Forces/Specific_Interactions/Dipole-Dipole_Interactions) which is also weak. When both of the atoms are excited to a Rydberg state, then the resonant dipole-dipole interaction becomes $V_{rr} = \frac{(n^{2}ea_{0})^{2}}{R^{3}}$ where $a_{0}$ is the Bohr radius. This interaction is around 100MHz at $R = 10 \mu m$, around twelve orders of mafnitude larger. This interaction potential induces a blockade, where-in, ==if one atom is excited to a Rydberg state, the other nearby atoms cannot be excited to a Rydberg state because the two-atom Rydberg state is far detuned.== This phenomenon is called the Rydberg blockade. Rydberg mediated gates make use of this blockade as a control mechanism to implement two qubit controlled gates.


## References 

[1].    Quantum computing with atomic qubits and Rydberg interactions: Progress and challenges, M. Saffman, arxiv: https://arxiv.org/abs/1605.05207[https://arxiv.org/abs/1605.05207](https://arxiv.org/abs/1605.05207)

[2].    Jaksch, D.; Cirac, J. I.; Zoller, P.; Rolston, S. L.; Côté, R.; Lukin, M. D. (4 September 2000). "Fast Quantum Gates for Neutral Atoms". [https://arxiv.org/abs/quant-ph/0004038](https://arxiv.org/abs/quant-ph/0004038)

[3].    Walker, Thad G.; Saffman, Mark (1 July 2012). "Chapter 2 - Entanglement of Two Atoms Using Rydberg Blockade". Advances in Atomic, Molecular, and Optical Physics. 61. Academic Press: 81–115. [arXiv:1202.5328](https://arxiv.org/abs/1202.5328)

[x].    Neutral atom quantum computer (wiki):[https://en.wikipedia.org/wiki/Neutral_atom_quantum_computer](https://en.wikipedia.org/wiki/Neutral_atom_quantum_computer)

[x].    Rydberg atom (wiki): [https://en.wikipedia.org/wiki/Rydberg_atom](https://en.wikipedia.org/wiki/Rydberg_atom)

