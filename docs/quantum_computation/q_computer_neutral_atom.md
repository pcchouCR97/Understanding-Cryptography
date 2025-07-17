# Neutral atom quantum computer

## Introduction
A Neutral atom quantum computer is a new type of quantum computer which is made by *Rydberg atoms* (A Rydberg atom is an excited atom with one or more electrons that have a very high principal quantum number, n. ). It utailzes technologies such as *laser cooling*, *magneto-optical trapping* and *optical tweezers*. To perfrom computation, the atoms are first trapped in a *magneto-optical trap*. Qubits are then encoded in the *energy level* of the atoms. (how?). By manipulate laser on qubits, we can accomplish actions such as initialization and operations. The laser can accomplish arbitrary single qubi gates and a `CZ` gate for *universal quantum gates*. Measurement is enforced at the end of the computation with a camera that generates an image of the outcome by measueing the fluorescence of the atoms.

## Architecture
One of the architecture [[1]](../quantum_computation/q_computer_neutral_atom.md#references), an array of atoms is loaded into a laser cooled at micro-kelvin temperatures. In each of these atoms, two levels of hyperfine ground subspace are isolated. The qubits are prepared in some initial state using optical pumping. Logic gates are performed using optical or microwave frequency fields and the measurements are done using *resonance fluorescence*. 

>   Common atoms types: rubidium, caesium, ytterbium, and strontium atoms.

## Single qubit gates
Global single qubit gates on all the atoms can be done either by applying a microwave field for qubits encoded in the [Hyperfine manifold (wiki)](https://en.wikipedia.org/wiki/Hyperfine_structure) such as Rb and Cs or by applying an RF magnetic field for qubits encoded in the nuclear spin such as Yb and Sr. Focused laser beams can be used to do single-site one qubit rotation using a lambda-type three level Raman scheme (see figure). In this scheme, the rotation between the qubit states is mediated by an intermediate excited state. Single qubit gate *fidelities* have been shown to be as high as .999 in state-of-the-art experiments.

## References 

[1].    Quantum computing with atomic qubits and Rydberg interactions: Progress and challenges, M. Saffman, arxiv: https://arxiv.org/abs/1605.05207[https://arxiv.org/abs/1605.05207](https://arxiv.org/abs/1605.05207)

[2].    Neutral atom quantum computer (wiki):[https://en.wikipedia.org/wiki/Neutral_atom_quantum_computer](https://en.wikipedia.org/wiki/Neutral_atom_quantum_computer)

[3].    Rydberg atom (wiki): [https://en.wikipedia.org/wiki/Rydberg_atom](https://en.wikipedia.org/wiki/Rydberg_atom)

