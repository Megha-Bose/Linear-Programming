# Linear Programming Notes
---
## Introduction

### Aim
- **Linear programming is an optimization technique for a system of linear constraints and a linear objective function.**
- Here, "linear" means an expression of the form $\sum_i a_i x_i + b$ where $x_i$'s are variables and $b$ is a constant.
- **The objective function** defines the quantity to be optimized. The aim of linear programming is to find the values of the variables (decision variables) that maximize or minimize the objective function while satisfying all the constraints.


### Algorithms

- The simplex algorithm for solving linear programming problems performs well in practice but it takes exponential time in the worst case. 
- However, linear programming has been solved in polynomial time by algorithms like the ellipsoid algorithm, and the Karmarkar’s algorithm which led to increased research in interior-point methods for linear programming. 
- LP-duality is used as a prime proof technique in several algorithms and associated theorems which we will look at subsequently.

---
## Modelling the problem

### The general problem
**Maximize (or) minimize objective function $Z = c_{1}x_{1} + c_{2}x_{2} + c_{3}x_{3} + ... c_{n}x_{n}$ subject to the constraints:**

$a_{11}x_{1} + a_{12}x_{2} + ... + a_{1n}x_{n} \leq \text{or} =$ or $\geq b_{1}$
$a_{21}x_{1} + a_{22}x_{2} + ... + a_{2n}x_{n} \leq \text{or} =$ or $\geq b_{2}$
$...$
$a_{m1}x_{1} + a_{m2}x_{2} + ... + a_{mn}x_{n} \leq \text{or} =$ or $\geq b_{m}$

and the **non-negativity restrictions** $x_{1}, x_{2}, x_{3}, ... x_{n} \geq 0$.

### Basic Terminology
- If $x$ satisfies $Ax = b$, $x \geq 0$, then x is a **feasible solution**.
- A linear program is feasible if $∃$ a feasible solution, otherwise it is said to be infeasible.
- An **optimal solution** $x^{∗}$ is the minimum (or maximum) out of all feasible solutions.
- A linear program is **unbounded from below (or above)** if $\forall\lambda$ $\in$ R, $\exists$ a feasible $x^{*}$ s.t. $c^{T}x^{∗} \leq (or \geq) λ$ where $c$ is the coefficient vector of the objective function.

### Canonical and Standard Forms
Let the decision variables in a linear program be  $x_{1}, x_{2}, x_{3}, ... x_{n}$, subject to linear equality or inequality constraints. To describe properties of and algorithms for linear programs, it is convenient to express them in similar forms. 

#### Canonical form
A linear program is said to be in canonical form if it satisfies the following conditions:
- Objective function requires to be maximised
- Permits only $\leq$ constraints
- The decision variables must be non-negative

To convert any linear program to its canonical form, we
- Replace any constraint of the form $ax \geq b$ with $-ax \leq -b$. and constraint of the form $ax = b$ with two contraints $ax \leq b$ and $-ax \leq -b$. 
- Change a minimization problem to a maximization problem by negating the objective function.
- Replace any unconstrained or negative variable $x$ with the difference of two new variables $x^{+}-x^{-}$ where $x^{+}, x^{-} \geq 0$.


#### Standard form
A linear program is said to be in standard form if it satisfies the following conditions:
- RHS (constant side) of the constraints needs to be non-negative
- Permits only $=$ constraints
- The decision variables must be non-negative

To convert any linear program to its standard form, we
- Replace any constraint of the form $ax \geq (or \leq) b$ where $b<0$  with $-ax \leq (or \geq) -b$.
- Add a nonnegative variable to $\leq$ constraints (called slack variable) and subtract a nonnegative variable from $\geq$ constraints (called surplus variable). 
For example: By introducing the slack variable $y\geq 0$ in the inequality $ax \leq b$, we can convert it to $ax + y = b$ and by introducing the surplus variable $y'\geq 0$ in the inequality $a'x' \geq b'$, we can convert it to $a'x' = b' + y'$.
- Replace any unconstrained or negative variable $x$ with the difference of two new variables $x^{+}-x^{-}$ where $x^{+}, x^{-} \geq 0$.


---

## Convexity
### Convex Combinations
**$\lambda_{1}v_{1} + \lambda_{2}v_{2} + ... \lambda_{n}v_{n}$ is a convex combination of points $v_{1}, v_{2} ... v_{n}$ if $\lambda_{1} + \lambda_{2} + ... \lambda_{n} = 1$ and $\lambda_{i} \geq 0$ for $1 \leq i \leq n$.**
### Convex Set
**A set $S$ in $R^{n}$ is convex if it contains all the convex combinations of the points in $S$.**
### Some useful points
- The intersection of convex sets is a **convex set**. (easy to prove)
- A **hyperplane** is a subspace whose dimension is one less than that of its ambient space (surrounding space). For instance, a line cutting across a plane.
-  A **half-space** is either of the two parts into which a hyperplane divides a space.
-  A **half space** can be represented by the linear inequality $a_{1}x_{1} + a_{2}x_{2} + ... + a_{n}x_{n} \geq b$. It is a convex set.
- The intersection of a set of half spaces and is called a **polyhedron**. If it is finite, it is called a **polytope**.
- A **face** is an intersection of finitely many hyperplanes.
- For $n$ variables, a face of dimension $n-1$ (in one hyperplane) is called a **facet** and a face of dimension $0$ (in $n$ hyperplanes) is called a **vertex**.
- **Every point in a polytope can be expressed as a convex combination of its vertices.**
>*A rough intuitive proof can be thought using induction. For 1 vertex (dim V = 0) its trivially true, we assume it true for dim V = k. When we add extend the polytope to the next dimension (dim V = k+1), we consider lines passing through a new points in the polytope. These lines will intersect the faces of the polytope at two points. Now, any point on the face can be expressed as a convex combination of respective vertices (as dim V = k). Hence, the new points can eventually be expressed as a convex combination of the vertices.*

<p align="center"> <img src="https://i.imgur.com/14M5VSo.jpg" width="150">
&nbsp;&nbsp;&nbsp;<img src="https://i.imgur.com/k6CoZqi.jpg" width="250"><img src="https://i.imgur.com/UKyla2v.jpg" width="300"></p>

---
## Geometric view
### Theorem
**At least one of the points where the objective function is minimal is a vertex.**

### Proof
Let $x^{*}$ be the minimum. As each point in a polytope is a convex combination of the vertices $v_{1}, v_{2} ... v_{t}$, we have
$x^{*} = \lambda_{1}v_{1} + \lambda_{2}v_{2} + ... \lambda_{t}v_{t}$ where $\lambda_{1} + \lambda_{2} + ... \lambda_{t} = 1$

and the objective function value at optimality can be expressed as 
$cx^{*} = \lambda_{1}(cv_{1}) + \lambda_{2}(cv_{2}) + ... \lambda_{t}(cv_{t})$  where c is transpose of coefficient vector and $x^{*}$ and $v_{i}$ s are position vectors of the concerned points. 

Now, let us assume that the minimum is not at a vertex, i.e.,
$cx^{*} < cv_{i}$ for $1 \leq i \leq t$

Therefore,

**$cx^{*}$ $=$ $\lambda_{1}cv_{1} + \lambda_{2}cv_{2} + ... \lambda_{t}cv_{t}$ $>$ $\lambda_{1}cx^{*} + \lambda_{2}cx^{*} + ... \lambda_{t}cx^{*} = (\lambda_{1} + \lambda_{2} + ... \lambda_{t})cx^{*} = cx^{*}$ which is a contradiction.**

Hence, $x^{*}$ must be one of the $v_{i}$ where $1 \leq i \leq t$.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://i.imgur.com/ZfAFaeg.jpg" width="300"> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://i.imgur.com/fFsADYl.jpg" width="300">

---

# Simplex Method
### Intuition
Goal: **To solve a linear program**

For a set of linear equations, **a basic solution** can be thought of as a solution where $x_{1}~,...x_{m}$ take the values $b_{1},...b_{m}$ respectively and all other x~i~ s are assigned the value $0$ as shown below:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.imgur.com/2qYemMc.jpg" width="400" height="200">


Useful Points:
- A **Basic Feasible Solution (BFS)** essentially requires $b_{i}$'s to be non negative as for a linear program, all the $x_{i}$'s need to take non negative values.
- An **optimal solution is located at a vertex**.
- A **vertex is a BFS**.
- We can move from one BFS to a neighbouring BFS.
- We can detect whether a BFS is optimal.
- From any BFS, we can move to a BFS with a better cost.

Use in Linear Programs:
- For a linear program, we first express it in its canonical form and add corresponding slack variables to convert the $\leq$ inequalities into equalities. We modify the objective function so that we need to minimise it.
- *To get a basic solution, we essentially select $m$ $x_{i}$'s and put them on the LHS and express them in terms of the other variables (called non basic variables) using Gaussian Elimination.*
- *Then, we assign them the values of the constants on the RHS and the put the rest $x_{i}$'s as 0 to get a BFS (vertex) of the linear program.*
> *It is easy to see why this gives a vertex as it is the single point that intersects with all m constraints on the boundary.*

### Naive algorithm
The naive algorithm for this method would be to iterate over all the vertices and get the optimal solution. However, there will be **$^{n} c_{m}$** such possible combinations which does not work well computationally for large cases.

### Algorithm
Goal: **To minimise the objective function while satisfying the constraints.**

*The key idea of the simple algorithm is to move from one BFS to a better BFS till we reach the optimal (smallest value of objective function). We are guaranteed to find an optimal due to the convexity of the solution space.*

So, **the basic steps we would take to move from one BFS to another BFS given we are at an BFS already is as follows**:
- Select a non basic variable with negative coefficient in the objective function since assigning a positive value to it will drive the objective function down - entering variable
- Inroduce this variable in the basis by removing a basic variable from the equation where the coefficient of the entering variable is negative - leaving variable
- Perform Gaussian Elimination to get to the form of a basic solution and do the same with the objective function by expressing it in terms of only the non basic variables.
- **When we reach a point where objective function is of the form $c_{0}+\sum c_{i}x_{i}$ where all $c_{i} \geq 0$, we have reached the optimal solution. This is true because we cannot bring down the objective function any further since assigning a positive value to any of the variables will increase the objective function.**

#### Things to keep in mind:

- For the entering variable, we the swap the variable in the equation with minimum 
$\frac{b_i}{ -(\text{coeff of entering variable})}$ so that we ensure that $b_{i}$ s remain non-negative.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.imgur.com/oEeQEGz.jpg" width="300"><img src="https://i.imgur.com/J4AnbZF.jpg" width="300">

- If at a point we find, $b_{i} = 0$, we might get stuck between BFS with same value. To ensure termination in this situation, we can use the following methods:-
     * Bland Rule - Selecting the first entering variable lexicographically
     * Lexicographic pivoting rule 
     * Perturbation methods  

- If at a point we see that the variable we have chosen to insert in the basis has no negative coefficient in the constraint equations, it means that we can drive the value of the variable arbitarily low without violating any of the non negativity constraints. This implies that the linear program is unbounded and hence impractical.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://i.imgur.com/tVnRl5Y.jpg" width="300">

---
### Matrix representation
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://i.imgur.com/H5cjUTX.jpg" width="600" height="300">

### Simplex algorithm in matrix notation
1. Consider $Ax = b$
2. Choose m linearly independent columns $A_{B}$ 
$Ax = A_{B}x_{B} + A_{N}x_{N} = b$
$A_{B}x_{B} = b - A_{N}x_{N}$
$x_{B} = A^{-1}_{B}b - A^{-1}_{B}A_{N}x_{N}$
$x_{B} = b^{'} - A^{'}_{N}x_{N}$
3. $x_{B}$ is feasible if $b' \geq 0$
4. The matrix $A_{B}$ is called a basis.
5. Cost for basis B is 
$cx$ 
$= c_{B}x_{B} + c_{N}x_{N}$
$= c_{B}(A^{-1}_{B}b - A^{-1}_{B}A_{N}x_{N}) + c_{N}x_{N}$ 
$= c_{B}A^{-1}_{B}b + (c_{N} - c_{B}A^{-1}_{B}A_{N})x_{N} + (c_{B} - c_{B}A^{-1}_{B}A_{N})x_{B}$
$= c_{B}A^{-1}_{B}b + (c - c_{B}A^{-1}_{B}A)x$

6. We define $\prod = c_{B}A^{-1}_{B}$ and call it the simplex multiplier.
Therefore, $cx = \prod b + (c - \prod A)x$

7. $\prod b$ is the optimal solution when $c \geq \prod A$.

#### Proof:
$cx = \prod b + (c - \prod A)x$
We have already seen that the coefficients of all the variables being non-negative guarantees that we have reached the optimal solution. Hence in the case of optimal solution, 
$(c - \prod A) \geq 0 \implies c \geq \prod A$ 

Now, let $\prod b = c_{0}^{*}$
Let $y$ be a feasible solution. Then, we have $Ay = b$.
Hence, $cy \geq \prod Ay = \prod b = c_{0}^{*}$. 


### Converting to tableau
Basically, **in comparison to what we did earlier, we bring the non basic variables too on the LHS and send constant $c_{0}$ in $z$ to RHS whose value will ultimately give the optimal value for $z$.** Then we make a table out of it to ease our calculations.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://i.imgur.com/lAlHRsh.jpg" width="600" height="350">


### Simplex algorithm using tableau

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://i.imgur.com/Pvp7Wbo.jpg" width="600" height="350">

When we see that all coefficients in the first row are non-negative, we have reached the optimal solution. In the given case, it is $\frac{9}{2}$.

---
# Duality
### Primal and dual
We define the primal and dual as follows:

#### Primal
**Minimise $cx$ subject to $Ax \geq b$ and $x_{j} \geq 0$ where $1 \leq j \leq n$ on the m constraints.**

#### Dual
**The dual of the primal mentioned above is as follows:
Maximise $yb$ subject to $yA \leq c$ and $y_{i} \geq 0$ where $1 \leq i \leq m$ on the n constraints.**

<img src="https://i.imgur.com/9svi8Sl.jpg" width="550" height="350">

### Weak Duality Theorem
For every minimisation linear program, value of objective function of primal is always greater than that of the dual.
For every maximisation linear program, value of objective function of primal is always less than that of the dual.
**Proof:** 
Let x and $\prod_{0}$ be feasible solutions to the primal and dual respectively where the primal is a minimisation linear program. We have,
$c \geq \prod_{0} A  \implies cx \geq \prod_{0} Ax \geq \prod_{0} b$.
Similarly, we can prove the result for maximisation linear program.

### Strong Duality Theorem
**If the primal has an optimal solution, then the dual has an optimal solution with the same cost.**
#### Proof:
- By weak duality theorem, we can say that since the primal has a feasible solution, dual cannot be unbounded.
- Now, we already know that for the primal, in the optimal condition, $c \geq \prod A$ where $\prod$ is the simplex multiplier of the primal, which is hence a feasible solution of the dual.
- Now, from what we have seen for the primal, let $x^{*}$ be the optimal solution of primal. Then we have the associated $x^{*}_{B} = A_{B}^{-1} b$.
The dual has a feasible solution, $y^{*} = \prod = c_{B}A^{-1}_{B}$ from previous result. Hence, $y^{*}b = c_{B}A^{-1}_{B}b = c_{B}x^{*}$
Now, we can say that a feasible solution of the dual has the same cost as that of the optimal solution of the primal and since the cost the dual is always $\leq$ cost of the primal, this is indeed the optimal value of the dual too.

### Interpretation and use
#### Bounding
##### Can we find an upper bound to the objective function we want to maximise (maximal case)?
- Yes, we can try to make a linear combination of the constraints so that each coefficient in this combination is greater than the coefficient of the corresponding variable in the objective function. The value of this combination will be an upper bound to the objective function as  $\forall i$ $x_{i} \geq 0$ (non-negativity constraint).
<img src="https://i.imgur.com/ATfmvrs.jpg" width="310" height="300"> <img src="https://i.imgur.com/6LfYT5T.jpg" width="310" height="300"><img src="https://i.imgur.com/94Z8OBv.jpg" width="70" height="300">
- We then try to minimise the value of this combination which can be treated as an objective function with the value of constraints of the original LP as the decision variables. (Does this ring any bells?)
- Yes, it is basically minimising the dual's objective function.
  
#### Complementary Slackness
The primal being: minimise $cx$ subject to $Ax \geq b$ and $x_{j} \geq 0$ where $1 \leq j \leq n$ on the m constraints, 
and the dual being: maximise $yb$ subject to $A^{T}y \leq c$ and $y_{i} \geq 0$ where $1 \leq i \leq m$ on the n constraints.
Let $x^{*}$ and $y^{*}$ be the optimal solutions to the primal and the dual respectively. The following conditions are necessary and sufficient for the optimality of $x^{*}$ and $y^{*}$:
- **$\sum_{j=1}^n a_{ij}$ $x_{j}^{*} = b_{i}$ or $y_{i}^{*} = 0$ for $1 \leq i \leq m$**, and
- **$\sum_{i=1}^m a_{ji}$ $y_{i}^{*} = c_{i}$ or $x_{j}^{*} = 0$ for $1 \leq j \leq n$**

**Proof:**
Using the fact that $x^{*T}A^{T} y^{*}$ is a $1 × 1$ matrix, $x^{*T}A^{T} y^{*} = (x^{*T}A^{T} y^{*})^{T} = y^{*T}Ax^{*}$. 

Now by Strong Duality, as $x$ and $y$ are both optimal to their respective LPs,

$cx^{*} = by^{*}$ &nbsp; and &nbsp; $cx^{*} = x^{*T} c \geq x^{*T}A^{T} y^{*} = y^{*T}Ax^{*} \geq y^{*T}b = by^{*}$ (from weak duality)
$\Longleftrightarrow$ $cx^{*} = x^{*T} c = x^{*T}A^{T} y^{*} = y^{*T}Ax^{*} = y^{*T}b = by^{*}$
$\Longleftrightarrow x^{*}(A^{T}y^{*} − c) = 0$ &nbsp; and &nbsp; $y^{*}(b − Ax^{*}) = 0$. 


We note that $0 = x^{*}(A^{T} y^{*} − c) = \sum_{j=1}^{n}(x^{*}_{j}(\sum_{i=1}^{m}a_{ji}y^{*}_{i}-c_{j}))$

But, $x^{*}_{j}\geq 0$ and $(\sum_{i=1}^{m}a_{ji}y^{*}_{i}-c_{j})\geq0 \implies$ $x^{*}_{j}(\sum_{i=1}^{m}a_{ji}y^{*}_{i}-c_{j}) = 0$ for each $j = 1, 2, . . . , n$

Similarly, we deduce that $y^{*}(b − Ax^{*}) = 0$ iff $y^{*}_{i}(\sum_{j=1}^{n}a_{ij}x^{*}_{j}-b_{i}) = 0$ for each $i = 1, 2, . . . , m$

#### A sample demonstration:
Let the primal be to maximise $4x_{1}+x_{2}+5x_{3}+3x_{4}$ with the following constraints:

$\begin{bmatrix} 1\\5\\-1 \end{bmatrix}x_{1}$ + $\begin{bmatrix} -1\\1\\2 \end{bmatrix}x_{2}$ + $\begin{bmatrix} -1\\3\\3 \end{bmatrix}x_{3}$ + $\begin{bmatrix} 3\\8\\-5 \end{bmatrix}x_{4} \leq \begin{bmatrix} 1\\55\\3 \end{bmatrix}$

After including the dual variables $y_{i}$ 's,

$\begin{bmatrix} \boxed{1y_{1}}\\5y_{2}\\-1y_{3} \end{bmatrix}\boxed{x_{1}}$ + $\begin{bmatrix} \boxed{-1y_{1}}\\1y_{2}\\2y_{3} \end{bmatrix}\boxed{x_{2}}$ + $\begin{bmatrix} \boxed{-1y_{1}}\\3y_{2}\\3y_{3} \end{bmatrix}\boxed{x_{3}}$ + $\begin{bmatrix} \boxed{3y_{1}}\\8y_{2}\\-5y_{3} \end{bmatrix}\boxed{x_{4}} \leq \begin{bmatrix} \boxed{1y_{1}}\\55y_{2}\\3y_{3} \end{bmatrix}$
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$\geq 4$ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$\geq 1$ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $\geq 5$ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $\geq 3$

which at optimality,

$= \begin{bmatrix} 4x_{1}\\1x_{2}\\5x_{3}\\3x_{4} \end{bmatrix} \leq  \begin{bmatrix} \boxed{1x_{1}}\\\boxed{-1x_{2}}\\\boxed{-1x_{3}}\\\boxed{3x_{4}} \end{bmatrix}\boxed{y_{1}}$ + $\begin{bmatrix} 5x_{1}\\1x_{2}\\3x_{3}\\8x_{4} \end{bmatrix}y_{2}$ + $\begin{bmatrix} -1x_{1}\\2x_{2}\\3x_{3}\\-5x_{4} \end{bmatrix}y_{3}$
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$\leq 1$ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$\leq 55$ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$\leq 3$ 

However, the extreme l.h.s and the extreme r.h.s are the same thing which forces equality in the intermediate inequalities. Thus,

$\begin{bmatrix} \boxed{1y_{1}}\\5y_{2}\\-1y_{3} \end{bmatrix}\boxed{x_{1}}$ + $\begin{bmatrix} \boxed{-1y_{1}}\\1y_{2}\\2y_{3} \end{bmatrix}\boxed{x_{2}}$ + $\begin{bmatrix} \boxed{-1y_{1}}\\3y_{2}\\3y_{3} \end{bmatrix}\boxed{x_{3}}$ + $\begin{bmatrix} \boxed{3y_{1}}\\8y_{2}\\-5y_{3} \end{bmatrix}\boxed{x_{4}}  = \begin{bmatrix} \boxed{1y_{1}}\\55y_{2}\\3y_{3} \end{bmatrix}$

which makes it clear that $1y_{1}x_{1} - 1y_{1}x_{2} - 1y_{1}x_{3} + 3y_{1}x_{4} = 1y_{1} \implies y_{1} (x_{1} - x_{2} - x_{3} + 3x_{4}) = 0$ and similar deductions for other rows.

- This basically means that for **the optimal case, either the value of a decision variable of primal is $0$ or the corresponding slack in the dual is $0$**. 
- We can use this property of complementary slackness to get optimal solution to the primal in cases like when we have the optimal solution and the values of dual variables. An example is given below where $x_{j}$'s and $u_{i}$'s are the primal's and dual's decision variables respectively and $s_{i}$'s and $e_{j}$'s are slack variables in primal and dual respectively.
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.imgur.com/5pQ439x.jpg" width="600" height="250">
- We can also use it to check the optimality of a solution.

#### Duality in the Tableau
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.imgur.com/SWRldN3.jpg" width="600" height="300">

---
## Strong LP Duality
### Separating Hyperplane Theorem

### Farkas Lemma

### LP strong duality from Farkas lemma

---
# Some Applications
## Max-flow / Min-cut Problem
### What is the max-flow problem?
A **flow network** is a directed graph $G$ with vertices $V$ and edges $E$ ech of which is associated with a capacity $c_{e}$, which assigns each edge $e∈E$ a non-negative integer value. One of its verties is a source and one is sink.

A **flow** in a flow network can be denoted by a function $f$, that assigns each edge $e$ a non-negative integer value, namely the flow. The function has to satisfy the following conditions:

- **Capacity constraint:** The flow of an edge cannot exceed its capacity.
$f(e)≤c_{e}$
- **Conservation Constraint:** The sum of the incoming flow of a vertex $u$ has to be equal to the sum of the outgoing flow of $u$ except for the source and sink vertices. Hence,
$∑_{(v,u)∈E}f((v,u))=∑_{(u,v)∈E}f((u,v))$
- The source vertex s has only an outgoing flow, and the sink vertex t has only incoming flow.
- Also for s being the source and t being the sink, the ourgoing flow of source should be equal to the incoming flow of sink which is basically the flow of the network.
$∑_{(s,u)∈E}f((s,u))=∑_{(u,t)∈E}f((u,t))$
- Without loss of generality, we take that $s$ has no incoming edges and $t$ has no outgoing edges.

**A maximal flow is a flow with the maximal possible value. Our goal is to find this maximal flow of a flow network.**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://i.imgur.com/pUXBlHH.jpg" width="500" height="200">

The maximal flow of 10 in the above flow network is as follows:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://i.imgur.com/C1VGyuu.jpg" width="500" height="200">

### Why care about it?
The maximum flow problem can model the routing of traffic through a transportation network, data packets through a network, or oil through a pipeline network. Problems like bipartite matching and image segmentation reduce to the maximum flow problem.

### Algorithms
- #### Naive Greedy Algorithm
    - The greedy approach to the max-flow problem can be to start with zero flow for all edges and greedily proceed from one flow to the next to send more flow on some path from s to t. However, this might not give the max-flow all the time.

> ***Concept of Residual Graph***
> *Residual edges are edges in the opposite direction of flow of original edge from one vertex to other with initial flow/capacity of 0/0. Residual graph contains residual edges along with the original edges.*
> *An augmenting path is a path of edges in the residual graph with unused capacity greater than zero from the source s to the sink t.*
> *Every augmenting path has a bottleneck value and that can be used to augment the flow.*
> *However, till now we could only increase flow in an edge using the bottleneck but we might need to undo the update by decreasing the flow for augmenting paths that donot give max flow. We do this by using residual edges in a way that increasing the negative flow in an residual edge decreases flow in original edge.*


- #### Ford-Fulkerson Algorithm
    - In this algorithm, we find augmenting paths through the residual graph repeatedly and augment the flow till no more augmenting paths can be found.
    - Time complexity of this algorithm is $O(maxflow * E)$ since we run a loop while there is an augmenting path and in worst case, we may add 1 unit flow in every iteration. 
```
initialize f_e = 0 for all e ∈ E
repeat
search for an s-t path P in the current residual graph G_f such that
every edge of P has residual capacity > 0
// takes O(|E|) time using BFS or DFS
if no such path then
    halt with current flow {f_e} e ∈ E
else
    diff = min e ∈ P (e’s residual capacity in G_f )
    // augment the flow f using the path P
    for all edges e of G whose corresponding forward edge is in P
    increase f_e by diff
    for all edges e of G whose corresponding reverse edge is in P
    decrease f_e by diff
```

### Max-flow as a Linear Program
1. **Decision variables:** We trying to solve for a flow, specifically, the amount $f_e$ of flow on each edge e. So, our variables are just ${f_e }$ where $e ∈ E$.
2. **Constraints:** Taking $\delta^-(v)$ as the set of all incoming edges into vertex $v$ and $\delta^+(v)$ as set of outgoing edges from vertex $v$, conservation constraints and capacity constraints which can be written as
    - $\sum_{e \in \delta^-(v)} f_e - \sum_{e \in \delta^+(v)} f_e = 0$ for every vertex $v\neq s, t$. 
    -  $f_e ≤ u_e$ for every edge $e ∈ E$. 
    - We also need to add nonnegativity constraints: $f_e \geq 0$ for every edge $e ∈ E$. 
    - It can be seen that every one of these $2m + n − 2$ constraints (where $m = |E|$ and $n = |V|$) is linear.
3. **Objective function:** max $\sum_{e \in \delta^+(s)}f_e$ which is also a linear function.

### Max-flow min-cut theorem
Max-flow min-cut theorem states that in a flow network, the maximum amount of flow passing from the source s to the sink t is equal to the total weight of the edges in a minimum cut. 

### Max-flow/min-cut theorem via duality
> **Concept of s-t cut**
> An s-t cut $C = (S, T)$ is a partition of vertex set V of the flow network such that $s ∈ S$ and $t ∈ T$. Basically, an s-t cut is a division of the vertices of a network into two parts, with the source in one part and the sink in the other.
> Capacity of an s-t cut: The capacity of an s–t cut is defined as the sum of the capacity of each edge in the set containing edges going from the source's side to the sink's side. 

#### Minimum s-t Cut Problem  
Finding an s-t cut whose the capacity of is minimal. It can also be thought of as finding the the smallest total weight of the edges which if removed, would disconnect the source from the sink. The s-t cut found is called the minimum cut.

#### Duality view of the theorem
* **The Primal**
    To model the primal, we take path decompositions this time rather than flows. So, the decision variables, denoting the flow through the path p, have the form $f_p$ , where $p$ is a path from source s to sink t. Let $P$ denote the set of all such paths. Now, there is no need to explicitly state the conservation constraints. Thus our problem now is to maximise $\sum_{p \in P}$ $f_{p}$ subject to:
    - Total flow on $e \leq$ capacity of e, i.e., $\sum_{p \in P: e \in p} f_{p} \leq c_{e}$ $\forall e \in E$
    - $f_{p} \geq 0$ $\forall$ $p \in P$
    - In matrix form, it is maximising *$1^{T}f$* where *$1$* is the p dimensional vector $\begin{bmatrix}1\\1\\.\\.\\.\\1\end{bmatrix}$ 
    subject to $Af\leq c$ and $f \geq 0$.
* **The Dual**
    The dual can written as follows in matrix form taking $l_e$ as the decision variables:
    
    - Minimise $c^{T}l$ subject to $\sum_{e \in p} l_e \geq 1$ $\forall$ $p \in P$ and $l_e \geq 0$ $\forall e \in E$
    
    - The key observation is that every s-t cut corresponds to a feasible solution to this dual linear program. To see this, we fix a cut $(S, T)$, with $s ∈ S$ and $t ∈ T$, and set 
    $l_e = 1$ if $e \in \delta^{+}(S)$ where $\delta^{+}(S)$ is the set of outgoing edges from S and $0$ otherwise.
    
    - We can see that the constraints are satisfied as $l_e$ being either 0 or 1 satisfies the non-negativity constraint and as every s-t path must cross the cut $(S, T)$ at some point since it starts in S and ends in T. Thus, every s-t path has at least one edge e with $l_e = 1$.
    
    - The objective function is hence $\sum_{e \in p}c_el_e =$ $\sum_{e \in \delta^{+}(S)} c_e =$ capacity of $(S,T)$.
    - Hence, the dual is basically minimising the capacity of s-t cuts in the flow network. This is the minimal-cut problem.
    - However, we have considered onlt the 0-1 solutions of the dual. So, by weak duality we can say that,
     $max-flow = optimal \; value \; of \; primal \leq optimal \; value \; of \; dual \leq min-cut$.
     - By strong duality, if a max-flow value exists, then it must be equal to the min-cut value.
---
    
## Minimum-Cost flow
### What is the mininum cost flow problem?
In the minimum cost flow problem, we have:
- a directed graph $G = (V, E)$
- a source $s ∈ V$ and sink $t ∈ V$
- a target flow value $d$
- a nonnegative capacity $u_e$ for each edge $e ∈ E$
- a real-valued cost per flow unit $c_e$ for each edge $e ∈ E$.

The goal is to compute a flow $f$ with value $d$ i.e., pushing $d$ units of flow from s to t, subject to the usual conservation and capacity constraints so that the cost is minimised, i.e., $min \sum _{e \in E} c_e f_e$ where $e \in E$.

It is to be noted that for each edge $e$ that since $c_e$ is “per-flow unit” cost, the contribution of edge $e$ to the overall cost with $f_e$ units of flow is $c_e f_e$.

### Minimum Cost Flow as Linear Program
Taking the minimum cost flow problem as defined earlier, the linear objective function is simply

$min \sum _{e \in E} c_e f_e$ 

where $e \in E$ as defined earlier.
We need to add the following constraint to the usual capacity and conservation constraints:

$\sum_{e \in \delta^{+}(s)} f_e = d$ where d is the target flow value. 

The flexibility of linear programs can seen from the fact that if we want to impose a lower bound, say $l_e$ on the amount of flow on each edge e, in addition to the usual upper bound $u_e$, we can just replace $“f_e ≥ 0”$ by $f_e ≥ l_e$ .

---
## Linear Regression
<img src="https://i.imgur.com/RqzAmls.jpg" width="600" height="250">



### The problem
We want to fit a line to data points. It is frequently used in basic problems in machine learning. Formally, the input consists of $m$ data points $p_1 , . . . , p_m ∈ R^d$, each with $d$ real-valued “features” (coordinates). These coordinates can be thought of as properties of your data, say for $d = 3$, it can be the profession, income and numer of years of experience of an employee.

The input also consists of a “label”:  $l_i ∈ R$ for each point $p_i$ which for example, can be the expenditure of the employee. Thus, $l_i$ can be thought of as the output we get from our data model for a particular point $p_i$.
The $p_i$'s and $l_i$'s are fixed and are parts of the input. They are not decision variables. 

Informally, the goal is to express the $l_i$'s as effectively as possible as a linear function of the $p_i$'s. 
Thus, the goal is to compute a linear function $h : R^d → R$ such that $h(p_i) ≈ l_i$ for every data point $p_i$.

Computing a “best-fit” linear function finds common use in prediction and data analysis. 
- In the case of prediction, we use labeled data to identify a linear function $h$ that, at least for data points whose labels are known, does a good job of predicting the label $l_i$ from the feature values $p_i$. 
Then we hope that this linear function also makes accurate predictions for other data points for which the label is not already known, i.e., assuming it "generalises". 
- In the case of data analysis, the goal is to understand the relationship between each feature of the data points and the labels, and also the relationships between the different features. As a simple example, it might be interesting to know when one of the $d$ features is much more strongly correlated with the label $l_i$ than any of the others. 

### Linear Regression as LP
We now show that computing the best line, for one definition of “best-fit” reduces to linear programming. 

Every linear function $h : R^d → R$ has the form:

$h(z) = \sum_{j=1} ^{d} a_jz_j + b$

for coefficients $a_1, . . . , a_d$ and intercept $b$.

Hence, the coefficients $a_j$ and b can be taken as the decision variables.

#### But, what’s our objective function? 
Clearly if the data points are colinear we would want to compute the line that passes through all of them. But that is an ideal case, so we must compromise between how well we approximate different points.

- For a given choice of $a_1, . . . , a_d$, $b$, we define the error on point $i$ as:

    $E_{p_i} (a,b) = | (\sum_{j=1} ^{d} a_jp_{ij} - b) - l_i |$

    where $p_{ik}$ is the $k^{th}$ feature in point $p_i$.
    Geometrically, when $d = 1$, we can think of each $(p_{i1} , p_i)$ as a point on the plane and $E_{p_i} (a,b)$ being the vertical distance between this point and the computed line.

- Here, we consider the objective function of minimizing the sum of errors:
    $min_{(a, b)} \sum_{i=1}^{m} E_{p_i}(a,b)$

- Consider the problem of choosing a, b to minimize the objective function defined earlier. The problem is that this is not a linear program. The source of nonlinearity is the absolute value sign $||$. However, these absolute values can be made linear with a simple trick.
    
    The trick is to introduce extra variables $e_1 , . . . , e_m$, one data point. The intent is for $e_i$ to take on the value $E_{p_i} (a, b)$ using the result $|x| = max(x, −x)$. 

- Hence, we add two constraints for each data point:

    $e_i \geq [(\sum_{j=1} ^{d} a_jp_{ij} - b) - l_i]$ and $e_i \geq -[(\sum_{j=1} ^{d} a_jp_{ij} - b) - l_i]$

    We change the objective function to $min \sum_{i=1}^m e_i$ with decision variables $e_1 , . . . , e_m , a_1 , . . . , a_d , b$.


- The key point is: at an optimal solution to this linear program, $e_i$ must be equal to $E_{p_i} (a, b)$ for every data point $p_i$.

    Feasibility of the solution already implies that $e_i ≥ E_{p_i} (a, b)$ for every $p_i$. And if $e_i > E_{p_i} (a, b)$ for some $p_i$, then we can decrease $e_i$ slightly so that the constraints still hold, to obtain a superior feasible solution. 

- Hence, we conclude that an optimal solution to this linear program, we get the line minimizing the sum of errors.

---
## Linear Classifiers
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.imgur.com/yPuc2C8.jpg" width="400" height="250">

### The problem
We are looking for a binary function (from $R^d \rightarrow {0, 1}$). For example, data points could represent images, and we want to know which ones have a dog and which ones don’t.
Formally, the input consists of m data points $p^1, . . . , p^m ∈ R^d$ labeled $1$ and $m'$ data points $q^1 , . . . , q^m ∈ R^d$ labeled $0$. 

The goal is to compute a linear function $h(z) = \sum_{j=1} a_j z_j + b$ (from $R^d$ to $R$), where $z_k$ represents the $k^{th}$ feature of $z$, such that $h(p^i) > 0$ and $h(q^i) < 0$.

Geometrically, we are looking for a hyperplane in $R^d$ such that all positive points are on one side and all negative points on the other.  Such a hyperplane can be used for predicting the labels of other unlabeled points by checking which side of the hyperplane it is on. If there is no such hyperplane, an algorithm should correctly report this fact.

### Linear classifier as LP
The only issue with this problem is that the constraints are strict inequalities, which is not allowed in linear programs.

However, we can easily add an extra decision variable. 
- The new decision variable $\delta$ represents the “margin” by which the hyperplane satisfies the constraints. So, we have the linear program:

    $max$ $\delta$  subject to

    $\sum_{j=1}^da_jp_j^i+b-\delta \geq 0$ for points $p^1, p^2, ... p^m$
    $\sum_{j=1}^da_jp_j^i+b+\delta \leq 0$ for points $q^1, q^2, ... q^{m'}$
    
    which is a linear program with decision variables $\delta, a_1, ... , a_d ,b$. 
- If the optimal solution to this linear program has strictly positive objective function value, then the values of the variables $a_1 , . . . , a_d, b$ define the desired separating hyperplane. 
     not, then there is no such hyperplane. 
- We conclude that computing a linear classifier reduces to linear programming.

---
## Hungarian Algorithm


---
## The Minimax theorem for two-player zero-sum games
### Some useful definitions
- **Game**: A game is defined as a conflict involving gains and losses between two or more opponents who need to follow some formal rules.
- **Mixed strategies**: A collection of moves along with a corresponding set of weights which are followed based on certain probabilities in the playing of a game.
- **Payoff Matrix**: An $m×n$ matrix which gives the possible outcome of a two-person zero-sum game when player A has $m$ possible moves and player B has $n$ possible moves.

### Zero-Sum Games 
A zero-sum game is a game in which players make payments only to each other. In such a game, one player's loss is the other player's gain, so the total amount of "resource" available remains constant. 
- A zero-sum game is specified by a real-valued matrix $m × n$ payoff matrix $A$. 
- One player, the row player, picks a row. The other (column) player picks a column. 
- Rows and columns are also called strategies.
- By definition, the entry $a_{ij}$ of the matrix $A$ is the row player’s payoff when the person chooses row $i$ and the column player chooses column $j$. The column player’s payoff in this case is defined as $−a_{ij}$, hence the term “zero-sum”.
- Hence, $a_{ij}$ is the amount that the column player pays to the row player in the outcome $(i, j)$.
- Thus, the row and column players prefer bigger and smaller numbers, respectively.

For example, the matrix $A$ for rock-paper-scissors game is as follows:

$\begin{bmatrix}  & \text{Rock} & \text{Paper} & \text{Scissors} \\ \text{Rock} & 0 & 1 & -1 \\ \text{Paper} & -1 & 0 & 1 \\ \text{Scissors} & 1 & -1 & 0 \end{bmatrix}$ 

### The Minimax Theorem
Let A be a $m × n$ matrix representing the payoff matrix for a two-person, zerosum game. Then the game has a value and there exists a pair of mixed strategies which are optimal for the two players.

### LP Duality and Minimax

---

# Linear Programming Algorithms
## Simplex Method Revisited
- **Worst-Case Running Time**
    The simplex method is **very fast in practice**, and routinely solves linear programs with hundreds of thousands of variables and constraints. However, the **worst-case running time** of the simplex method is **exponential in the input size**.

    We see that the number of vertices of a feasible region can be exponential in the dimension (e.g., $2^n$ vertices of the $n$-dimensional hypercube). However, it is very hard to construct a linear program where the simplex method actually visits all of the vertices of the feasible region. 

- **Average-Case and Smoothed Running Time**
    It has been shown that the simplex method (with a suitable pivot rule) runs in polynomial time “on average” with respect to various distributions over linear programs.
    
    The simplex method has been proved to have polynomial “smoothed complexity”. Smoothed analysis is a hybrid of worst-case and average-case analyses. It measures the expected performance of algorithms under slight random perturbations of worst-case inputs. 
    
    The main result is that, for every initial linear program, in expectation over the perturbed version of the linear program, the running time of simplex is polynomial in the input size.
    
    On the whole, bad examples for the simplex method are rare and hence simplex method remains the most commonly used linear programming algorithm in practice.

---
## Ellipsoid Method
- **Worst-Case Running Time**
    The ellipsoid method was originally proposed as an algorithm for non-linear programming. Later it was proved that for linear programs, the algorithm is actually guaranteed to run in polynomial time. This was the **first polynomial-time algorithm for linear programming**.

    The ellipsoid method is **very slow in practice** — usually multiple orders of magnitude slower than the fastest methods. But, how can a polynomial-time algorithm be so much worse than the exponential-time simplex method? There are two reasons behind that. 
    - The **degree in the polynomial** bounding the ellipsoid method’s running time is **huge** (like 4 or 5, depending on the implementation).
    - The **performance** of the ellipsoid method **on “typical cases”** is generally **close to its worst-case performance**. This is in sharp contrast to the simplex method, which almost always solves linear programs in time far less than its worst-case (exponential) running time.
    
- **Separation Oracles**

    The ellipsoid method is useful for proving theorems, especially for establishing that other problems are worst-case polynomial-time solvable, and thus are at least efficiently solvable in principle.
    
    **The ellipsoid method can solve some linear programs with n variables and an exponential (in n) number of constraints in time polynomial in n.**
    
    - But, doesn’t it take exponential time just to read all of the constraints? 
    For other linear programming algorithms, yes but not for the ellipsoid method as it doesn’t need an explicit description of the linear program. **It just  needs is a helper subroutine** known as a **separation oracle**. 
    
    **The responsibility of a separation oracle is to take as input an allegedly feasible solution x to a linear program, and to either verify feasibility (if feasible) or produce a constraint violated by x (otherwise).** Of course, the separation oracle should also run in polynomial time.
    
- **An Example:** How could one possibly check an exponential number of constraints in polynomial time? 
    We have actually already seen some examples of this. 
    - For example, we have seen **the dual of the path-based linear programming formulation of the max-flow problem**:
    - Minimise $c^{T}l$ subject to $\sum_{e \in p} l_e \geq 1$ $\forall p \in P$ and $l_e \geq 0$ $\forall e \in E$
Here, $P$ denotes the set of s-t flow paths in the flow network (with edge capacities $c_e$).
    - Since a graph can have an exponential number of s-t paths, this linear program has a potentially exponential number of constraints. But, it has a polynomial-time separation oracle. The key observation is: at least one constraint is violated iff 
    $min_{p \in P}$ $\sum_{e \in p} l_e < 1$
    - Thus, the separation oracle is just Dijkstra’s algorithm in this case. Notice that the above condition is equivalent to the following: 
    - Given an allegedly feasible solution ${l_e}$ where $e∈E$ to the linear program, 
    the separation oracle checks if each $l_e$ is  non-negative (if $l_e < 0$, it returns the violated constraint $l_e ≥ 0$). 
    - If the solution passes this test, then the separation oracle runs Dijkstra’s algorithm to compute a shortest s-t path, using the $l_e$ s as (non-negative) edge lengths. 
    - If the shortest path has length at least 1, then all of the constraints are satisfied and the oracle reports “feasible”.
If the shortest path $p^{∗}$ has length less than 1, then it returns the violated constraint $\sum_{e∈p^*}$  $l_e ≥ 1$. 
    - Thus, we can solve the above linear program in polynomial time using the ellipsoid method.
- **How the Ellipsoid Method Works**
    The first step in ellipsoid method is to reduce the optimization problem to a feasibility problem. Basically,if the objective is max $c^T x$, we replace the objective function by the constraint $c^T x ≥ k$ for some target objective function value $k$. 
    If we can solve this feasibility problem in polynomial time, then we solve the original optimization problem using binary search on the target objective function value $k$.
    
    *The algorithm:*
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://i.imgur.com/i8FonKW.jpg" width="350" height="250">

    - The ellipsoid method maintains at all times an ellipsoid which is guaranteed to contain the entire feasible region. 
    - It is initialised with a huge sphere to ensure the invariant at initialization. 
    - It then invokes the separation oracle on the center of the current ellipsoid. 
    - If the ellipsoid center is feasible, then the problem is solved. 
    - If not, the separation oracle produces a constraint satisfied by all feasible points that is violated by the ellipsoid center.
    - Geometrically, the feasible region and the ellipsoid center fall on opposite sides of the corresponding halfspace boundary. 
    - Thus we know we can recurse on the appropriate half-ellipsoid. 
    - Before recursing, however, the ellipsoid method recreates a new ellipsoid that contains this half-ellipsoid which is now guaranteed to contain the entire feasible region. 
    - It can be shown that the volume of the current ellipsoid is guaranteed to reduce at a certain rate after each iteration, and this yields a polynomial bound on the number of iterations required. 
    - The algorithm stops when the current ellipsoid is so small that it cannot possibly contain a feasible point (given the precision of the input data).
    
- Thus we see how this method solves linear programs even with an exponential number of constraints. It never works with an explicit description of the constraints. It just generates constraints with every iteration. Since it terminates in a polynomial number of iterations, it generates only a polynomial number of constraints.

---
## Interior Point Methods
Till now we have seen the simplex and the ellipsoid methods. The simplex method works “along the boundary” of the feasible region, and the ellipsoid method works “outside in”. Interior point methods work “inside out”. 
There are many genres of interior-point methods, beginning with Karmarkar’s algorithm. 

- The main idea is, instead of maximizing the given objective $c^T x$, we maximize
    $c^T x − λ · f$(distance between x and boundary),
    where $λ ≥ 0$ is a parameter and f is a “barrier function”. 
- In general, a barrier function is a continuous function whose value on a point increases to infinity as the point approaches the boundary of the feasible region. 
    Here, the barrier function $f$ goes to $+∞$ as distance between x and boundary goes to $0$ (e.g., $log(\frac{1}{z})$). 
- We initialise with a large value of $λ$ so as to simplify the problem. For example, when $f (z) = log(\frac{1}{z}$) , the solution is the center of the feasible region, and can be computed using methods like the Newton’s method. 
- Then we gradually decrease the value of $λ$ and track the corresponding optimal point along the way. When $λ = 0$, the optimal point is an optimal solution to the linear program, as required.

Interior-point methods consist of many algorithms that run in polynomial time in the worst case many interior-point methods also compete with the simplex method in practice. For example, one of Matlab’s LP solvers uses an interior-point algorithm.

---
