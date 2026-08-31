# Mathematical Foundation for AI & Data Science
### Complete Study Notes — UE25MA25242A / UE19MA251 (PES University)
### Vector Spaces, Subspaces, Rank, Linear Independence, Basis, Linear Transformations, and Special Matrices

---

## Table of Contents

1. [Vector Spaces](#1-vector-spaces)
2. [Subspaces](#2-subspaces)
3. [Echelon Form, RREF, Pivot & Free Variables, Rank](#3-echelon-form-rref-pivot--free-variables-rank)
4. [Linear Independence and Dependence](#4-linear-independence-and-dependence)
5. [Basis and Dimension](#5-basis-and-dimension)
6. [The Four Fundamental Subspaces](#6-the-four-fundamental-subspaces)
7. [Linear Transformations](#7-linear-transformations)
8. [Special Linear Transformations: Rotation (Q), Projection (P), Reflection (H)](#8-special-linear-transformations-rotation-q-projection-p-reflection-h)
9. [Quick-Reference Formula Sheet](#9-quick-reference-formula-sheet)

---

## 1. Vector Spaces

### 1.1 Definition

A **real vector space** $V$ is a nonempty set of objects called **vectors**, together with two operations — **vector addition** and **scalar multiplication** — satisfying the following axioms for all $u, v, w \in V$ and scalars $c_1, c_2 \in \mathbb{R}$:

**Closure axioms:**
- **(I)** If $u, v \in V$, then $u + v \in V$ (closed under addition)
- **(II)** If $c \in \mathbb{R}$ and $u \in V$, then $cu \in V$ (closed under scalar multiplication)

**Algebraic axioms:**
- (a) $u + v = v + u$ (commutative law)
- (b) $u + (v + w) = (u + v) + w$ (associative law)
- (c) There is a unique zero vector $0 \in V$ such that $0 + u = u + 0 = u$ (additive identity)
- (d) For each $u$ there is a unique vector $-u$ such that $u + (-u) = (-u) + u = 0$ (additive inverse)
- (e) $c_1(u + v) = c_1u + c_1v$ (distributive law over vector addition)
- (f) $(c_1 + c_2)u = c_1u + c_2u$ (distributive law over scalar addition)
- (g) $c_1(c_2 u) = (c_1c_2)u$ (associativity of scalar multiplication)
- (h) $1 \cdot u = u$ (multiplicative identity, $1 \in \mathbb{R}$)

> **Intuition:** A vector space is any collection where you can "add" and "scale" objects, and the results always stay inside the collection, obeying ordinary arithmetic-like rules. It doesn't have to be arrows in space — it can be matrices, polynomials, functions, etc.

### 1.2 Examples of Vector Spaces (from the course)

1. $\mathbb{R}$ — the set of all real numbers, with ordinary addition and multiplication.
2. $\mathbb{C}$ — the set of all complex numbers, with ordinary addition and scalar multiplication.
3. $R_n(x)$ — the set of all polynomials with real coefficients of degree $\le n$.
4. $\mathbb{R}^n = \{(x_1, x_2, \dots, x_n) : x_i \in \mathbb{R}\}$ — the set of all real $n$-tuples.
5. The set of all $m \times n$ matrices, with matrix addition and scalar multiplication.

Other important named spaces:
- $\mathbb{R}^2 = \{(x,y) : x, y \in \mathbb{R}\}$
- $\mathbb{R}^3 = \{(x,y,z) : x,y,z \in \mathbb{R}\}$
- $\mathbb{R}^\infty = \{(x_1, x_2, \dots) : x_i \in \mathbb{R}\}$ (infinite sequences)

**Key idea:** In any vector space, we can take **linear combinations** — i.e., add any two vectors and multiply any vector by a scalar, and stay inside the space.

### 1.3 Worked Example — Verifying/Disproving a Vector Space

**Example 3 (from course):** Show that the set of integers $\mathbb{Z}$, with ordinary addition and multiplication *by a real number*, is **NOT** a vector space.

**Solution:** Multiplying an integer by a real number may not produce an integer.
Let $x = -2$. Multiplying by $\sqrt{3}$ gives $-2\sqrt{3} \approx -3.46$, which is **not an integer**.
So $\mathbb{Z}$ is not closed under scalar multiplication by reals ⇒ **not a vector space** (over $\mathbb{R}$).

**Example (from course) — First-quadrant set:**
$$V = \left\{ \begin{bmatrix} x \\ y \end{bmatrix} : x \ge 0, y \ge 0,\ x,y \in \mathbb{R} \right\}$$
Is this a vector space under the usual operations?

**Solution:** Take $u = (1, 1) \in V$ and the scalar $c = -1$. Then $cu = (-1,-1) \notin V$ since both components are negative.
So $V$ is **not closed under scalar multiplication** ⇒ **not a vector space**. (It is *closed under addition*, but that alone is not enough.)

### 1.4 Additional Examples (for practice)

**Example A:** Is $W = \{(x, y, z) \in \mathbb{R}^3 : z = 0\}$ (the xy-plane) a vector space under the usual operations?
*Check:* $(0,0,0) \in W$ ✓. If $(x_1,y_1,0), (x_2,y_2,0) \in W$, sum is $(x_1+x_2, y_1+y_2, 0) \in W$ ✓. $c(x,y,0) = (cx,cy,0) \in W$ ✓. **Yes**, it is a vector space (in fact a subspace of $\mathbb{R}^3$).

**Example B:** Is the set of all $2\times2$ **invertible** matrices a vector space (with matrix addition)?
*Check:* The zero matrix (which must be in every vector space) is **not invertible**, so it isn't even in the set. **Not a vector space.**

**Example C:** Is $V = \{ f : \mathbb{R} \to \mathbb{R} \mid f \text{ is continuous} \}$, with pointwise addition and scalar multiplication, a vector space?
*Check:* Sum of continuous functions is continuous; a scalar multiple of a continuous function is continuous; the zero function is continuous. **Yes**, this is a vector space (it's the space $C(\mathbb{R})$ used heavily in functional analysis / ML kernel theory).

### 1.5 Practice Questions — Vector Spaces

1. Determine whether $W = \{(x,y) \in \mathbb{R}^2 : xy \ge 0\}$ is a vector space under the usual operations. *(Hint: check closure under addition using $(1,0)$ and $(0,1)$.)*
2. Is the set of all polynomials of degree **exactly** 3 (not $\le 3$) a vector space? *(Hint: think about what happens when you add $t^3 + 1$ and $-t^3$.)*
3. Show that the set of all $2\times 2$ matrices with determinant $= 1$ is **not** a vector space.
4. Is $\mathbb{R}^+$ (positive reals) a vector space if "addition" is defined as $x \oplus y = xy$ and "scalar multiplication" as $c \odot x = x^c$? *(This is a classic trick question — work through all 8 axioms.)*
5. Prove using the axioms that in any vector space, $0 \cdot u = 0$ (the zero scalar times any vector gives the zero vector).

---

## 2. Subspaces

### 2.1 Definition

A nonempty subset $W$ of a vector space $V$ is called a **subspace** of $V$ if $W$ is itself a vector space under the same operations of vector addition and scalar multiplication defined on $V$.

**Three-point subspace test** (sufficient conditions — you don't need to re-check all 8 axioms):

(i) $0 \in W$ (the zero vector always belongs to a subspace)
(ii) If $u, v \in W$, then $u + v \in W$ (closed under addition)
(iii) If $c$ is a scalar and $u \in W$, then $cu \in W$ (closed under scalar multiplication)

> If any one of these three fails, $W$ is **not** a subspace.

### 2.2 Worked Examples

**Problem 7 (from course):** Describe the subspace of $\mathbb{R}^3$ spanned by:

**(1)** The two vectors $(1,1,-1)$ and $(-1,-1,1)$.

$$A = \begin{pmatrix}1 & -1\\ 1 & -1 \\ -1 & 1\end{pmatrix} \xrightarrow[R_3 \to R_3+R_1]{R_2 \to R_2 - R_1} \begin{pmatrix}1&-1\\0&0\\0&0\end{pmatrix}$$

Only **one** pivot column ⇒ vectors are **linearly dependent** ⇒ span is a **line in $\mathbb{R}^3$**.

**(2)** The vectors $(0,1,1), (1,1,0), (0,0,0)$.

$$A = \begin{pmatrix}0&1&0\\1&1&0\\1&0&0\end{pmatrix} \to \begin{pmatrix}1&1&0\\0&1&0\\1&0&0\end{pmatrix} \to \begin{pmatrix}1&1&0\\0&1&0\\0&-1&0\end{pmatrix} \to \begin{pmatrix}1&1&0\\0&1&0\\0&0&0\end{pmatrix}$$

**Two** pivot columns ⇒ vectors are **linearly independent** (the zero vector contributes nothing) ⇒ span is a **plane in $\mathbb{R}^3$**.

### 2.3 Additional Examples

**Example A:** Is $W = \{(x,y,z) : x + y + z = 1\}$ a subspace of $\mathbb{R}^3$?
*No* — the plane doesn't pass through the origin, so $0 \notin W$. (This is an *affine* set, not a subspace.)

**Example B:** Is $W = \{(x, y) : y = x^2\}$ a subspace of $\mathbb{R}^2$?
*No* — take $u=(1,1), v=(2,4) \in W$. $u+v = (3,5)$, but $5 \ne 3^2=9$, so $u + v \notin W$. Fails closure under addition.

**Example C:** Is $W = \{A \in M_{2\times2} : \text{trace}(A) = 0\}$ a subspace of the vector space of all $2\times2$ matrices?
*Yes* — the zero matrix has trace 0; trace is additive, $\text{tr}(A+B)=\text{tr}(A)+\text{tr}(B)$; and $\text{tr}(cA) = c\,\text{tr}(A)$. All three conditions hold.

### 2.4 Practice Questions — Subspaces

1. Is $W = \{(x,y,z) \in \mathbb{R}^3 : x = 2y = 3z\}$ a subspace of $\mathbb{R}^3$? Find a basis if so.
2. Is the set of all upper-triangular $3\times3$ matrices a subspace of $M_{3\times3}$?
3. Is the union of the $x$-axis and $y$-axis a subspace of $\mathbb{R}^2$? *(Hint: try adding $(1,0)$ and $(0,1)$.)*
4. Describe (as a line, plane, or all of $\mathbb{R}^3$) the subspace spanned by $(1,2,3), (2,4,6), (0,0,1)$.
5. If $W_1$ and $W_2$ are both subspaces of $V$, prove that $W_1 \cap W_2$ is also a subspace of $V$. Is $W_1 \cup W_2$ always a subspace? Give a counterexample if not.

---

## 3. Echelon Form, RREF, Pivot & Free Variables, Rank

### 3.1 Definitions

A rectangular matrix is in **echelon form** $U$ if:
(i) All zero rows are below the nonzero rows.
(ii) Each pivot (leading nonzero entry) lies to the **right** of the pivot in the row above (staircase pattern).
(iii) All entries in a column **below** a pivot are zero.

The matrix is in **Row-Reduced Echelon Form (RREF)** $R$ if, additionally:
(iv) Every pivot equals **1** and is the **only** nonzero entry in its column.

**Rank of a matrix:** $\rho(A)$ (or simply $r$) = number of nonzero rows in the echelon form $U$ of $A$ = number of pivots.
*Note:* If $A$ is $m\times n$, then $r \le \min(m,n)$.

**Pivot variables vs. Free variables** (for solving $Ax=0$ or $Rx=0$):
- **Pivot variables** correspond to columns **with** a pivot.
- **Free variables** correspond to columns **without** a pivot.
- Free variables can take *any* value; pivot variables are then determined in terms of them.
- The general solution to $Ax=0$ is a linear combination of **special solutions**, one per free variable (obtained by setting that free variable = 1 and all others = 0).

### 3.2 Worked Example — Echelon Form

$$A = \begin{bmatrix}1&2&3\\0&1&6\\0&0&1\end{bmatrix}=U \text{ (echelon form)} \qquad A=\begin{bmatrix}1&0&0&10\\0&1&0&5\\0&0&1&2\end{bmatrix}=R \text{ (RREF)}$$

### 3.3 Worked Example — Rank

$$\text{Ex 1: } A=\begin{bmatrix}1&2&3\\2&4&5\end{bmatrix}\sim\begin{bmatrix}1&2&3\\0&0&-1\end{bmatrix};\ \rho(A)=2$$
$$\text{Ex 2: } A=\begin{bmatrix}1&2&3\\2&4&6\end{bmatrix}\sim\begin{bmatrix}1&2&3\\0&0&0\end{bmatrix};\ \rho(A)=1$$

### 3.4 Worked Example — Pivot/Free Variables

Given $Rx = 0$ where:
$$\begin{bmatrix}1&2&0&-1\\0&0&1&4\\0&0&0&0\end{bmatrix}\begin{bmatrix}u\\v\\w\\y\end{bmatrix}=\begin{bmatrix}0\\0\\0\end{bmatrix}$$
Columns 1 and 3 have pivots ⇒ **pivot variables** are $u, w$. Columns 2, 4 have no pivots ⇒ **free variables** are $v, y$.
From the rows: $u + 2v - y = 0 \Rightarrow u = -2v + y$, and $w + y = 0 \Rightarrow w = -y$.

**Special solutions** (set one free variable = 1, other = 0):
$$x = v\begin{bmatrix}-2\\1\\0\\0\end{bmatrix} + y\begin{bmatrix}1\\0\\-1\\1\end{bmatrix}$$

### 3.5 Worked Problem Set — Echelon Form, RREF, Rank (Problem 1)

For each matrix, find Echelon Form $U$, RREF $R$, and Rank:

| Matrix | Echelon Form $U$ | RREF $R$ | Rank |
|---|---|---|---|
| $A=\begin{bmatrix}1&3\\-2&2\end{bmatrix}$ | $\begin{bmatrix}1&3\\0&8\end{bmatrix}$ | $\begin{bmatrix}1&0\\0&1\end{bmatrix}$ | 2 |
| $B=\begin{bmatrix}2&6&-2\\3&-2&8\end{bmatrix}$ | $\begin{bmatrix}2&6&-2\\0&-22&22\end{bmatrix}$ | $\begin{bmatrix}1&0&2\\0&1&-1\end{bmatrix}$ | 2 |
| $C=\begin{bmatrix}2&-2&4\\4&1&-2\\6&-1&2\end{bmatrix}$ | $\begin{bmatrix}2&-2&4\\0&5&-10\\0&0&0\end{bmatrix}$ | $\begin{bmatrix}1&0&0\\0&1&-2\\0&0&0\end{bmatrix}$ | 2 |
| $D=\begin{bmatrix}-2\\3\\1\end{bmatrix}$ | $\begin{bmatrix}-2\\3\\1\end{bmatrix}$ | $\begin{bmatrix}1\\0\\0\end{bmatrix}$ | 1 |
| $E=\begin{bmatrix}-2&3&1\end{bmatrix}$ | $\begin{bmatrix}-2&3&1\end{bmatrix}$ | $\begin{bmatrix}1&-\tfrac32&-\tfrac12\end{bmatrix}$ | 1 |

### 3.6 Worked Problem — Gauss Elimination with Pivot/Free Variables (Problem 2)

Solve: $x+2y+3z=9,\ 2x-2z=-2,\ 3x+2y+z=7$.

$$[A:b] = \begin{bmatrix}1&2&3&|&9\\2&0&-2&|&-2\\3&2&1&|&7\end{bmatrix}\xrightarrow[R_3\to R_3-3R_1]{R_2\to R_2-2R_1}\begin{bmatrix}1&2&3&|&9\\0&-4&-8&|&-20\\0&-4&-8&|&-20\end{bmatrix}$$
$$\xrightarrow{R_3\to R_3-R_2}\begin{bmatrix}1&2&3&|&9\\0&-4&-8&|&-20\\0&0&0&|&0\end{bmatrix}\xrightarrow[R_1\to R_1-2R_2]{R_2\to -\tfrac14R_2}\begin{bmatrix}1&0&-1&|&-1\\0&1&2&|&5\\0&0&0&|&0\end{bmatrix}$$

**Pivot variables:** $x, y$. **Free variable:** $z$. Rank of coefficient matrix = 2. Infinitely many solutions.
Let $z=t$. From row 2: $y = 5-2t$. From row 1: $x=t-1$.
$$\boxed{(x,y,z)=(-1+t,\ 5-2t,\ t),\quad t\in\mathbb{R}}$$

### 3.7 Worked Example — Parametric Matrix (Problem with parameter $c$)

For every $c$, find $R$ and special solutions to $Ax=0$ where $A = \begin{bmatrix}1-c&2\\0&2-c\end{bmatrix}$.

**Case $c=1$:** $A=\begin{bmatrix}0&2\\0&1\end{bmatrix}\xrightarrow{R_2\to R_2-2R_1... }$ — reduces to $U=\begin{bmatrix}0&2\\0&0\end{bmatrix}$, then $R=\begin{bmatrix}0&1\\0&0\end{bmatrix}$. From $Rx=0$: $y=0$, $x$ is free. Special solution: $x=\begin{bmatrix}1\\0\end{bmatrix}$.

**Case $c=2$:** $A\sim\begin{bmatrix}-1&2\\0&0\end{bmatrix}=U \xrightarrow{R_1\to -R_1}\begin{bmatrix}1&-2\\0&0\end{bmatrix}$. Here $y$ is free. Solving $x-2y=0$ gives special solution $x=\begin{bmatrix}2\\1\end{bmatrix}$.

### 3.8 Additional Examples

**Example A — Rank via Echelon Form:**
$$A=\begin{bmatrix}1&2&1\\2&4&3\\3&6&5\end{bmatrix}$$
$R_2 \to R_2-2R_1,\ R_3\to R_3-3R_1$: $\begin{bmatrix}1&2&1\\0&0&1\\0&0&2\end{bmatrix}$, then $R_3\to R_3-2R_2$: $\begin{bmatrix}1&2&1\\0&0&1\\0&0&0\end{bmatrix}$.
Rank $= 2$. Pivot columns: 1, 3. Free variable: column 2 (i.e., $y$).

**Example B — Special Solutions:**
Find special solutions to $Ax=0$ for $A=\begin{bmatrix}1&3&0&2\\0&0&1&4\end{bmatrix}$ (already in RREF).
Pivot variables: $x_1, x_3$. Free: $x_2, x_4$.
$x_1 = -3x_2-2x_4,\ x_3=-4x_4$.
Special solutions: $\begin{bmatrix}-3\\1\\0\\0\end{bmatrix}, \begin{bmatrix}-2\\0\\-4\\1\end{bmatrix}$.

### 3.9 Practice Questions

1. Find the echelon form, RREF, and rank of $A = \begin{bmatrix}1&2&-1\\2&4&1\\-1&-2&2\end{bmatrix}$.
2. For $A = \begin{bmatrix}1&2&2&2\\2&4&6&8\\3&6&8&10\end{bmatrix}$, find pivot and free variables, and the special solutions to $Ax=0$.
3. Solve by Gauss elimination, identifying pivot/free variables: $x+y+z=6,\ 2x-y+z=3,\ x-2y=-6$.
4. For what value(s) of $k$ does $A=\begin{bmatrix}1&2\\3&k\end{bmatrix}$ have rank 1? Rank 2?
5. True or False: A square matrix always has rank equal to its number of rows. Justify with an example.

---

## 4. Linear Independence and Dependence

### 4.1 Concept

A set of vectors $v_1, v_2, \dots, v_n$ is **linearly independent** if the only solution to
$$c_1v_1 + c_2v_2 + \cdots + c_nv_n = 0$$
is the **trivial** one, $c_1=c_2=\cdots=c_n=0$.
If a **non-trivial** combination (not all $c_i = 0$) also gives $0$, the vectors are **linearly dependent**.

**Test:** Write the vectors as columns of a matrix $A$. Row-reduce.
- If **every** column has a pivot (rank = number of vectors), the vectors are **independent**.
- If **some** column has **no** pivot, the vectors are **dependent**.

**Key fact:** A set of $n$ vectors in $\mathbb{R}^m$ must be linearly dependent if $n > m$ (more vectors than dimensions ⇒ automatically dependent).

### 4.2 Worked Problem 1 — Independence Test

**(a)** Vectors $(1,3,2), (2,1,3), (3,2,1)$.
$$A=\begin{pmatrix}1&2&3\\3&1&2\\2&3&1\end{pmatrix}\xrightarrow[R_3\to R_3-2R_1]{R_2\to R_2-3R_1}\begin{pmatrix}1&2&3\\0&-5&-7\\0&-1&-5\end{pmatrix}\xrightarrow{R_3\to R_3-\frac15R_2}\begin{pmatrix}1&2&3\\0&-5&-7\\0&0&-\frac{18}{5}\end{pmatrix}$$
All 3 columns have pivots ⇒ $\rho(A)=n=3$ ⇒ **linearly independent**.

### 4.3 Worked Problem 2 — Dependence Test

**(a)** Vectors $(1,-3,2), (2,1,-3), (-3,2,1)$: only 2 pivots found ⇒ $\rho(A)=2 < n=3$ ⇒ **linearly dependent**.
**(b)** Vectors $(1,1), (2,3), (1,2)$: 3 vectors in $\mathbb{R}^2$ ⇒ automatically **dependent** (since $n=3>m=2$).
**(c)** Vectors $(1,1,0,0)^T, (1,0,1,0)^T, (0,0,1,1)^T, (0,1,0,1)^T$: a non-trivial combination of these sums to zero ⇒ **linearly dependent**.

### 4.4 Worked Problem — Proof-Style Question

**Problem:** If $w_1, w_2, w_3$ are independent vectors, show that $v_1 = w_2-w_3,\ v_2=w_1-w_3,\ v_3=w_1-w_2$ are **dependent**.

**Solution:** Since $w_1,w_2,w_3$ independent: $\alpha w_1+\beta w_2+\gamma w_3=0 \Rightarrow \alpha=\beta=\gamma=0$ only.

Check $c_1v_1+c_2v_2+c_3v_3=0$:
$$c_1(w_2-w_3)+c_2(w_1-w_3)+c_3(w_1-w_2)=0$$
$$\Rightarrow w_1(c_2+c_3) + w_2(c_1-c_3) + w_3(-c_1-c_2) = 0$$

By independence of $w_1,w_2,w_3$: $c_2+c_3=0,\ c_1-c_3=0,\ -c_1-c_2=0$
$$\Rightarrow c_2=-c_3,\ c_1=c_3,\ \Rightarrow c_1=c_3=-c_2$$

These are satisfied by **any** value (e.g. $c_1=1,c_2=-1,c_3=1$), not just zero ⇒ **$v_1,v_2,v_3$ are dependent**.
(Geometrically, $v_1+v_2+v_3 = 0$ always — this is the key relation.)

### 4.5 Additional Examples

**Example A:** Are $(1,0,1), (0,1,1), (1,1,0)$ independent?
$$A=\begin{pmatrix}1&0&1\\0&1&1\\1&1&0\end{pmatrix}\xrightarrow{R_3\to R_3-R_1}\begin{pmatrix}1&0&1\\0&1&1\\0&1&-1\end{pmatrix}\xrightarrow{R_3\to R_3-R_2}\begin{pmatrix}1&0&1\\0&1&1\\0&0&-2\end{pmatrix}$$
All 3 pivots ⇒ **independent**.

**Example B:** Are $(2,4), (1,2)$ independent?
Second vector $= \tfrac12 \times$ first vector — a clear non-trivial dependency ⇒ **dependent** (they're parallel, span only a line).

### 4.6 Practice Questions

1. Determine whether $(1,2,-1), (2,1,1), (0,-3,3)$ are linearly independent.
2. For what value of $k$ are $(1,2,3), (2,4,k)$ linearly dependent?
3. If $u,v$ are linearly independent, show that $u+v$ and $u-v$ are also linearly independent.
4. Are the polynomials $1+t,\ 1-t,\ t^2$ linearly independent in $P_2$? *(Hint: write as coordinate vectors w.r.t. basis $\{1,t,t^2\}$.)*
5. Can 4 vectors in $\mathbb{R}^3$ ever be linearly independent? Explain why or why not.

---

## 5. Basis and Dimension

### 5.1 Definitions

A **basis** for a vector space (or subspace) $V$ is a set of vectors that:
1. **Spans** $V$ (every vector in $V$ is a linear combination of them), **and**
2. Is **linearly independent**.

The **dimension** of $V$ is the number of vectors in any basis of $V$ (all bases of the same space have the same size).

### 5.2 Worked Problem 3 — Basis/Dimension of Subspaces of $\mathbb{R}^4$

**(a) All vectors whose components are equal:**
$$\begin{pmatrix}x\\x\\x\\x\end{pmatrix} = x\begin{pmatrix}1\\1\\1\\1\end{pmatrix},\ x\in\mathbb{R}$$
**Basis:** $\{(1,1,1,1)^T\}$; **Dimension = 1**

**(b) All vectors whose components add to zero** ($x+y+z+t=0 \Rightarrow x=-y-z-t$):
$$\begin{pmatrix}-y-z-t\\y\\z\\t\end{pmatrix}=y\begin{pmatrix}-1\\1\\0\\0\end{pmatrix}+z\begin{pmatrix}-1\\0\\1\\0\end{pmatrix}+t\begin{pmatrix}-1\\0\\0\\1\end{pmatrix}$$
**Basis:** $\{(-1,1,0,0)^T,(-1,0,1,0)^T,(-1,0,0,1)^T\}$; **Dimension = 3**

### 5.3 Worked Problem 4

$V = \{(x_1,x_2,x_3,x_4)\in\mathbb{R}^4 : x_1-x_2+x_3-x_4=0\}$. Find basis and dimension.

$x_1=x_2-x_3+x_4 \Rightarrow$ vector $= x_2\begin{pmatrix}1\\1\\0\\0\end{pmatrix}+x_3\begin{pmatrix}-1\\0\\1\\0\end{pmatrix}+x_4\begin{pmatrix}1\\0\\0\\1\end{pmatrix}$

**Basis:** $\{(1,1,0,0)^T,(-1,0,1,0)^T,(1,0,0,1)^T\}$; **Dimension = 3**

### 5.4 Worked Problem 5 — Matrix Spaces (2×2)

**1) All diagonal matrices:**
$$\begin{pmatrix}a&0\\0&b\end{pmatrix}=a\begin{pmatrix}1&0\\0&0\end{pmatrix}+b\begin{pmatrix}0&0\\0&1\end{pmatrix}$$
**Basis** $=\left\{\begin{pmatrix}1&0\\0&0\end{pmatrix},\begin{pmatrix}0&0\\0&1\end{pmatrix}\right\}$; **Dimension = 2**

**2) All symmetric matrices** ($A^T=A$):
$$\begin{pmatrix}a&b\\b&c\end{pmatrix}=a\begin{pmatrix}1&0\\0&0\end{pmatrix}+b\begin{pmatrix}0&1\\1&0\end{pmatrix}+c\begin{pmatrix}0&0\\0&1\end{pmatrix}$$
**Dimension = 3**

**3) All skew-symmetric matrices** ($A^T=-A$):
$$\begin{pmatrix}0&a\\-a&0\end{pmatrix}=a\begin{pmatrix}0&1\\-1&0\end{pmatrix}$$
**Dimension = 1**

### 5.5 Worked Problem 6 — Polynomial Space

$P(a)$ of degree $\le 3$: **Basis** $=\{1,\ t,\ t^2,\ t^3\}$; **Dimension = 4**.
*(General rule: $P_n$ has basis $\{1,t,t^2,\dots,t^n\}$ and dimension $n+1$.)*

### 5.6 Additional Examples

**Example A:** Find a basis and dimension for $W=\{(x,y,z) : 2x-y+3z=0\}\subset\mathbb{R}^3$ (a plane through origin).
$y = 2x+3z \Rightarrow$ vector $= x(1,2,0) + z(0,3,1)$. **Basis:** $\{(1,2,0),(0,3,1)\}$; **Dimension = 2**.

**Example B:** Find a basis for the space of all $2\times3$ matrices.
Standard basis: the six matrices with a single $1$ in one position and $0$ elsewhere. **Dimension = 6**.

### 5.7 Practice Questions

1. Find a basis and dimension for the subspace of $\mathbb{R}^4$ consisting of vectors whose **first and last** components are equal.
2. Find a basis for the space of all upper-triangular $2\times2$ matrices. What is its dimension?
3. Show that $\{1+t, 1-t\}$ is a basis for the space of all linear polynomials $\{a+bt\}$.
4. If $\dim(V)=n$, prove that any $n$ linearly independent vectors in $V$ automatically form a basis.
5. Find the dimension of $W = \{(x,y,z,w) : x=y,\ z=w\}\subset \mathbb{R}^4$, and give a basis.

---

## 6. The Four Fundamental Subspaces

For an $m \times n$ matrix $A$ with rank $r = \rho(A)$:

| Subspace | Definition | Lives in | Dimension |
|---|---|---|---|
| **Column space** $C(A)$ | All linear combinations of the columns of $A$; the set of all outcomes $Ax$ | $\mathbb{R}^m$ | $r$ |
| **Null space** $N(A)$ | All $x$ satisfying $Ax=0$ | $\mathbb{R}^n$ | $n-r$ |
| **Row space** $C(A^T)$ | All linear combinations of the rows of $A$ (= column space of $A^T$) | $\mathbb{R}^n$ | $r$ |
| **Left null space** $N(A^T)$ | All $y$ satisfying $A^Ty=0$ | $\mathbb{R}^m$ | $m-r$ |

> **Note:** $N(A)$ and $C(A^T)$ are both subspaces of $\mathbb{R}^n$; $C(A)$ and $N(A^T)$ are both subspaces of $\mathbb{R}^m$.
> Also: $\dim C(A) = \dim C(A^T) = r = \text{rank}(A)$, and $\dim N(A) = n-r$, $\dim N(A^T) = m-r$. This is the **Rank–Nullity theorem**.

**Finding a basis for $N(A^T)$ (left null space):** apply row operations to $A$; the row-combinations that produce the **zero rows** of $U$ give the basis of the left null space — the scalars used in that combination are the components of the basis vector.

### 6.1 Simple Illustration

Let $A=\begin{bmatrix}1&2\\3&6\end{bmatrix}$. Then $m=n=2$, rank $r=1$.
1. $C(A)$ is the line through $(1,3)$
2. $C(A^T)$ is the line through $(1,2)$
3. $N(A)$ is the line through $(-2,1)$
4. $N(A^T)$ is the line through $(-3,1)$

### 6.2 Full Worked Example

Find the dimensions and a basis for the four fundamental subspaces of:
$$A=\begin{bmatrix}1&2&1&2\\1&2&1&3\\3&6&3&7\end{bmatrix}\sim\begin{bmatrix}1&2&1&2\\0&0&0&1\\0&0&0&0\end{bmatrix}=U$$
Rank $r=2$.

**Column space** — take the columns of $A$ corresponding to the pivot columns of $U$ (columns 1 and 4):
$$C(A) = \left\{c_1\begin{bmatrix}1\\1\\3\end{bmatrix}+c_2\begin{bmatrix}2\\3\\7\end{bmatrix} : c_1,c_2\in\mathbb{R}\right\},\quad \dim C(A) = 2$$

**Row space** — take the pivot rows of $U$ (or equivalently, express as combinations of the rows of $A$):
$$C(A^T) = \left\{c_1\begin{bmatrix}1\\2\\1\\2\end{bmatrix}+c_2\begin{bmatrix}0\\0\\0\\1\end{bmatrix}\right\},\quad \dim C(A^T)=2$$

**Null space** — solve $Ux=0$: $x+2y+z+2t=0$ and $t=0$ ⇒ $x=-2y-z,\ t=0$.
$$N(A)=\left\{y\begin{bmatrix}-2\\1\\0\\0\end{bmatrix}+z\begin{bmatrix}-1\\0\\1\\0\end{bmatrix}\right\},\quad \dim N(A)=2$$

**Left null space** — row-reducing $A$, Row2 $\to$ Row2 $-$ Row1 gives a zero-combination pattern; tracing back to the *original* rows of $A$: the combination $(-2)\cdot\text{Row}_1 + (-1)\cdot\text{Row}_2 + 1\cdot\text{Row}_3$ gives a zero row.
$$N(A^T) = \left\{c_1\begin{bmatrix}-2\\-1\\1\end{bmatrix}\right\},\quad \dim N(A^T) = 1$$

**Verification:** $\dim C(A)+\dim N(A) = 2+2 = 4 = n$ ✓, and $\dim C(A^T)+\dim N(A^T) = 2+1=3=m$ ✓.

### 6.3 Additional Example

Let $A=\begin{bmatrix}1&1&2\\2&2&4\end{bmatrix}$. Row reduce: $R_2\to R_2-2R_1$ gives $\begin{bmatrix}1&1&2\\0&0&0\end{bmatrix}$. Rank $=1$.
- $C(A)$: line through $(1,2)$ in $\mathbb{R}^2$, dimension 1.
- $N(A)$: solve $x+y+2z=0$; free vars $y,z$; basis $\{(-1,1,0),(-2,0,1)\}$, dimension 2 (checks: $n-r=3-1=2$ ✓).
- $C(A^T)$: spanned by $(1,1,2)$, dimension 1.
- $N(A^T)$: solve $A^Ty=0$; since $R_2-2R_1=0$ used coefficients $(-2,1)$, basis is $\{(-2,1)\}$, dimension 1 (checks: $m-r=2-1=1$ ✓).

### 6.4 Practice Questions

1. For $A=\begin{bmatrix}1&0&2\\0&1&1\end{bmatrix}$, find bases for all four fundamental subspaces and verify the rank-nullity relations.
2. If $A$ is $5\times3$ with rank 2, what are $\dim C(A)$, $\dim N(A)$, $\dim C(A^T)$, $\dim N(A^T)$?
3. Explain (in words) why $N(A)$ and $C(A^T)$ are **orthogonal complements** of each other in $\mathbb{R}^n$.
4. Find the left null space of $A=\begin{bmatrix}1&2\\2&4\\3&6\end{bmatrix}$.
5. Can $C(A) = \mathbb{R}^m$ for an $m\times n$ matrix with $n<m$? Explain using rank.

---

## 7. Linear Transformations

### 7.1 Definition

Let $A$ be a matrix of order $n$ (or more generally $m\times n$). When $A$ multiplies an $n$-dimensional vector $x$, it **transforms** $x$ into an $n$- (or $m$-) dimensional vector $Ax$. This happens for every $x \in \mathbb{R}^n$: the whole space $\mathbb{R}^n$ is **transformed / mapped** into $\mathbb{R}^m$ by $A$. We say $A$ **induces a transformation**.

A transformation $T$ on $\mathbb{R}^n$ is called **linear** if it satisfies the **rule of linearity**:
$$T(cx+dy) = c\,T(x) + d\,T(y) \quad \text{for all scalars } c,d \text{ and vectors } x,y$$

**Notes:**
1. If $T$ is linear then $T(0)=0$ (it preserves the origin). The **converse is not always true** — preserving the origin doesn't guarantee linearity.
2. If $A$ is $m\times n$, it induces a transformation from $\mathbb{R}^n$ to $\mathbb{R}^m$: $T_A(x) = Ax$.
3. Every linear system $Ax=b$ can be viewed as a linear transformation.

### 7.2 Geometric Examples

Let $x=(x,y)$ throughout.

| # | Matrix $A$ | $Ax$ | Effect |
|---|---|---|---|
| 1 | $\begin{bmatrix}c&0\\0&c\end{bmatrix}$ | $(cx,cy)$ | **Stretches/scales** every vector by factor $c$ (whole space expands or contracts) |
| 2 | $\begin{bmatrix}0&-1\\1&0\end{bmatrix}$ | $(-y,x)$ | **Rotates** every vector 90° counter-clockwise about the origin |
| 3 | $\begin{bmatrix}0&1\\1&0\end{bmatrix}$ | $(y,x)$ | **Reflects** every vector across the line $y=x$ (a permutation matrix) |
| 4 | $\begin{bmatrix}1&0\\0&0\end{bmatrix}$ | $(x,0)$ | **Projects** every vector onto the $x$-axis |

(Visually: stretching moves $(x,y)\to(cx,cy)$ along the same ray; 90° rotation sweeps $(x,y)\to(-y,x)$; reflection about $y=x$ swaps coordinates; projection onto the $x$-axis drops the $y$-component to 0.)

### 7.3 Checking Linearity — Worked Examples

Let $v=(v_1,v_2)$. Determine which are linear:
1. $T(v)=(v_2,v_1)$ — **Linear** ✓
2. $T(v)=(v_1,v_1)$ — as stated in the course slide, **not linear** *(students should double check this by applying the linearity test directly — see note below)*
3. $T(v)=(0,v_1)$ — **not linear**
4. $T(v)=(0,1)$ — **not linear** (constant, doesn't even satisfy $T(0)=0$)
5. $T(v)=(v_1,v_2)$ (identity) — **Linear** ✓

**Important note:** If a transformation preserves the origin ($T(0)=0$), it **may or may not** be linear — origin-preservation is necessary but not sufficient for linearity. Always verify $T(cx+dy)=cT(x)+dT(y)$ directly.

### 7.4 Linear Transformations on Function/Polynomial Spaces

**Example 1 — Differentiation:** $A = \dfrac{d}{dt}$ is linear. It takes $P_{n+1} \to P_n$. Column space = all of $P_n$; null space = $P_0$ (the 1-dimensional space of constants, since $\frac{d}{dt}(\text{constant})=0$).

**Example 2 — Integration:** $A = \int_0^t$ is linear. It takes $P_n \to P_{n+1}$. Column space is a subspace of $P_{n+1}$; null space is just the zero vector (integrating a nonzero polynomial never gives zero — unless we fix the constant appropriately).

**Example 3 — Multiplication by a fixed polynomial:** Multiplying by $3+4t$ is linear. If $p(t) = a_0+a_1t+\cdots+a_nt^n$, then $Ap(t) = (3+4t)p(t) = 3a_0 + \cdots + 4a_nt^{n+1}$. This sends $P_n \to P_{n+1}$.

### 7.5 Matrix Representation of Transformations

If we know $Ax$ for each vector in a **basis**, we know $Ax$ for **every** vector in the space (by linearity). 

**Example:** If $x=(1,0) \to (1,3,5)$ and $(0,1) \to (3,7,0)$, then
$$A = \begin{bmatrix}1&3\\3&7\\5&0\end{bmatrix}$$
(The images of the basis vectors become the **columns** of $A$.)

The *same* transformation $A$, viewed via a **different basis** $(1,1)$ and $(2,1)$, also satisfies $A(1,1)=(4,10,5)$ and $A(2,1)=(5,13,10)$ — consistent with the same $A$ (basis choice doesn't change the transformation, only how we compute it).

**Worked example — Matrix of differentiation:**
Basis for $P_3$: $u=1,\ v=t,\ w=t^2,\ z=t^3$. Derivatives: $0, 1, 2t, 3t^2$, i.e. $Au=0,\ Av=u,\ Aw=2v,\ Az=3w$.
$$A_{\text{diff}} = \begin{bmatrix}0&1&0&0\\0&0&2&0\\0&0&0&3\end{bmatrix}_{3\times4}$$

**Worked example — Matrix of integration** (brings $P_2$ back to $P_3$):
$$A_{\text{int}} = \begin{bmatrix}0&0&0\\1&0&0\\0&1/2&0\\0&0&1/3\end{bmatrix}_{4\times3}$$
Observe: $A_{\text{diff}}$ is a **left inverse** of $A_{\text{int}}$ (differentiating after integrating recovers the original).

### 7.6 Additional Examples

**Example A:** Is $T(x,y) = (2x-y, x+3y)$ linear? Check: $T(cx_1+dx_2, cy_1+dy_2) = (2(cx_1+dx_2)-(cy_1+dy_2),\ \ldots) = c(2x_1-y_1,x_1+3y_1)+d(2x_2-y_2,x_2+3y_2) = cT(x_1,y_1)+dT(x_2,y_2)$. **Linear**, matrix $=\begin{bmatrix}2&-1\\1&3\end{bmatrix}$.

**Example B:** Is $T(x,y)=(x+1, y)$ linear (a translation)? $T(0,0)=(1,0)\ne(0,0)$, so it fails the necessary condition immediately. **Not linear.**

### 7.7 Practice Questions

1. Determine if $T(x,y,z) = (x-y, 2z, x+y+z)$ is linear. If so, find its matrix.
2. Is $T(x,y)=(xy, x)$ linear? Justify using the linearity rule.
3. Find the matrix that represents "reflection through the origin" (i.e., $T(x)=-x$) in $\mathbb{R}^2$.
4. If $T:\mathbb{R}^2\to\mathbb{R}^2$ is linear with $T(1,0)=(2,5)$ and $T(0,1)=(-1,3)$, find $T(4,-2)$.
5. Show that the trace function $\text{tr}: M_{n\times n} \to \mathbb{R}$ is a linear transformation.

---

## 8. Special Linear Transformations: Rotation (Q), Projection (P), Reflection (H)

These three families of $2\times2$ matrices are the classic geometric linear transformations, all parametrized by an angle $\theta$.

### 8.1 Rotation Matrices $Q_\theta$

**Formula:**
$$Q_\theta = \begin{bmatrix}\cos\theta & -\sin\theta\\ \sin\theta & \cos\theta\end{bmatrix}$$

**Derivation (from basis vectors):** Using the standard basis $e_1=(1,0),\ e_2=(0,1)$ of $\mathbb{R}^2$:
$$Q_\theta e_1 = \begin{pmatrix}\cos\theta\\\sin\theta\end{pmatrix}, \qquad Q_\theta e_2 = \begin{pmatrix}\cos(90°+\theta)\\\sin(90°+\theta)\end{pmatrix} = \begin{pmatrix}-\sin\theta\\\cos\theta\end{pmatrix}$$
These two images become the **columns** of $Q_\theta$.

**Key properties:**
- **Composition:** $Q_\theta \cdot Q_\psi = Q_{\theta+\psi}$ (rotating by $\psi$ then by $\theta$ = rotating by $\theta+\psi$). Proven by direct multiplication using angle-sum formulas for sine/cosine.
- Consequently $Q_\theta \cdot Q_\theta = Q_{2\theta}$, i.e. $Q_\theta^2 = Q_{2\theta}$.
- **Inverse:** $Q_\theta \cdot Q_{-\theta} = Q_0 = I$ (the identity), so $Q_\theta^{-1} = Q_{-\theta}$ (rotating backward undoes the rotation).
- Rotation **preserves all angles between vectors as well as their lengths** — so it is a **reversible** (invertible) process.
- This transformation is **invertible** since $Q_\theta$ has an inverse ($Q_{-\theta}$); a rotation through $-\theta$ brings back the original vector.

### 8.2 Projection Matrices $P_\theta$

**Formula (projection onto the line through the origin at angle $\theta$):**
$$P_\theta = \begin{bmatrix}\cos^2\theta & \cos\theta\sin\theta \\ \cos\theta\sin\theta & \sin^2\theta\end{bmatrix}$$

**Derivation (geometric):** For the $\theta$-line with unit vector direction $(\cos\theta,\sin\theta)$, project $e_1=(1,0)$ and $e_2=(0,1)$ onto it using right-triangle trigonometry:
- $P[e_1] = A' = (\cos\theta\cdot\cos\theta,\ \cos\theta\cdot\sin\theta)$ (since $OA'=\cos\theta$)
- $P[e_2] = B' = (\sin\theta\cdot\cos\theta,\ \sin\theta\cdot\sin\theta)$ (since $OB'=\sin\theta$)

These become the columns of $P_\theta$.

**Key properties:**
- $P_\theta$ has **no inverse** — because the projection transformation itself has no inverse (you cannot recover the original vector from its shadow on a line — information is lost).
- **Projecting twice = projecting once:** $P_\theta^2 = P_\theta$ (an **idempotent** matrix).

### 8.3 Reflection Matrices $H_\theta$

**Formula (reflection across the line through the origin at angle $\theta$):**
$$H_\theta = \begin{bmatrix}2\cos^2\theta - 1 & 2\cos\theta\sin\theta \\ 2\cos\theta\sin\theta & 2\sin^2\theta - 1\end{bmatrix}$$

**Derivation (geometric, from the course):**
Let $A$ be the foot of perpendicular and $A'$ the reflected image, $B$ the intersection point on the $\theta$-line. From the vector diagram:
$$\vec{OA} + \vec{AB} = \vec{OB} \quad \text{...(1)}, \qquad \vec{OA'} + \vec{A'B} = \vec{OB} \quad \text{...(2)}$$
Adding (1) and (2), and using $\vec{AB} = -\vec{A'B}$ (B is the midpoint of $AA'$):
$$\vec{OA} + \vec{OA'} = 2\vec{OB} \implies x + Hx = 2Px \implies \boxed{H = 2P - I}$$

**Key relation:** $H_\theta = 2P_\theta - I$

**Key properties:**
- **Two reflections bring back the original:** applying $H$ twice is the identity.
- Verified algebraically: $H^2 = (2P-I)^2 = 4P^2 - 4P + I = 4P-4P+I = I$ (using $P^2=P$).
- **A reflection is its own inverse:** $H^{-1} = H$.

### 8.4 Worked Problem — Relationship between S (Reflection) and T (Projection) on $y=x$

**Problem:** Find the matrix $S$ that reflects every vector in $\mathbb{R}^2$ on the line $y=x$. Also find the matrix $T$ that projects every vector onto $y=x$. Explain why $ST=TS$.

**Solution:** The line $y=x$ corresponds to $\theta = 45°$.

$$S = H_{45°} = \begin{bmatrix}2\cos^2(45°)-1 & 2\cos(45°)\sin(45°)\\ 2\cos(45°)\sin(45°) & 2\sin^2(45°)-1\end{bmatrix} = \begin{bmatrix}0&1\\1&0\end{bmatrix}$$

$$T = P_{45°} = \begin{bmatrix}\cos^2(45°) & \cos(45°)\sin(45°)\\ \cos(45°)\sin(45°) & \sin^2(45°)\end{bmatrix} = \begin{bmatrix}1/2&1/2\\1/2&1/2\end{bmatrix}$$

**Compute both products:**
$$ST = \begin{bmatrix}0&1\\1&0\end{bmatrix}\begin{bmatrix}1/2&1/2\\1/2&1/2\end{bmatrix} = \begin{bmatrix}1/2&1/2\\1/2&1/2\end{bmatrix}$$
$$TS = \begin{bmatrix}1/2&1/2\\1/2&1/2\end{bmatrix}\begin{bmatrix}0&1\\1&0\end{bmatrix} = \begin{bmatrix}1/2&1/2\\1/2&1/2\end{bmatrix}$$
$$\Rightarrow ST = TS$$

**Geometric explanation:** $ST$ is the composition "project onto $y=x$, then reflect onto $y=x$." $TS$ is "reflect onto $y=x$, then project onto $y=x$." Both operations send **every** point in the plane to the **same** final point on the line $y=x$ (since reflecting a point that's already on the line, or projecting the reflected point, both land back exactly on the line at the same spot) — so both composite transformations produce identical output for any input.

### 8.5 Worked Problem — Composed Transformations

**(a)** Project $x=(-2,1)$ onto the $y$-axis, then rotate the result by $45°$ counter-clockwise.

Projection onto $y$-axis ($\theta=90°$): $P_{90°} = \begin{bmatrix}0&0\\0&1\end{bmatrix}$

Rotation by 45°: $Q_{45°} = \begin{bmatrix}1/\sqrt2 & -1/\sqrt2\\ 1/\sqrt2&1/\sqrt2\end{bmatrix}$

$$Q_{45°}\,P_{90°}\begin{bmatrix}-2\\1\end{bmatrix} = \begin{bmatrix}1/\sqrt2&-1/\sqrt2\\1/\sqrt2&1/\sqrt2\end{bmatrix}\begin{bmatrix}0&0\\0&1\end{bmatrix}\begin{bmatrix}-2\\1\end{bmatrix} = \begin{bmatrix}-1/\sqrt2\\1/\sqrt2\end{bmatrix}$$

**(b)** Rotate $x=(-2,1)$ by $60°$ counter-clockwise, then project onto the $x$-axis.

$Q_{60°} = \begin{bmatrix}1/2 & -\sqrt3/2\\ \sqrt3/2 & 1/2\end{bmatrix}$, $\quad P_{0°} = \begin{bmatrix}1&0\\0&0\end{bmatrix}$

$$P_{0°}\,Q_{60°}\begin{bmatrix}-2\\1\end{bmatrix} = \begin{bmatrix}1&0\\0&0\end{bmatrix}\begin{bmatrix}1/2&-\sqrt3/2\\\sqrt3/2&1/2\end{bmatrix}\begin{bmatrix}-2\\1\end{bmatrix} = \begin{bmatrix}-1-\sqrt3/2\\0\end{bmatrix}$$

*(Note the order matters: applying rotation first then projection, as in the matrix product $P\cdot Q \cdot x$ read right-to-left, gives a different — generally — result than projection then rotation, since matrix multiplication is not commutative in general, unlike the special $\theta=45°$ case above.)*

### 8.6 Additional Examples

**Example A:** Find the rotation matrix for $\theta = 30°$ and use it to rotate the point $(3,4)$.
$$Q_{30°} = \begin{bmatrix}\sqrt3/2 & -1/2\\ 1/2 & \sqrt3/2\end{bmatrix}, \quad Q_{30°}\begin{bmatrix}3\\4\end{bmatrix} = \begin{bmatrix}\tfrac{3\sqrt3}{2}-2\\ \tfrac32+2\sqrt3\end{bmatrix} \approx \begin{bmatrix}0.598\\5.964\end{bmatrix}$$

**Example B:** Show directly that $P_\theta^2 = P_\theta$ for general $\theta$ (idempotency), using $\cos^2\theta+\sin^2\theta=1$.
$$P_\theta^2 = \begin{bmatrix}\cos^2\theta&\cos\theta\sin\theta\\\cos\theta\sin\theta&\sin^2\theta\end{bmatrix}\begin{bmatrix}\cos^2\theta&\cos\theta\sin\theta\\\cos\theta\sin\theta&\sin^2\theta\end{bmatrix}$$
Top-left entry: $\cos^4\theta + \cos^2\theta\sin^2\theta = \cos^2\theta(\cos^2\theta+\sin^2\theta)=\cos^2\theta$. Similarly for other entries ⇒ $P_\theta^2=P_\theta$. ✓

**Example C:** Find $H_{90°}$ (reflection across the $y$-axis) and verify it against $H=2P-I$.
$$P_{90°}=\begin{bmatrix}0&0\\0&1\end{bmatrix} \Rightarrow H_{90°} = 2\begin{bmatrix}0&0\\0&1\end{bmatrix}-\begin{bmatrix}1&0\\0&1\end{bmatrix} = \begin{bmatrix}-1&0\\0&1\end{bmatrix}$$
This matches intuition: reflecting across the $y$-axis flips the sign of $x$ and keeps $y$ the same.

### 8.7 Practice Questions

1. Find $Q_{90°}$ and verify it matches the "rotate 90° counter-clockwise" matrix $\begin{bmatrix}0&-1\\1&0\end{bmatrix}$ from Section 7.2.
2. Find $H_{0°}$ (reflection across the $x$-axis) using the formula, and check it matches your geometric intuition.
3. Compute $P_{60°}$ and verify $P_{60°}^2 = P_{60°}$ numerically.
4. A vector $(1,0)$ is first rotated by $30°$, then reflected across the line $y=x$. Find its final coordinates.
5. Prove algebraically that $Q_\theta$ is an **orthogonal matrix**, i.e. $Q_\theta^T Q_\theta = I$. What does this imply about lengths and angles being preserved?
6. Show that $\det(Q_\theta) = 1$ and $\det(H_\theta) = -1$ for all $\theta$. What does the sign of the determinant tell you about each transformation (orientation-preserving vs. orientation-reversing)?
7. If $S$ and $T$ are reflection and projection matrices onto the **same** line at angle $\theta$ (not necessarily 45°), prove in general that $ST = TS$ using $H_\theta = 2P_\theta - I$. *(Hint: does $P_\theta$ commute with itself and with $I$?)*

---

## 9. Quick-Reference Formula Sheet

**Vector Space Axioms:** closure under $+$ and scalar $\times$; commutative, associative, identity, inverse, distributive laws.

**Subspace Test:** $0\in W$; closed under $+$; closed under scalar $\times$.

**Rank–Nullity:** for $m\times n$ matrix $A$ with rank $r$:
$$\dim C(A) = \dim C(A^T) = r,\qquad \dim N(A) = n-r,\qquad \dim N(A^T) = m-r$$

**Four Fundamental Subspaces:**
$$C(A),\ N(A^T) \subseteq \mathbb{R}^m \qquad\qquad N(A),\ C(A^T) \subseteq \mathbb{R}^n$$

**Linearity Rule:** $T(cx+dy) = cT(x)+dT(y)$.

**Special $2\times2$ transformation matrices** (angle $\theta$):
$$Q_\theta=\begin{bmatrix}\cos\theta&-\sin\theta\\\sin\theta&\cos\theta\end{bmatrix}\ (\text{rotation}) \quad P_\theta=\begin{bmatrix}\cos^2\theta&\cos\theta\sin\theta\\\cos\theta\sin\theta&\sin^2\theta\end{bmatrix}\ (\text{projection}) \quad H_\theta=2P_\theta-I\ (\text{reflection})$$

| Property | $Q_\theta$ | $P_\theta$ | $H_\theta$ |
|---|---|---|---|
| Invertible? | Yes, $Q_\theta^{-1}=Q_{-\theta}$ | No | Yes, $H_\theta^{-1}=H_\theta$ |
| Composition rule | $Q_\theta Q_\psi = Q_{\theta+\psi}$ | $P_\theta^2 = P_\theta$ | $H_\theta^2 = I$ |
| Preserves | angles & lengths | neither (collapses dimension) | angles & lengths |

---

*End of notes. These notes consolidate all topics from the 111-slide course deck "Mathematical Foundation for AI & Data Science" (UE25MA25242A) and the related "Linear Algebra and Its Applications" (UE19MA251) unit on Linear Transformations & Orthogonality, Dr. Jyothi R, PES University — with additional worked examples and practice questions added for deeper understanding.*
