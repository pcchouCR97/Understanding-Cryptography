# Jop shop problem

## Problem definition and conventions
A Jop shop problem (JSP) consists of 

1.  A set of jobs $\mathcal{J} = \{j_{1},\cdots,j_{N}\}$ that be scheduled on a set of machines $\mathcal{M} = \{m_{1},\cdots,m_{N}\}$. 
2.  Each job consists of a sequence of operations that must by performed in a predefined order $j_{n} = \{ O_{n1}\rightarrow O_{n2}\rightarrow \cdots O_{nL_{n}} \}$ of total of $L_{n}$ operations. 
3.  Each operation $O_{nj}, j = 1,\cdots L_{n}$ has integer execution time $p_{nj}$.
4.  And $O_{nj}$ has to be executed by an assigned machine $m_{qnj} \in \mathcal{M}, q = 1,\cdots, M$, assigned machine. 

There can only be one operation tunning on any given machine at any given point in time and each opeariton of a job needs to complete before the following one can start. The usual objective is to schedule all operations in a valide sequence while minimizing the makespan (i.e., the completion time of the last running job). We will denote $\mathcal{T}$ the minimum possible makespan associated with a given JSP instance.

As defined above, the JSP variant we consider is denoted $\text{J}M|p_{nj}\in [p_{\text{min}},\cdots,p_{\text{max}}]|C_{max}$ in the $\alpha|\beta|\gamma$ notationm where $p_{\text{min}}$ and $p_{\text{max}}$ are the smallest and largest execution times allowed, respectively. In this notation, $\text{J}M$ stands for job-shop type on $M$ on mahcines, and $C_\text{max}$ means we are optimizing the makespan. For notational convenience, we enumerate the operations in a lexicographical order in such a way that 

$$
\begin{array}{ll}
J_{1} = & \{ O_{1}\rightarrow \cdots \rightarrow O_{k_{1}} \},\\
J_{2} = & \{ O_{k_{1}+1}\rightarrow \cdots \rightarrow O_{k_{2}} \},\\
\cdots = & \\
J_{N} = & \{ O_{k_{N-1}+1}\rightarrow \cdots \rightarrow O_{k_{N}} \}.
\end{array}
\tag{1}
$$

1.  Given the running index **overall operations** $i \in {1,\cdots,k_{N}}$, we let $q_{i}$ be the index of the machine $m_{q_i}$ responsible for executing operation $O_{i}$.
2.  We define $I_{m}$ to be the set of indices of all of the operations that have to be executed on machine $m_{m}$, i.e., $I_{m} = \{i:q_{i} = m \}$. The execution time of operation $O_{i}$ is now simply denoted $p_i$.
3.  A job $j$ can use the same machine more than oncem or use only a fraction of the $M$ available machines. Auturs in [1](Proj_Flow_Shop_Scheduling.md#references) define a ratio of $\theta$ that specifies the fraction of the total number of mahcines that is used by each job, assuming no repetition when $\theta < 1$. For example, a ratio of $0.5$ means that each job uses only $0.5M$ distinct machines.

## Quantum annealing formulation
In this work, we seek a suitable formulation of the JSP for a quantum annealing optimizer. The optimizer is best described as an oracle that solves an Ising problem with a given probability. This Ising problem is equivalent to a quadratic unconstrained binary optimization (QUBO) problem.

The optimizer is expected to find the global minimum with some probability which itself depends on the problem and the device’s parameters.

We distinguish between the *optimization* version of the JSP, in which we seek a valid schedule with a minimal makespan, and the *decision* version which is limited to validating whether or not a solution exists with a makespan smaller than or equal to a user-specified timespan $T$.

We focus exclusively on the decision version and later describe how to implement a full optimization version based on a binary search.

## QUBO Problem Formulation

We assign a set of binary variables for each operation, corresponding to the various possible discrete starting times the operation can have 

$$
x_{i,t} =
\begin{cases} 
1, & \text{operation } O_i \text{ starts at time } t, \\
0, & \text{otherwise}.
\tag{2}
\end{cases}
$$

1.  Here $t$ is bounded from above by the timespan $T$, which represents the maximum time we allow for the jobs to complete. The timespan itself is bounded from above by the total work of the problem, that is, the sum of the execution times of all operations.

### Constraints
#### An operation must start once and only once

An opeartion must start once and only once leads to the constraint and assocaited penalty function 

$$
\bigg(\sum_{t}x_{i,t} = 1 \ \text{for each}\ i \bigg) \rightarrow \sum_{i}\bigg( \sum_{t} x_{i,t} - 1\bigg)^{2}.
\tag{3}
$$

#### There can only be one job running on each machine at any given point in time

There can only be one job running on each machine at any given point in time, which expressed as quadratic constraints yields

$$
\sum_{(i,t,k,t')\in R_{m}} x_{i,t}x_{k,t'} = 0 \ \text{for each} \ m,
\tag{4}
$$

where $R_{m} = A_{m} \cup B_{m}$ and 

$$
\begin{array}{ll}
A_{m} = & \{(i,t,k,t'): (i,k) \in I_{m} \times I_{m},\\
        & i \neq k, 0 \leq t, t'\leq T,0 < t'-t < p_{i}\},\\
B_{m} = & \{(i,t,k,t'): (i,k) \in I_{m} \times I_{m},\\
        & i < k, t' = t, p_{i} > 0, p_{j}>0\}.
\end{array}
$$

*   The set $A_{m}$ is defined so that the constraint forbids operation $O_{j}$ from starting at $t'$ if there is another operation $O_{i}$ still running. This happens if $O_{i}$ started at time $t$ and $t' - t$ is less than $p_{i}$.
*   The set $B_{m}$ is defined so that two jobs $J$ cannot start at the same time, unless at least of of them has an execution time equal to zero.

#### Enforced order of the operations within a job

The order of the operations within a job are enforced with 

$$
\sum_{k_{n-1}<i<k_{n}, t+p_{i} > t} x_{i,t}x_{i+1,t'} \ \text{for each}\ n,
\tag{5}
$$

which counts the number of precedence voilations between consecutive operations only.

### Hamiltonian

The resulting classical objective function (Hamiltonian) is given by 

$$
H_{T}(\overline{x}) = \eta h_{1}(\overline{x}) + \alpha h_{2}(\overline{x}) + \beta h_{3}(\overline{x}),
\tag{6}
$$

where

*The order of the operations within a job are enforced with:*

$$
h_{1}(\overline{x}) = \sum_{n}\bigg( \sum_{k_{n-1}<i<k_{n}, \ t+p_{i} > t} x_{i,t}x_{i+1,t'}\bigg), \ \text{for each}\ n,
\tag{7}
$$

*There can only be one job running on each machine at any given point in time:*

$$
h_{2}(\overline{x}) = \sum_{m}\bigg( \sum_{(i,t,k,t')\in R_{m}} x_{i,t}x_{k,t'} \bigg), \ \text{for each}\ m,
\tag{8}
$$

*An operation must start once and only once:*

$$
h_{3}(\overline{x}) = \sum_{i}\bigg( \sum_{t} x_{i,t} - 1\bigg)^{2},
\tag{10}
$$

and the penalty constants $\eta, \alpha$ and $\beta$ are required to be larger than $0$ to ensure that unfeasible solution do not have a lower energy then the ground state(s). 

As expected for a decision problem, we note that the minimum of $H_{T}$ is $0$ and it is only reached if a schedule satisfies all of the constraints. The index of $H_T$ explicitly shows the dependence of the Hamiltonian on the timespan T, which addects the number of variables involved.

### Appendix

**1. $R_m$ – The Set of Conflicting Job Pairs on Machine $m$**
$$
R_m = A_m \cup B_m
$$
- $R_m$ contains **pairs of operations** that **cannot run simultaneously** on machine $m$.
- It is formed by combining two subsets:  
  - **$A_m$** (Jobs overlapping in time)  
  - **$B_m$** (Jobs starting at the same time)

---

**2. $A_m$ – Overlapping Job Constraint**
$$
A_m = \{(i,t,k,t') : (i,k) \in I_m \times I_m, \quad i \neq k, \quad 0 \leq t, t' \leq T, \quad 0 < t' - t < p_i \}
$$
- **Ensures that if an operation $O_i$ is running, no other operation $O_k$ can start before $O_i$ finishes.**
- **$t' - t < p_i$** → If $O_i$ starts at $t$, then $O_k$ cannot start at $t'$ if $O_i$ is still running.
- **Example**:  
  - $O_1$ starts at **$t = 2$** and has **$p_1 = 4$**.
  - Another job $O_2$ **cannot start before $t = 6$ (2 + 4)** on the same machine.

---

**3. $B_m$ – Simultaneous Start Constraint**
$$
B_m = \{(i,t,k,t') : (i,k) \in I_m \times I_m, \quad i < k, \quad t' = t, \quad p_i > 0, \quad p_j > 0 \}
$$
- **Prevents two jobs from starting at the exact same time unless one has zero execution time.**
- **$t' = t$** → If $O_i$ and $O_k$ both try to start at $t$, at least one must have $p = 0$.
- **Example**:  
  - If $O_1$ and $O_2$ are scheduled at $t = 3$, at least one must be a dummy job ($p = 0$).


#### **1. What is $I_m \times I_m$?**
- **$I_m$ is the set of operations assigned to machine $m$**.
- **$I_m \times I_m$** means **all possible pairs** of operations on the same machine.

👉 **Example**:
- Suppose **Machine 1** has **3 operations**: $I_1 = \{O_1, O_2, O_3\}$.
- Then, **$I_1 \times I_1$** creates pairs of operations:
  $$
  I_1 \times I_1 = \{(O_1, O_1), (O_1, O_2), (O_1, O_3), (O_2, O_1), (O_2, O_2), (O_2, O_3), (O_3, O_1), (O_3, O_2), (O_3, O_3) \}
  $$
- This includes all possible combinations (even pairs with themselves, but in constraints we usually exclude those).

---

#### **2. Simple Example of $A_m$ (Overlapping Jobs Constraint)**
**Definition**:
$$
A_m = \{(i,t,k,t') : (i,k) \in I_m \times I_m, \quad i \neq k, \quad 0 < t' - t < p_i \}
$$
- If **job $O_i$ is running**, another **job $O_k$ cannot start** until $O_i$ finishes.

👉 **Example**:
- **Machine 1** has $I_1 = \{O_1, O_2\}$.
- **Durations**: $O_1$ takes **3 time units**.
- **$O_1$ starts at $t = 2$**.
- **$O_2$ tries to start at $t' = 3$**.
- Since **$O_1$ is still running** (from $t=2$ to $t=5$), **$O_2$ must wait**.
- **Valid pairs** in $A_m$:  
  $$
  (O_1,2,O_2,3), (O_1,2,O_2,4)
  $$
  because $0 < (t' - t) < p_1$ (i.e., $0 < 3-2 < 3$).

---

#### **3. Simple Example of $B_m$ (Simultaneous Start Constraint)**
**Definition**:
$$
B_m = \{(i,t,k,t') : (i,k) \in I_m \times I_m, \quad i < k, \quad t' = t, \quad p_i > 0, \quad p_k > 0 \}
$$
- Two **operations cannot start at the same time** unless one has $p = 0$ (dummy operation).

👉 **Example**:
- **Machine 2** has $I_2 = \{O_3, O_4\}$.
- Both jobs **try to start at $t = 1$**.
- If **$p_3 > 0$ and $p_4 > 0$**, then **this is not allowed**.
- **Valid pairs in $B_m$**:
  $$
  (O_3,1,O_4,1)
  $$
  since both jobs **attempt to start at the same time**.

---

### **Final Summary**
| **Set** | **Purpose** | **Example** |
|---------|------------|------------|
| $I_m \times I_m$ | All operation pairs on the same machine | $\{(O_1, O_2), (O_2, O_3)\}$ for $I_m = \{O_1, O_2, O_3\}$ |
| $A_m$ | Prevents overlapping execution | If $O_1$ runs from $t=2$ to $t=5$, $O_2$ cannot start at $t=3$ |
| $B_m$ | Prevents two jobs starting together | If $O_3$ and $O_4$ both try to start at $t=1$, it is **not allowed** |

This ensures that **machines are used efficiently, preventing job conflicts**. 


## References

[1]. D. Venturelli, D. Marchand, and G. Rojo, "Quantum Annealing Implementation of Job-Shop Scheduling", [arXiv:1506.08479v2](https://arxiv.org/abs/1506.08479v2)

[2]. Job Shop Scheduling [Dwave](https://cloud.dwavesys.com/leap/examples/222052816)