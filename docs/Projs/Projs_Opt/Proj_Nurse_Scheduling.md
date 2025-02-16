# Nurse Scheduling

The Nurse Scheduling Problem (NSP) involves creating a rotating roster for nurses in a hospital while ensuring compliance with constraints related to their availability and workload. In its simplest form, a two-shift system assigns nurses to either day or night shifts, ensuring balanced coverage and adherence to scheduling requirements.

In this example, we impose two hard constraints to ensure a feasible schedule. The first constraint, known as the **shift constraint**, mandates a minimum number of nurses assigned to each shift. The second, the **nurse constraint**, ensures that each nurse receives a minimum rest period between consecutive shifts. These constraints are essential for maintaining adequate staffing levels while preventing excessive workload for individual nurses.

1.  Shift constraints require that a sufficient number of nurses be assigned to each shift. However, the necessary number may depend on the experience of the assigned nurses, as more experienced nurses may be capable of performing more work during their shift.

2.  By contrast, nurse constraints represent the need to satisfy the appropriate working condition for each nurse. This includes time between shifts to get enough rest as well as days off and scheduled vacations.

We will apply the following constraints to our instance of NSP:

1.  Upper and lwer limit of the number of breaks.
2.  The number of nurses in duty for each shift slot.
3.  Upper and lower limit of time interval between two days of duty.

## Method

The basic idea behind the quantum annealing can be found in [Adiabatic Quantum and Quantum Annealing (AQQA)](../../QuantumOpt/QOpt/AQQA.md). The basic fundation of adiabatic quantum optimization can be described on the time-dependent Schrodinger equation

$$
\hat{H}(t) \lvert\psi(t)\rangle = i \hbar \frac{\partial \lvert\psi(t)\rangle}{\partial t}
$$

where:
1. $\hbar$ is the reduced Planck's constant,
2. $\lvert\psi(t)\rangle = \psi(x,t)$ is the wave function,
3. $\hat{H}(t)$ is the Hamiltonian operator, time-dependent.
4. $i$ is the imaginary unit.

A generic form of this Hamiltonian is 

$$
H(t) = A(t)H_{0} + B(t)H_{1}
$$

with $t \in [0, T]$ and $T$ the final evolution time. And $A(t), B(t)$ are monotonic and satisfy $A(0) = 1, B(0) = 0$ and $A(T) = 0, B(T) = 1$.

### Ising model formulation of NSP
A common approach to casting combinatorial optimization as an Ising model is to first express the problem as quadratic unconstrained binary optimization (QUBO).

Consider a set of $N$ nurses labeled as $n=1,\cdots, N$ and a schedule consisting of $D$ working days labelled as $d = 1,\cdots, D$. Using the binary variable $q_{n,d} \in {0,1}$, let $q_{n,d} = 1$ specify the assignment of nurse $n$ to day $d$. We then consider specific instances of the shift and nurse constraints discussed above.

Three types of constraints are:

1.  **Hard shift constraint**: we require that the schedule must ensure at least 1 nurse is assigned each working each day.
2.  **Hard nurse constraint**: the schedule must ensure no nurse works two or more consecutive days.
3.  **Soft nurse constraint**: requires that all nurses should have approximately even work schedules.

We construct objective functions that correspond to each shift and nurse constraint and then use the sum of these terms to express the QUBO form. 

1.  We introduce composite indices $i(n,d)$ and $j(n,d)$ as functions of the nurse $n$ and the day $d$.
2.  We construct the hard nurse constraint by introducing a symmetric, real-valued matrix $J$ such that $J_{i(n,d),j(n,d+1)} = a$ and zero otherwise.
3.  The positive correlation constant $a$ enforces the nurse constraint by penalizing a schedule for nurse $n$ to work two consecutive days.
4.  The resulting objectuve function is **quadratic**, i.e., $J_{i,j}q_{i}q_{j}$ and takes its minimal when the hard nurse constraint is satisfied. Note that the nurse constraint can be modified by changing the entries of the matrix $J$.

#### Level of efforts
We express the hard shift constraint in terms of the required workforce $W(d)$ needed on each day $d$ and the level of effort $E(n)$ available from each nurse $n$. 

1.  We seek an equality solution for this constraint by introducing a quadratic function that penalizes schedules with too many or too few nurses assigned.
2.  In the same fashion, a quadratic penalty is also used on the soft nurse constraint for failing to account for nurse preferences in the work schedule.
3.  We use $F(n)$ to specify the number of work days that each nurse wishes to be scheduled and $G(n,d)$ to define a prederence for nurse $n$ to work on day $d$.

Here is an simple example of the preference function in the product $G(n,d) = h_{1}(n)h_{2}(d)$, in such a way that 

$$
h_1(n) =
\begin{cases} 
3, & \text{busy} \\ 
2, & \text{moderate} \\ 
1, & \text{idle}, 
\end{cases}
$$

In addition, they can also have options whether they may work on weekend/ night or not by tuning $h_{2}(d):$

$$
h_2(d) =
\begin{cases} 
2, & \text{weekend or night} \\ 
1, & \text{weekday},
\end{cases}
$$

The formulation can be more sophisticaed by including three-shift systems, distinction of weekdays and weekends regarding burden, or day-off request with priority. We simply requre the minimum duty days $F(n)$ for all nurses are equal to or greater than $[D/N]$, where $[x] (x \in R)$ means the integer part of $x$.

Therefore, we can have a QUBO form of 

$$
\begin{array}{ll}
H_{1}(q) & = \sum_{n,n'}^{N}\sum_{n,d'}^{D}J_{i(n,d),j(n',d')}q_{i(n,d)}q_{j(n',d')}\\
& + \lambda \sum_{d}^{D}\bigg( \sum_{n}^{N} E(n)q_{i(n,d)} - W(d) \bigg)^{2}\\
& + \gamma  \sum_{n}^{N}\bigg( \sum_{d}^{D} h_{1}(n)h_{2}(d)q_{i(n,d)} - F(n) \bigg)^{2}
\end{array}
$$

where the positive real-valued numbers $\lambda$ and $\gamma$ tune the relative significance of each term. The objective function
has its mnimum when all the constraints are satisfied and takes on a positive value otherwise. 

1.  We assum that the functions $E(n), F(n),$ and $W(d)$ are integer-valued functions of $n$ or $d$ but this is not required.
2.  We require the minimum duty days $F(n)$ for all nurses $n$ are equal to or greater than $[D/N]$, where $[x] (x \in R)$ means the integer part of $x$.  

Next, we would have to transform this QUBO in to a Ising model.

## Reference

[1]. Nurse Scheduling - by Dwave. [https://cloud.dwavesys.com/leap/examples/254188327](https://cloud.dwavesys.com/leap/examples/254188327).