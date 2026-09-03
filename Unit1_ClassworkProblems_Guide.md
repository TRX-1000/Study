# Unit 1: Linear Systems and Structural Foundations
## Complete Teaching Guide + Worked Solutions (Problems 1–31)

*(Faithful to your uploaded problem sheet's notation and given answers. Where my own step-by-step elimination genuinely disagrees with a printed answer, I've flagged it explicitly rather than silently forcing a match — see the notes on Problems 6 and 7.)*

---

# PART A — LINEAR SYSTEMS: PICTURES, RANK, RREF, ELIMINATION

## A.1 The Row Picture and the Column Picture

Every system `Ax = b` can be read two completely different ways:

- **Row picture:** each equation is a line (in 2 variables) or a plane (in 3 variables). The solution is the point/line/plane where **all** the rows' geometric objects meet simultaneously.
- **Column picture:** rewrite the system as `x₁·(col 1) + x₂·(col 2) + ⋯ = b`. The solution tells you **which combination of the column vectors** reproduces `b`.

Both pictures describe the *same* algebra — they're just two different geometric lenses on it.

### Worked Problem 1 — row/column pictures and solving

**a) `x − y = 2`, `2x + y = 7`**
*Row picture:* two lines in the `xy`-plane; solve by elimination — add the equations: `3x = 9 → x = 3`, then `y = x − 2 = 1`. **(3, 1)**.
*Column picture:* `x(1,2) + y(−1,1) = (2,7)`. Plugging `x=3, y=1`: `3(1,2)+1(−1,1) = (3−1, 6+1) = (2,7)` ✓.

**b) (HW) `x − y = −1`, `2x + y = 4`** → add: `3x=3→x=1`, `y=x+1=2`. **(1, 2)**.

**c) `2x + y = 5`, `x − y = 7`** → add: `3x=12→x=4`, `y=x−7=−3`. **(4, −3)**.

**d) `x + 2y = 2`, `2x + 4y = 6`** → the second equation is `2×(x+2y) = 4`, but it's set equal to `6` — a contradiction. Geometrically, the two lines are **parallel and distinct**. **No solution.**

**e) (HW) `x + 2y = 3`, `2x + 4y = 6`** → the second equation is exactly `2×` the first — same line twice. **Infinitely many solutions** (a whole line of them).

**Exam tip:** to instantly tell (d) from (e), check whether the second row is a *scalar multiple of the first row of the coefficients* — if yes, check whether the right-hand side follows the same scalar (consistent → infinite solutions; inconsistent → no solution).

## A.2 Rank via Elimination

**Definition:** the rank of `A` is the number of nonzero pivot rows you get after row-reducing `A` — equivalently, the number of linearly independent rows (which always equals the number of linearly independent columns).

### Worked Problem 2 — finding rank

**i) `A = [1 2 1; 3 5 4; 2 4 2]`**
`R2−3R1 → (0,−1,1)`; `R3−2R1 → (0,0,0)`.
Result: `[1 2 1; 0 −1 1; 0 0 0]` → **2 nonzero rows → rank = 2.**

**ii) `A = [1 2 1; 2 4 2; 3 5 4; 4 8 4]`**
`R2−2R1 → (0,0,0)`; `R3−3R1 → (0,−1,1)`; `R4−4R1 → (0,0,0)`.
Only rows 1 and 3 (after elimination) are nonzero and independent → **rank = 2.**

**iii) `A = [1 2 3; 2 5 4; 3 1 2]`**
`R2−2R1 → (0,1,−2)`; `R3−3R1 → (0,−5,−7)`.
`R3+5R2 → (0,0,−17)`.
Three nonzero pivots → **rank = 3** (full rank — this matrix is invertible).

## A.3 Row Reduced Echelon Form (RREF)

**Procedure:** row-reduce to echelon form (like ordinary Gaussian elimination), then (1) normalize every pivot to `1`, and (2) eliminate *upward* so every pivot column is entirely zero except for that one `1`.

### Worked Problem 3 — RREF

**i) `A = [1 1 2 3; 2 3 5 8; 1 2 3 5]`**
`R2−2R1 → (0,1,1,2)`; `R3−R1 → (0,1,1,2)`.
`R3−R2 → (0,0,0,0)`.
So far: `[1 1 2 3; 0 1 1 2; 0 0 0 0]`.
Eliminate upward: `R1−R2 → (1,0,1,1)`.
**RREF = `[1 0 1 1; 0 1 1 2; 0 0 0 0]`.**

**ii) `A = [1 2 1; 2 4 3; 3 6 4]`**
`R2−2R1 → (0,0,1)`; `R3−3R1 → (0,0,1)`.
`R3−R2 → (0,0,0)`.
So far: `[1 2 1; 0 0 1; 0 0 0]`.
Eliminate upward: `R1−R2 → (1,2,0)`.
**RREF = `[1 2 0; 0 0 1; 0 0 0]`.**

## A.4 Gauss Elimination: Consistency and Solving

**Procedure:**
1. Form the augmented matrix `[A|b]`.
2. Row-reduce as usual.
3. If you get a row `[0 0 ... 0 | k]` with `k ≠ 0` → **inconsistent, no solution.**
4. If you get exactly `n` pivots (one per variable) → **unique solution**, found by back substitution.
5. If you get fewer than `n` pivots but no contradiction → **infinitely many solutions**, expressed with free parameter(s).

### Worked Problem 4

**a) `x+2y−z=6, 2x+y+z=3, x−y+z=−2`**
`[1 2 −1|6; 2 1 1|3; 1 −1 1|−2]`
`R2−2R1 → (0,−3,3|−9)`; `R3−R1 → (0,−3,2|−8)`.
`R3−R2 → (0,0,−1|1) → z = −1`.
Back: `−3y+3(−1)=−9 → −3y=−6 → y=2`. Then `x+2(2)−(−1)=6 → x+5=6 → x=1`.
**(x,y,z) = (1, 2, −1)** ✓ matches given.

**b) `x−3y−8z=−10, 3x+y−4z=0, 2x+5y+6z=13`**
`[1 −3 −8|−10; 3 1 −4|0; 2 5 6|13]`
`R2−3R1 → (0,10,20|30)` → divide by 10 → `(0,1,2|3)`.
`R3−2R1 → (0,11,22|33)` → divide by 11 → `(0,1,2|3)` — **identical to the row above**, meaning the system only has **2 independent equations** (rank 2 < 3 variables) → one free parameter, but **no contradiction**, so infinitely many solutions.
Let `z = k`. From `y+2z=3 → y = 3−2k`. From row 1: `x−3y−8z=−10 → x = −10+3y+8z = −10+3(3−2k)+8k = −1+2k`.
**(x,y,z) = (2k−1, −2k+3, k)** ✓ matches given — a whole *line* of solutions, parametrized by `k`.

**c) (labeled "b)" in the sheet, likely a typo) `x+2y−4z=−4, 2x+5y−9z=−10, y−z=2`**
`[1 2 −4|−4; 2 5 −9|−10; 0 1 −1|2]`
`R2−2R1 → (0,1,−1|−2)`.
Compare to row 3 `(0,1,−1|2)`: **same left-hand side, different right-hand side** → `R3 − R2(new) → (0,0,0|4)`, i.e. `0 = 4`. **Contradiction → No solution.**

**HW a) `2x+y−z=4, x+y+z=4, x−y+z=2`**
Swap rows for convenience: `[1 1 1|4; 2 1 −1|4; 1 −1 1|2]`
`R2−2R1 → (0,−1,−3|−4)`; `R3−R1 → (0,−2,0|−2)`.
`R3−2R2 → (0,0,6|6) → z=1`. Back: `−y−3=−4→y=1`. `x+1+1=4→x=2`.
**(2, 1, 1)** ✓.

**HW b) `x+2y−3z=1, y−2z=2, 2y−4z=4`**
Third row = `2×`second row exactly (both sides) → dependent, one free parameter.
Let `z=k`: `y=2+2k`. From row 1: `x = 1−2y+3z = 1−2(2+2k)+3k = −3−k`.
**(x,y,z) = (−3−k, 2+2k, k)** ✓ matches given.

## A.5 Parameter Problems — "for which a/b does the system behave a certain way"

**Procedure:** eliminate symbolically, keeping the parameter as a letter. Whatever makes a **pivot vanish** is the critical value; then check the corresponding row's right-hand side for consistency.

### Worked Problem 5
`ax+2y=0`, `2x+ay=0` — this is homogeneous, so `x=y=0` is always a solution. A "whole line of solutions" (infinitely many, not just the trivial one) happens exactly when the coefficient matrix is **singular**, i.e. `det = a²−4 = 0 → a = ±2`. ✓ matches given.

### Worked Problem 6 — full derivation, with an honest flag

`x+y+z=5`, `ax+y+az=3`, `(1+a)x+2y+3z=b`.

Eliminate `x` using `x = 5−y−z` from the first equation, substitute into the other two:
- Equation 2: `a(5−y−z)+y+az=3 → 5a + y(1−a) + z(−a+a) = 3` — **the z-terms cancel!** → `(1−a)y = 3−5a`.
- Equation 3: `(1+a)(5−y−z)+2y+3z=b → 5+5a + y(1−a) + z(2−a) = b`. Substituting `(1−a)y = 3−5a` from above: `5+5a+(3−5a)+z(2−a)=b → 8+z(2−a)=b → z(2−a)=b−8`.

So:
- `y` is uniquely determined **only if `1−a ≠ 0`**, i.e. `a ≠ 1`.
- `z` is uniquely determined **only if `2−a ≠ 0`**, i.e. `a ≠ 2`.

**My derivation therefore gives: unique solution requires `a ≠ 1` *and* `a ≠ 2`** (for any `b`). At `a = 2`: the equation for `z` becomes `0 = b−8`, so `b=8` gives infinitely many solutions (z free) and `b≠8` gives no solution — this part matches your answer key exactly. At `a = 1`, however, the equation for `y` becomes `0·y = 3−5(1) = −2`, i.e. `0 = −2`, which is a contradiction **for every value of `b`** — so `a=1` should also be excluded from "unique solution," and in fact makes the system inconsistent regardless of `b`.

**Your printed answer key states only `a≠2` for the unique-solution case** and doesn't mention `a=1`. Based on straightforward elimination this looks like it should read `a ≠ 1 and a ≠ 2`. Worth double-checking with your professor — but the *method* here (eliminate symbolically, track which pivot vanishes) is exactly the technique to reuse on similar exam questions, regardless of this one detail.

### Worked Problem 7 — same honest-flag treatment

`x+y+az=2b`, `x+3y+(2+2a)z=7b`, `3x+y+(3+3a)z=11b`.

Eliminate `x` via `x=2b−y−az`:
- Eq 2: `(2b−y−az)+3y+(2+2a)z=7b → 2y+(2+a)z=5b → y = [5b−(2+a)z]/2`.
- Eq 3: `3(2b−y−az)+y+(3+3a)z=11b → −2y+3z=5b → y = (3z−5b)/2`.

Setting the two expressions for `y` equal: `5b−(2+a)z = 3z−5b → 10b = z(5+a)`.

So `z` is uniquely determined whenever `a ≠ −5`; this matches your key's `a≠−5` condition for both "trivial" and "unique non-trivial" cases (at `a≠−5`, plugging `b=0` gives the trivial solution `(0,0,0)`; any `b≠0` gives a unique non-trivial solution).

At `a = −5`: the equation becomes `0 = 10b`, which by my derivation requires **`b = 0`** for consistency (giving infinitely many solutions), and any `b ≠ 0` gives no solution. **Your key states the critical value as `b = 4/9` rather than `b = 0`.** Following the algebra as transcribed from your equations, I consistently get `b=0` as the tipping point, not `4/9` — this is the second place I'd suggest cross-checking the exact coefficients against your original source (a transcription slip in the `7b`/`11b`/`2b` constants would easily shift that number). The elimination *method* shown above is correct and reusable regardless.

### H.W. problem
`x+2y+z=3`, `ax+5y=10`, `2x+7y+az=b`.
Eliminating similarly (eliminate `x` using row 1, then compare resulting pivots) leads to a quadratic condition in `a` for the unique-solution boundary; your key states **unique solution for `a≠5` and `a≠−3`**, with infinite solutions at **`(a,b)=(5,12)` or `(−3,−4)`**. This is the same style of symbolic elimination as Problems 6–7 above — practice it yourself as the HW label intends, using the row-elimination method demonstrated in detail above.

## A.6 Which elementary matrices reduce A to U? (Problem 8)

`A = [1 1 0; 4 6 1; −2 2 0]`

`R2 − 4R1 → (0, 2, 1)`; `R3 + 2R1 → (0, 4, 0)`.
`R3 − 2R2(new) → (0, 0, −2)`.

`U = [1 1 0; 0 2 1; 0 0 −2]`.

```
E₂₁ = [1  0 0]     E₃₁ = [1 0 0]     E₃₂ = [1  0 0]
      [−4 1 0]           [0 1 0]           [0  1 0]
      [0  0 1]           [2 0 1]           [0 −2 1]
```
and `E₃₂E₃₁E₂₁A = U` (see Part 1 of your earlier study guide for the full theory behind this).

## A.7 LU and LDU factorization (Problem 9)

**i) `A = [3 1 2; 2 −3 −1; 1 2 1]`** — this is the exact matrix worked in full in your previous Class-4/5 study guide (§2.4): `U=[3 1 2; 0 −11/3 −7/3; 0 0 −8/11]`, `L=[1 0 0; 2/3 1 0; 1/3 −5/11 1]`.

**ii) (HW) `A = [1 2 1 2; 2 4 2 1; 1 3 2 1; 1 3 4 1]`**

This one is a great test of Part 3–4 skills (row exchanges), because plain elimination hits **two** zero-pivot snags.

`R2−2R1, R3−R1, R4−R1` →
```
[1 2 1 2]
[0 0 0 −3]
[0 1 1 −1]
[0 1 3 −1]
```
Pivot `(2,2)=0`, but row 3 has a `1` there — **swap rows 2 and 3**:
```
[1 2 1 2]
[0 1 1 −1]
[0 0 0 −3]
[0 1 3 −1]
```
`R4 − R2 →`
```
[1 2 1 2]
[0 1 1 −1]
[0 0 0 −3]
[0 0 2 0]
```
Pivot `(3,3)=0` again, but row 4 has a `2` there — **swap rows 3 and 4**:
```
U = [1 2 1  2]
    [0 1 1 −1]
    [0 0 2  0]
    [0 0 0 −3]
```
Tracking both swaps, the overall row order used was `(row1, row3, row4, row2)` of the *original* `A`, so:
```
P = [1 0 0 0]      L = [1 0 0 0]      (multipliers, read off directly
    [0 0 1 0]          [1 1 0 0]       from the elimination steps above,
    [0 0 0 1]          [1 1 1 0]       in the P-reordered row sequence)
    [0 1 0 0]          [2 0 0 1]
```
`PA = LU`. **Check (row 4 of PA should be original row 2 = `(2,4,2,1)`):** `[2,0,0,1]·U = 2(1,2,1,2)+1(0,0,0,−3) = (2,4,2,4−3)=(2,4,2,1)` ✓.

For `A=LDU`: `D = diag(1,1,2,−3)`, and dividing each row of `U` by its pivot gives `U(new) = [1 2 1 2; 0 1 1 −1; 0 0 1 0; 0 0 0 1]`.

**iii) (HW, non-square) `A = [1 2 0 2; 1 3 2 −1; 2 3 4 0]`**
`R2−R1 → (0,1,2,−3)`; `R3−2R1 → (0,−1,4,−4)`.
`R3+R2 → (0,0,6,−7)`.
```
U = [1 2 0  2]        L = [1  0 0]
    [0 1 2 −3]            [1  1 0]
    [0 0 6 −7]            [2 −1 1]
```
No row exchange needed here — the LU factorization goes through directly even though `A` isn't square.

## A.8 Inverse via Gauss–Jordan (Problem 10)

**i) `A = [2 2 1; 2 1 −1; 1 1 2]`**
`[A:I] = [2 2 1:1 0 0; 2 1 −1:0 1 0; 1 1 2:0 0 1]`
`R2−R1 → (0,−1,−2:−1,1,0)`; `R3−(1/2)R1 → (0,0,3/2:−1/2,0,1)`.
Normalize `R3 ×(2/3) → (0,0,1:−1/3,0,2/3)`.
`R2 + 2R3(new) → (0,−1,0:−1−2/3,1,4/3) = (0,−1,0:−5/3,1,4/3)`; multiply by `−1 → (0,1,0:5/3,−1,−4/3)`.
`R1 − R3(new) → (2,2,0:1+1/3,0,−2/3) = (2,2,0:4/3,0,−2/3)`; `R1 − 2R2(new) → (2,0,0:4/3−10/3,2,−2/3+8/3)=(2,0,0:−2,2,2)`; normalize `×(1/2) → (1,0,0:−1,1,1)`.
```
A⁻¹ = [−1    1    1  ]
      [ 5/3 −1  −4/3]
      [−1/3  0   2/3]
```
✓ matches your given answer exactly.

**ii) (HW) `A = [2 −1 0; −1 2 −1; 0 −1 2]`** — apply the identical algorithm (this is a symmetric tridiagonal matrix, a very common exam setup). Your key gives `A⁻¹ = [3/4 1/2 1/4; 1/2 1 1/2; 1/4 1/2 3/4]` — notice `A⁻¹` is itself symmetric, exactly as guaranteed by the property from Part 8 of your earlier guide ("if a symmetric matrix has an inverse, that inverse is also symmetric"). Work through the same forward-then-backward Gauss–Jordan pass to verify this yourself.

---

# PART B — VECTOR SPACES AND SUBSPACES

## B.1 What is a vector space?

A **vector space** is a set of objects ("vectors") that you can add together and scale by numbers, and which behaves consistently under those operations — in particular, it must:
1. Contain a **zero vector** `0` (an "identity" for addition).
2. Be **closed under addition**: adding two vectors in the set gives another vector in the set.
3. Be **closed under scalar multiplication**: scaling a vector in the set gives another vector in the set.

(There are more formal axioms — associativity, commutativity, distributive laws — but in practice, checking these three is what catches almost every exam question.)

### Worked Problem 11 (MCQ)
**Which of these is NOT a vector space: (A) polynomials of degree exactly 2, (B) degree at most 2, (C) continuous functions on [0,1], (D) all 2×2 matrices?**

**Answer: (A).** The set of degree-*exactly*-2 polynomials fails closure under addition: `x² + (−x²) = 0`, which has degree less than 2 (in fact it's the zero polynomial, degree undefined/−∞), so the sum leaves the set. It also doesn't contain a zero vector at all (the zero polynomial isn't "degree exactly 2"). (B), (C), (D) all satisfy every vector space axiom — degree-at-most-2 polynomials are closed under addition/scaling and include the zero polynomial; continuous functions and matrices behave the same way.

**Exam pattern to remember:** "exactly `n`" degree conditions almost always fail to be vector spaces; "at most `n`" almost always succeed. Always check the zero vector first — it's the fastest way to rule something out.

## B.2 Subspaces

A **subspace** `W` of a vector space `V` is a subset that is *itself* a vector space using `V`'s own operations. The **Subspace Test** says `W` is a subspace if and only if:
1. `0 ∈ W`,
2. `W` is closed under addition,
3. `W` is closed under scalar multiplication.

### Worked Problem 12 (MCQ)
**Which condition is sufficient to guarantee `W` is a subspace?**
**Answer: (C)** — contains the zero vector, closed under addition, and closed under scalar multiplication (all three, together). (A) and (B) each only check one closure property and skip the other — both insufficient alone. (D) (same dimension as `V`) is irrelevant to being a subspace at all.

### Worked Problem 13
**`S = {(x,y,z) ∈ ℝ³ : x+2y−z=0}` — what is it?**
**Answer: (B) — a plane through the origin, hence a subspace.** Because the defining equation is **homogeneous** (equals zero, no constant term), `(0,0,0)` automatically satisfies it, and the equation is preserved under both addition and scalar multiplication of solutions (if `(x₁,y₁,z₁)` and `(x₂,y₂,z₂)` both satisfy it, so does their sum and any scalar multiple). A non-homogeneous version like `x+2y−z=5` would describe a plane **not** through the origin — and that would fail the subspace test immediately (zero vector doesn't satisfy it).

**Exam pattern:** *homogeneous* linear equations (`=0`) define subspaces; anything with a nonzero constant does not.

### Worked Problem 14
**If `W` is a subspace of `ℝ⁴` with `dim(W)=4`, then `W` must be:**
**Answer: (B) — equal to `ℝ⁴` itself.** A subspace of `ℝⁿ` with dimension exactly `n` must contain `n` linearly independent vectors — which is already enough to form a basis for all of `ℝⁿ`. There's no "room" left for `W` to be a proper subset.

---

# PART C — LINEAR (IN)DEPENDENCE AND SPAN

## C.1 Linear independence — definition and test

A set of vectors `{v₁, v₂, …, vₖ}` is **linearly independent** if the only way to write `c₁v₁+c₂v₂+⋯+cₖvₖ = 0` is with **all coefficients zero**. If some *nontrivial* combination equals zero, the set is **dependent** — meaning at least one vector is a combination of the others.

**Practical test:** try to express one vector as a combination of the others (or set up and solve the homogeneous system `c₁v₁+⋯=0` for the `c_i`'s).

### Worked Problem 15
`v1=(1,2,1)`, `v2=(2,1,0)`, `v3=(0,3,2)`. Check: `2v1 − v2 = (2,4,2)−(2,1,0) = (0,3,2) = v3`.
Since `v3` is a combination of `v1,v2`, the set is **linearly DEPENDENT** (the given check confirms this relationship — it does *not* mean they're independent; finding *any* nontrivial relation like this is exactly what dependence means).

### Worked Problem 16
`v1=(1,3,2)`, `v2=(2,1,4)`, `v3=(3,2,6)`.
Try `v3 = a·v1 + b·v2`: `a+2b=3`, `3a+b=2`, `2a+4b=6` (this last equation is just `2×` the first, so it's automatically satisfied once the first holds). Solving the first two: from `a=3−2b`, substitute into the second: `3(3−2b)+b=2 → 9−5b=2 → b=7/5, a=1/5`.
So `v3 = (1/5)v1 + (7/5)v2`, confirmed: `(1/5)(1,3,2)+(7/5)(2,1,4) = (1/5+14/5, 3/5+7/5, 2/5+28/5) = (3,2,6)` ✓.
**Dependent** ✓ matches given.

### Worked Problem 17
`v1=(1,−3,2)`, `v2=(2,1,−3)`, `v3=(−3,2,1)`. Check `v1+v2+v3`: `(1+2−3, −3+1+2, 2−3+1) = (0,0,0)`.
**Dependent, with relation `v1+v2+v3=0`** ✓ matches given.

### Worked Problem 18 (HW) — a general independence proof
Show `{u+v, u−v, u−2v+w}` is linearly independent, **assuming `u, v, w` themselves are linearly independent.**
Suppose `c₁(u+v)+c₂(u−v)+c₃(u−2v+w)=0`. Group by `u, v, w`:
`(c₁+c₂+c₃)u + (c₁−c₂−2c₃)v + c₃w = 0`.
Since `u,v,w` are independent, each coefficient must be zero: `c₃=0`; then `c₁+c₂=0` and `c₁−c₂=0` — adding these gives `2c₁=0 → c₁=0`, and then `c₂=0`.
All coefficients forced to zero → **the set is linearly independent.** *(This is a good general template: whenever the problem gives combinations of "assumed independent" vectors, expand, group by the original vectors, and use their independence to force each combined coefficient to zero.)*

## C.2 Span

A set of vectors **spans** a space `V` if every vector in `V` can be written as some combination of them. For `n` vectors to span `ℝⁿ`, it's enough (and necessary) that they be **linearly independent** — equivalently, that the matrix formed from them has a **nonzero determinant** (full rank).

### Worked Problem 19
**Do `{(2,2,3), (−1,−2,1), (0,1,0)}` span `ℝ³`?**
Compute the determinant of the matrix with these as rows (or columns):
```
det [ 2  2  3]
    [−1 −2  1]
    [ 0  1  0]
```
Expand along row 3 (only one nonzero entry, the `1` in the middle): `= −1 · det[2 3; −1 1] = −1·(2·1 − 3·(−1)) = −1·(2+3) = −5 ≠ 0`.
Nonzero determinant → the three vectors are linearly independent → **they span `ℝ³`.** ✓ matches given ("yes").

---

# PART D — THE FOUR FUNDAMENTAL SUBSPACES, BASIS, DIMENSION, AND RANK-NULLITY

## D.1 The four subspaces of a matrix A (m×n)

| Subspace | Lives in | Dimension |
|---|---|---|
| **Column space** `C(A)` | `ℝᵐ` | `rank(A) = r` |
| **Row space** `C(Aᵀ)` | `ℝⁿ` | `r` (same as column space!) |
| **Null space** `N(A)` (solutions of `Ax=0`) | `ℝⁿ` | `n − r` |
| **Left null space** `N(Aᵀ)` (solutions of `Aᵀy=0`, i.e. `yᵀA=0`) | `ℝᵐ` | `m − r` |

**Rank–Nullity Theorem:** `rank(A) + nullity(A) = n` (number of columns). Equivalently, `dim(row space) + dim(null space) = n`.

### Worked Problem 20

**i) `A = [1 1 −1 1; 1 −1 2 −1; 3 1 0 1]` (3×4)**

Row reduce: `R2−R1 → (0,−2,3,−2)`; `R3−3R1 → (0,−2,3,−2)`; `R3−R2(new) → (0,0,0,0)`.
Only **2 independent rows** → `rank = 2`. Fully reduced (dividing row 2 by −2, then clearing row 1): RREF `= [1 0 1/2 0; 0 1 −3/2 1; 0 0 0 0]`.

- **Row space:** dimension 2, basis `{(1,1,−1,1), (0,−2,3,−2)}` (the original pivot rows work fine as a basis too).
- **Column space:** dimension 2, basis = the *original* columns corresponding to the pivot columns of the RREF (columns 1 and 2): `{(1,1,3)ᵀ, (1,−1,1)ᵀ}`.
- **Null space:** from the RREF, `x₁ = −(1/2)x₃`, `x₂ = (3/2)x₃ − x₄`, with `x₃, x₄` free. Setting `(x₃,x₄)=(2,0)` gives `(−1,3,2,0)`; setting `(x₃,x₄)=(0,1)` gives `(0,−1,0,1)`. **Basis: `{(−1,3,2,0), (0,−1,0,1)}`, dimension 2.**
- **Left null space:** noticing `R3 = 2R1 + R2` (check: `2(1,1,−1,1)+(1,−1,2,−1) = (3,1,0,1)` ✓), we get `−2R1 − R2 + R3 = 0`. **Basis: `{(−2,−1,1)}`, dimension 1.**

**Rank–nullity check:** `rank + nullity = 2 + 2 = 4 = n` ✓ (number of columns). Also `rank + dim(left null space) = 2+1 = 3 = m` ✓ (number of rows).

**ii) `A = [4 2 4; 2 4 8; 2 2 2]` (3×3)**
`R2−(1/2)R1 → (0,3,6)`; `R3−(1/2)R1 → (0,1,0)`; `R3−(1/3)R2(new) → (0,0,−2)`.
**Three nonzero pivots → rank = 3 (full rank)**, so `A` is invertible: column space = row space = `ℝ³` (dimension 3), null space = `{0}` (dimension 0), left null space = `{0}` (dimension 0).
Check: `rank+nullity = 3+0=3=n` ✓.

**HW i) `A=[1 2 3 4; 3 7 10 11; 2 4 7 7]`, HW ii) `A=[1 0 2 1; 0 1 1 0; 1 0 2 1]`** — apply the exact same recipe: row-reduce, count pivots for rank, read the null space off the RREF's free variables, and find the left-null-space relation among the rows. *(Tip for HW-ii: notice row 3 equals row 1 exactly — that instantly tells you the rank is at most 2 before you even finish reducing.)*

## D.2 Finding a basis for an implicit subspace

### Worked Problem 21
**Find a basis for the plane `2x−y+z−3t=0` (a subspace of `ℝ⁴`).**
Solve for one variable in terms of the rest — here it's cleanest to isolate `x`: `x = (y−z+3t)/2`. The free variables are `y, z, t`.
- `(y,z,t)=(1,0,0) → x=1/2` → vector `(1/2, 1, 0, 0)`
- `(y,z,t)=(0,1,0) → x=−1/2` → vector `(−1/2, 0, 1, 0)`
- `(y,z,t)=(0,0,1) → x=3/2` → vector `(3/2, 0, 0, 1)`

**Basis: `{(1/2,1,0,0), (−1/2,0,1,0), (3/2,0,0,1)}`** ✓ matches given. This is a 3-dimensional subspace of `ℝ⁴` (a "hyperplane"), consistent with one homogeneous linear constraint on 4 variables cutting dimension down by exactly 1 (`4−1=3`).

---

# PART E — LINEAR TRANSFORMATIONS

## E.1 What makes a transformation linear

`T` is a **linear transformation** if, for all vectors `u,v` and scalars `k`:
1. `T(u+v) = T(u)+T(v)` (additivity), and
2. `T(kv) = kT(v)` (homogeneity).

**Fast necessary check:** `T(0)` must equal `0`. If `T(0) ≠ 0`, `T` is *immediately* disqualified — this catches most "not linear" exam answers in one line.

### Worked Problem 22
**a) `T(x,y,z) = (x+y+z, 2x−3y+4z)`** — every output coordinate is a linear combination of the inputs with no constants added. `T(0,0,0)=(0,0)` ✓. **This IS linear.**

**b) `T(x,y) = (x+3, 2y, x+y)`** — `T(0,0) = (3,0,0) ≠ (0,0,0)`. **NOT linear** (fails `T(0)=0`).

**c) `T(x,y) = (xy, x)`** — the first coordinate `xy` is a *product* of the inputs, not a linear combination. Check homogeneity directly: `T(kx,ky) = (k²xy, kx)`, but `kT(x,y) = (kxy, kx)`. These only match if `k²=k` for all `k`, which is false in general. **NOT linear** (fails `T(kv)=kT(v)`).

**So both (b) and (c) fail to be linear — (b) because `T(0)≠0`, (c) because it fails the scaling condition — while (a) is the only linear one.** *(Your source sheet's answer lists these as a separate "b)" and a stray "d)" bullet due to formatting, but there are only three options a/b/c — the content above is the complete, correctly-labeled picture.)*

### Worked Problem 23
The four items here are given as implicit equations (planes): **a) `x+2y+z−3=0`, b) `x+5y−2z=2t`, c) `x+2y+z=0`, d) `−5x−y−3z=0`.**
Only **(a)** has a nonzero constant term (`−3`), meaning it's **not homogeneous** — plugging in the zero vector gives `−3 ≠ 0`. **(a) fails to define a subspace/linear object**, exactly analogous to Problem 13's logic. (b), (c), (d) are all homogeneous and therefore fine.

## E.2 Matrix representation of a linear transformation

**Key idea:** a linear transformation `T` is *completely* determined by what it does to each basis vector. If `{e₁,…,eₙ}` is the standard basis, then the matrix of `T` has `T(e₁), T(e₂), …` as its **columns.**

### Worked Problem 24 — differentiation operator
On polynomials of degree ≤4 with basis `{1, t, t², t³, t⁴}`: `d/dt(1)=0`, `d/dt(t)=1`, `d/dt(t²)=2t`, `d/dt(t³)=3t²`, `d/dt(t⁴)=4t³`.
Writing each output's coefficients (in the degree ≤3 target space, basis `{1,t,t²,t³}`) as a column:
```
A_diff = [0 1 0 0 0]
         [0 0 2 0 0]
         [0 0 0 3 0]
         [0 0 0 0 4]
```
✓ matches given (a 4×5 matrix, since differentiation maps the 5-dimensional `P₄` down into the 4-dimensional `P₃`).

### Worked Problem 25 (HW) — integration operator
By the reverse logic (`∫1 dt = t`, `∫t dt = t²/2`, etc.), on `P₃ → P₄`:
```
A_Int = [0   0   0   0]
        [1   0   0   0]
        [0  1/2  0   0]
        [0   0  1/3  0]
        [0   0   0  1/4]
```
(a 5×4 matrix, mapping the 4-dimensional `P₃` up into the 5-dimensional `P₄`) — as given.

### Worked Problem 26 — multiplication operator
**Matrix representing "multiply by `3t+5`", from `P₃` to `P₄`.**
If `p(t) = c₀+c₁t+c₂t²+c₃t³`, then `(3t+5)p(t) = 5c₀ + (3c₀+5c₁)t + (3c₁+5c₂)t² + (3c₂+5c₃)t³ + 3c₃t⁴`.
Reading off each output coefficient as a row, and each input coefficient `c₀,c₁,c₂,c₃` as a column:
```
A = [5 0 0 0]
    [3 5 0 0]
    [0 3 5 0]
    [0 0 3 5]
    [0 0 0 3]
```
✓ matches given exactly. **General pattern worth remembering:** multiplying by a degree-1 polynomial `(αt+β)` always produces this "double-diagonal" band-matrix shape — `β` down the main diagonal, `α` down the diagonal just below it.

### Worked Problem 27 — matrix, range, and kernel of a transformation
**`T: ℝ³→ℝ²`, `T(x,y,z)=(2x−y, 3y+z)`, standard bases.**
`T(1,0,0)=(2,0)`; `T(0,1,0)=(−1,3)`; `T(0,0,1)=(0,1)`. Columns give:
```
T = [2 −1 0]
    [0  3 1]
```
**Range:** the two rows `(2,−1,0)` and `(0,3,1)` are clearly independent (rank 2), so the transformation hits every vector in the 2-dimensional target — **Range `= ℝ²`.**
**Kernel:** solve `2x−y=0` and `3y+z=0`. From the first, `y=2x`; from the second, `z=−3y=−6x`. So `Ker(T) = {(x,2x,−6x) : x∈ℝ} = span{(1,2,−6)}` — **a line through the origin in `ℝ³`.** ✓ matches given.

### Worked Problem 28 (HW) — same recipe
**`T(x,y,z)=(2y+z, x−4y, 3x)` on `ℝ³`, standard basis.**
`T(1,0,0)=(0,1,3)`; `T(0,1,0)=(2,−4,0)`; `T(0,0,1)=(1,0,0)`.
```
T = [0 2 1]
    [1 −4 0]
    [3 0 0]
```
Determinant (expand along row 3, only one nonzero entry): `3·det[2 1; −4 0] = 3·(0−(−4)) = 3·4 = 12 ≠ 0` → the matrix is invertible → **Range `= ℝ³`, Kernel `= {0}` (just the origin).** ✓ matches given.

## E.3 Geometric transformation matrices (rotation, projection, reflection)

**Standard building blocks:**
- **Rotation by angle `θ` (counterclockwise):** `Q = [cosθ −sinθ; sinθ cosθ]`. For `θ=90°`: `Q=[0 −1; 1 0]`.
- **Projection onto the x-axis:** `P = [1 0; 0 0]`.
- **Reflection about the y-axis:** `H = [−1 0; 0 1]`.
- **Composing transformations:** *apply-first goes on the right.* If you rotate then project, the combined matrix is `(project)·(rotate)`; if you project then rotate, it's `(rotate)·(project)`.

### Worked Problem 29
**Rotate 90° (CCW), then project onto the x-axis.**
`Q = [0 −1; 1 0]` (rotate first), `P = [1 0; 0 0]` (project second) → combined matrix `A = PQ`:
```
A = [1 0][0 −1]   [0 −1]
    [0 0][1  0] = [0  0]
```
✓ matches given.

### Worked Problem 30 (HW)
**Project onto the x-axis first, then rotate 90°.**
`P=[1 0; 0 0]` first, `Q=[0 −1; 1 0]` second → combined `A=QP`:
```
A = [0 −1][1 0]   [0 0]
    [1  0][0 0] = [1 0]
```
✓ matches given. Notice this is a **different** matrix from Problem 29 — order of operations genuinely matters, exactly as with any matrix product.

### Worked Problem 31
**Reflect about the y-axis; find the image of `(4,−3)`.**
`H = [−1 0; 0 1]`. `H·(4,−3) = (−1·4+0·(−3), 0·4+1·(−3)) = (−4, −3)`.
✓ matches given: the x-coordinate flips sign, the y-coordinate stays the same — exactly what reflecting across the y-axis should do.

---

# QUICK-REFERENCE PROCEDURES (add to your revision sheet)

- **Row/column picture + solve:** graph as lines/planes (row) or find the combination of columns hitting `b` (column); solve by elimination either way.
- **Rank:** row-reduce, count nonzero pivot rows.
- **RREF:** row-reduce → normalize each pivot to 1 → eliminate upward until each pivot column is clean.
- **Consistency of `Ax=b`:** row-reduce `[A|b]`; a `0=k≠0` row means no solution; fewer pivots than unknowns (and no contradiction) means infinitely many; full pivots means unique.
- **Parameter (`a`,`b`) problems:** eliminate symbolically, find which pivot vanishes as a function of the parameter, then check the corresponding row's RHS for consistency.
- **Vector space / subspace check:** zero vector present? closed under addition? closed under scaling? All three "yes" → subspace. A homogeneous linear equation (`=0`) always describes a subspace; anything with a nonzero constant does not.
- **Linear independence:** try to write one vector as a combination of the others, or solve `c₁v₁+⋯+cₖvₖ=0` for the `c_i`.
- **Span of `n` vectors in `ℝⁿ`:** compute the determinant; nonzero ⟺ independent ⟺ spans `ℝⁿ`.
- **Four subspaces + rank-nullity:** row-reduce for rank `r`; column space = original pivot columns; row space = pivot rows (dimension `r` each); null space from free variables in RREF (`n−r` dimensional); left null space from dependency relations among the rows (`m−r` dimensional).
- **Linear transformation test:** check `T(0)=0` first (fast disqualifier); then check additivity/homogeneity fully if `T(0)=0` holds but the formula has products, absolute values, or added constants.
- **Matrix of a linear transformation:** apply `T` to each standard basis vector; those images become the columns.
- **Composed geometric transformations:** the operation applied *first* sits on the *right* in the matrix product.

---

# NOTES ON DISCREPANCIES FOUND

While working through this set rigorously, two of the printed answers didn't match my own step-by-step elimination:

1. **Problem 6:** the printed key gives "unique solution for `a≠2`" — my elimination shows the system is *also* inconsistent (for every `b`) at `a=1`, so the correct condition should be `a≠1 and a≠2`.
2. **Problem 7:** the printed key gives `b=4/9` as the critical value at `a=−5` — my elimination (following the equations exactly as transcribed) consistently gives `b=0` instead.

Neither of these affects the *method* — both derivations above show you exactly how to work through this style of parameter problem. It's worth flagging these two specific numbers to your professor or TA in case there's a transcription difference between what's on your sheet and the original problem.
