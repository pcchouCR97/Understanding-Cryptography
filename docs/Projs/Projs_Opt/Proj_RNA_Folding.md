# RNA Folding

RNA folding refers to the process by which a single-stranded RNA molecule folds into a specific three-dimensional structure based on its sequence of nucleotides.

## Quantum computing goal in RNA Folding

The quantum applicatopm is to optimize the RNA folding process. The focus is on representing RNA folding as a combinatorial optimization problem that can be solved using quantum annealing. This involves:

*   Encoding RNA sequence and rules into a quantum system.
*   Using quantum algorithm (quantum annealing or others) to explore possible conformations and identify the optimal folding pattern that minimizes energy.


Representation of RNA Sequence:

1.  The RNA sequence consists of A (Adenine), U (Uracil), G (Guanine), and C (Cytosine).
2.  Hydrogen bonds form between A-U and G-C pairs.
3.  The goal is to determine which of these pairs should be formed to minimize the overall energy.

Predicting the existence of stems is important to predicting the properties of the RNA molecule. However, prediction is complicated by two important factors.

In predicting the stems of an RNA molecule, we build a quadratic model with three contributing factors.


### Pseudoknot
Second, the intertwining phenomenon known as a pseudoknot is less energetically favorable. In Figure 2, we see an example of such a pseudoknot, where one side of a stem occurs in between the two sides of a different stem. The use of a quadratic objective allows us to make pseudoknots less likely to occur in optimal solutions, increasing overall accuracy. Specifically, we include a quadratic term for each pair of stems that, if present, form a pseudoknot. The positive coefficient on this quadratic term discourages the forming of pseudoknots without explicitly disallowing the

## Problem Formulation
In predicting the stems of an RNA molecule, we build a quadratic model with three contributing factors.

1.  Each potential stem is encoded as a binary variable, linearly weighted by the negative square of the length, $k$.
2.  Each potential pseudoknot is encoded as a **quadratic term**, weighted by to the product of the two lengths times a positive parameter $c$.
3.  Overlapping stems are not allowed. Potential overlaps give rise to constraints in the model.

The model can be formulated as:

$$
\begin{array}{ll}
\text{minimize} & \sum_{i} -k_{i}^{2}x_{i} + c \sum_{(i,j)\in S} k_{i}k_{j}x_{i}x_{j}\\
\text{subject to} & x_{i} + x_{j} \leq 1, \text{for all pairs of overlapping stems, $i$ and $j$}
\end{array}
$$

Here, each $x_{i}$ is a binary variable indicating the inclusion/ exclusion of the $i^{th}$ stem. Each constant $k_{i}$ is the length of said stem. The indexing set $S$ is the set of all pairs stems that from a pseudoknot. 
-   $c$ is a tunable parameter adjusting the impact of pseudoknots. It is set to $0.3$ by default. If $c=0$, the affect of pseudoknots is ignored, while $c>1$ eliminates all pseudoknots from optimial solutions. This formulation (and default choice of $c$) is loosely based on [[1].](../../Projs/Projs_Opt/Proj_RNA_Folding.md#reference)

Overlap in RNA stems happens when two different stem structures try to use the same nucleotide(s) in their base pairing. Since a single nucleotide cannot be part of two different base pairs at the same time, the two stems compete for that region of the RNA sequence

## Code Overview 

1.  Preprocessing the RNA sequence to extract all possible stems, pseudoknots, and overlaps.
2.  Building the model and sending it to a hybrid solver to find a solution.
3.  Post-processing the solution to print appropriate information and create the plot.

A majority of the code is dedicated to step 1. Here, possible bonds are stored in a binary matrix, and the matrix is searched for possible stems. Possible stems (each corresponding to a decision variable) are stored in a dictionary structure that reduces the number of comparisons necessary when searching for pseudoknots and overlaps.

## Reference

[1] [RNA Folding](https://cloud.dwavesys.com/leap/examples/416822653)

[2] Fox DM, MacDermaid CM, Schreij AM, Zwierzyna M, Walker RC. "RNA folding using quantum computers," [PLOS Computational Biology](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1010032).

[3] Kai, Zhang, et al. "An efficient simulated annealing algorithm for the RNA secondary structure prediction with Pseudoknots," [BMC Genomics](https://bmcgenomics.biomedcentral.com/articles/10.1186/s12864-019-6300-2).
