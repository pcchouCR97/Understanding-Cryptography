# The big O notation

## Intorduction
### Big-O Notation (O)

Big-O gives an **upper bound** on a function’s growth. It describes the **worst-case** scenario — the maximum amount of time or space an algorithm might use. If we write '$f(n) = O(g(n))$', it means that for large enough '$n$', '$f(n)$' does not grow faster than '$g(n)$' (ignoring constant factors).

### Big-Omega Notation (Ω)

Big-Omega gives a **lower bound**. It tells us the **minimum** resources required — the best-case performance. If '$f(n) = \Omega(g(n))$', it means '$f(n)$' grows at least as fast as '$g(n)$' for large '$n$'.

### Big-Theta Notation (Θ)

Big-Theta gives a **tight bound**, meaning it describes the **exact growth rate**. If '$f(n) = \Theta(g(n))$', then '$f(n)$' grows at the same rate as '$g(n)$' up to constant factors. This implies both $'O(g(n))$' and '$\Omega(g(n))$' hold.


## Computational complexity

From Fast to Slow:

$$
\begin{array}{|c|c|l|}
\hline
\textbf{Complexity} & \textbf{Meaning} & \textbf{Example} \\
\hline
O(1)       & \text{Constant time}   & \text{Access array by index} \\
O(\log n)  & \text{Logarithmic}     & \text{Binary search} \\
O(n)       & \text{Linear}          & \text{Loop through list} \\
O(n \log n)& \text{Linearithmic}    & \text{Merge sort, heap sort} \\
O(n^2)     & \text{Quadratic}       & \text{Bubble sort, nested loops} \\
O(n^3)     & \text{Cubic}           & \text{3 nested loops} \\
O(2^n)     & \text{Exponential}     & \text{Subset generation} \\
O(n!)      & \text{Factorial}       & \text{Brute-force permutation sorting} \\
\hline
\end{array}
$$

In general:

* Good: **O(1)**, **O(log n)**, **O(n)**
* Acceptable: **O(n log n)**
* Bad: **O(n²)** and worse for large $n$

### **O(log n) — Halving Until You Find It**

Binary search appears when you repeatedly **cut a sorted set in half** and only follow the half that matters.

A perfect real-life example is the **Guess the Number** game. Imagine someone picks a number between 1 and 100. You don’t guess randomly — instead, you always choose the **middle number** and ask if the target is higher or lower. Each time, you eliminate half of the remaining options. In just a few guesses, you zero in on the correct number.

This strategy works because the problem size shrinks **exponentially** — from 100 to 50 to 25 and so on. After around **log₂(n)** steps, there’s only one number left. That’s why binary search runs in **O(log n)** time.

### **O(n log n) — Divide and Touch Everything**

**O(n log n)** arises when an algorithm **splits** the data repeatedly and still needs to **process every element** at each level of splitting. For example, in merge sort, the data is divided in half each time — this creates **log n levels** of recursion. But at every level, the algorithm still needs to **go through all n elements** to merge them. That’s why the total time becomes:

$$
\text{(log n levels)} \times \text{(n elements per level)} = O(n \log n)
$$

### **O(2ⁿ) — Problems of Choice**

A classic example is the **0/1 Knapsack problem**. Given `n` items, each with a value and weight, you decide for each one whether to include it in your bag without exceeding a weight limit. Every item presents two choices — take it or not — leading to $2^n$ possible combinations. That’s why brute-force knapsack is exponential.

A more relatable version is your **closet in the morning**. Imagine you own `n` shirts. Each day, you can either wear or not wear each shirt (ignoring fashion logic). That leads to $2^n$ outfit combinations — one for every possible subset of shirts.

### **O(n!) — Problems of Ordering**

On the other hand, **O(n!)** arises when the task is to examine every possible **ordering** of `n` items. A textbook case is the **Traveling Salesman Problem (TSP)**: a salesman must visit `n` cities once and return to the start, aiming for the shortest route. Every route is a unique permutation of the cities — and there are $n!$ of them.

A real-world analogy? Think of planning a **talent show** with `n` singers. You must decide who performs in which order. Each different sequence is a new possibility — and the total number of such lineups is again $n!$.

So in short, **2ⁿ** shows up when you’re picking **any combination** (yes/no for each item). **n!** appears when you’re deciding **all possible orders** of things. In algorithm analysis, two of the steepest time complexities are **O(2^{n})** and **O(n!)**, both representing explosive growth as input size increases. But they arise from different types of problems.
