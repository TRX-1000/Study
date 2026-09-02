# Mathematical Foundation for AI & Data Science
## Complete Study Guide — Class 4 & Class 5
### Elementary Matrices • Triangular Factorization • Permutation Matrices • Inverses • Gauss–Jordan • Transpose • Symmetric Matrices

*(Source: PES University lecture slides, UE25MA25242A, Dr. Jyothi R. All notation, examples and methods below follow the lecture material exactly. Any method not in the slides is explicitly labeled "Additional method.")*

---

# PART 1 — GAUSSIAN ELIMINATION AND ELEMENTARY MATRICES

## 1.1 Why we need this at all

When you solve a system of linear equations `Ax = b` by hand, what you are really doing is combining rows to knock out variables one at a time until the system becomes "staircase-shaped" (triangular), which you can then solve by simple back-substitution. Gaussian elimination is the *systematic* version of this: a fixed recipe of row operations that always reduces a matrix to an upper triangular matrix `U`.

The key insight the lecture builds on: **every row operation is itself equivalent to multiplying by a special matrix.** This means the whole elimination process can be written as a *sequence of matrix multiplications*, which is what lets us later factor `A` as a product `LU`. This is the bridge between "doing elimination by hand" and "understanding elimination as linear algebra."

## 1.2 Elementary matrices — definition and construction

**Definition (as given in the slides):**
An elementary matrix `E_ij` is obtained from the identity matrix `I` by applying the row transformation
```
R_i ← R_i − l_ij R_j
```
where `l_ij` is the multiplier used to eliminate the entry in row `i`, column `j`. Symbolically: `I → E_ij`.

**How to build one, step by step:**
1. Start with the identity matrix of the correct size.
2. Decide which entry you want to eliminate (row `i`, column `j`).
3. Apply exactly that one row operation to the identity matrix.
4. The result is `E_ij`.

**Worked example (from the slides): construct `E₃₂`**

Start with `I` (3×3):
```
I = [1 0 0]
    [0 1 0]
    [0 0 1]
```
Apply `R₃ − 2R₂`:
- Row 3 of `I` is `(0, 0, 1)`. Row 2 is `(0, 1, 0)`.
- New Row 3 = `(0,0,1) − 2·(0,1,0)` = `(0, −2, 1)`.

```
E₃₂ = [1  0  0]
      [0  1  0]
      [0 −2  1]
```

**Why this matters:** `E₃₂` is not just a bookkeeping trick. If you left-multiply *any* matrix `M` by `E₃₂`, the effect on `M` is *exactly* the row operation `R₃ ← R₃ − 2R₂` performed on `M`. This is the fact that turns "row reduction" into "matrix multiplication."

## 1.3 Elimination matrices during Gaussian elimination

An **elimination matrix** is simply an elementary matrix used in the specific context of reducing `A` to `U`. The terms are used interchangeably in the slides — "elimination matrix" emphasizes the *purpose* (eliminating an entry during Gaussian elimination), while "elementary matrix" emphasizes the *construction* (built from `I`).

### Worked example — full elimination of a 3×3 matrix

Consider
```
A = [3  1  2]
    [2 −3 −1]
    [1  2  1]
```

**Step 1 — Eliminate column 1 below the pivot (pivot = 3, top-left).**

Row 2 has a `2` in column 1. To zero it out we need `R₂ − (2/3)R₁`. Row 3 has a `1` in column 1, so we need `R₃ − (1/3)R₁`.

*Why these particular multipliers?* The multiplier is always `(entry to be eliminated) ÷ (pivot)`. Here pivot = 3. For row 2: `2/3`. For row 3: `1/3`.

```
R₂ − 2/3 R₁
R₃ − 1/3 R₁
```
gives
```
[3    1     2  ]
[0  −11/3  −7/3]
[0   5/3    1/3]
```

**Step 2 — Eliminate column 2 below the new pivot (pivot = −11/3).**

Row 3 has `5/3` in column 2. Multiplier = `(5/3) ÷ (−11/3) = −5/11`, so the operation is `R₃ + (5/11)R₂` (subtracting a negative multiplier means adding).

```
R₃ + 5/11 R₂
```
gives
```
U = [3    1     2   ]
    [0  −11/3  −7/3 ]
    [0    0   −8/11 ]
```

**Step 3 — Write down the elimination matrices used.**

- `E = E₂₁ = R₂ − (2/3)R₁` →
```
E₂₁ = [ 1    0  0]
      [−2/3  1  0]
      [ 0    0  1]
```
- `F = E₃₁ = R₃ − (1/3)R₁` →
```
E₃₁ = [ 1   0 0]
      [ 0   1 0]
      [−1/3 0 1]
```
- `G = E₃₂ = R₃ + (5/11)R₂` →
```
E₃₂ = [1    0   0]
      [0    1   0]
      [0  5/11  1]
```

**Step 4 — The master equation.**
```
E₃₂ E₃₁ E₂₁ A = U
```
This equation is the entire content of Gaussian elimination written in matrix form: apply `E₂₁` first (leftmost operation happens first because it acts on `A` directly, and matrix multiplication order matters), then `E₃₁`, then `E₃₂`. **Order matters**: you always apply elimination matrices in the order the row operations were actually performed, and each new one multiplies on the *left* of the accumulated product so far.

### Common mistake to flag here
A very common error is writing `E₂₁ E₃₁ E₃₂ A = U` (reversed order). Since matrices generally don't commute, this is wrong. The correct rule: **the matrix for the operation performed last is written leftmost.**

## 1.4 Two more worked "which elimination matrices" problems (straight from the slides)

**Problem 1 — 4×4 tridiagonal-type matrix**
```
A = [ 2 −1  0  0]
    [−1  2 −1  0]
    [ 0 −1  2 −1]
    [ 0  0 −1  2]
```
- `R₂ + (1/2)R₁` → row 2 becomes `(0, 3/2, −1, 0)`
- `R₃ + (2/3)R₂` → row 3 becomes `(0, 0, 4/3, −1)`
- `R₄ + (3/4)R₃` → row 4 becomes `(0, 0, 0, 5/4)`

Final: `U = [2 −1 0 0; 0 3/2 −1 0; 0 0 4/3 −1; 0 0 0 5/4]`

The elimination matrices `E₂₁, E₃₂, E₄₃` (built exactly as in §1.2) satisfy `E₄₃ E₃₂ E₂₁ A = U`. Notice the multipliers `1/2, 2/3, 3/4` appear as the sub-diagonal entries of each `E`, and (as you'll see in Part 2) these same numbers reappear directly inside `L`.

**Problem 2 — a matrix with a zero pivot column that still eliminates fine (3×5 case)**
```
A = [0  2 −6 −2  4]
    [0 −1  3  3  2]
    [0 −1  3  7 10]
```
Here the *pivot* for elimination is taken as the `2` in row 1, column 2 (column 1 is entirely zero, so elimination targets column 2 first).
- `R₂ + (1/2)R₁`, `R₃ + (1/2)R₁` → `[0 2 −6 −2 4; 0 0 0 2 4; 0 0 0 6 12]`
- `R₃ − 3R₂` → `[0 2 −6 −2 4; 0 0 0 2 4; 0 0 0 0 0] = U`

Elimination matrices: `E₂₁ = [1 0 0; 1/2 1 0; 0 0 1]`, `E₃₁ = [1 0 0; 0 1 0; 1/2 0 1]`, `E₃₂ = [1 0 0; 0 1 0; 0 −3 1]`.

**Lesson:** elementary/elimination matrices are always built the same size as `A`'s row space (here 3×3, acting on the rows of a 3×5 matrix), and the whole procedure works identically even when `A` is not square.

## 1.5 Exam perspective
- **HIGH PRIORITY:** "Find the elimination/elementary matrices that reduce A to U" — this appears directly as a problem type in the slides (twice). Practice writing each `E_ij` immediately after you decide on a row operation, don't do all row operations first and try to reconstruct them later.
- Know that `E_(last) E_(middle) ... E_(first) A = U` — order and left-multiplication.
- Know how to read off the multiplier for `R_i ← R_i − l_ij R_j` directly from the matrix (it's `entry / pivot`).

## 1.6 Common mistakes
- Reversing the order of multiplication of the `E` matrices.
- Writing `+l_ij` instead of `−l_ij` inside `E_ij`, or vice versa, when the row operation was addition (i.e. the multiplier itself was negative — this trips people up constantly, see Problem 1 above where the operations are additions, not subtractions, and the sub-diagonal entries of `E` are *negative* of the number in the arrow, e.g. `R₂+(1/2)R₁` gives `E₂₁` with `−1/2`, not `+1/2`, in that slot... **wait, correction**: In this specific lecture's convention, look at the general definition again: `E_ij` comes from `R_i − l_ij R_j`. If the actual operation was `R₂ + (1/2)R₁`, that means `l_21 = −1/2`, and the entry placed in `E₂₁` position (2,1) is `l_21 = −1/2`. Always match the sign to the definition `R_i − l_ij R_j`, don't just copy the sign shown in the arrow.
- Forgetting that elimination matrices are always identity matrices with **one single off-diagonal nonzero entry** — if you compute an `E` matrix with two changed entries, you've made an error.

---

# PART 2 — TRIANGULAR FACTORIZATION

## 2.1 Triangular matrices — the basics

- **Upper triangular matrix `U`**: all entries *below* the main diagonal are zero.
- **Lower triangular matrix `L`**: all entries *above* the main diagonal are zero.

Triangular matrices are useful because a linear system with a triangular coefficient matrix can be solved directly by substitution (no further elimination needed) — upper triangular by **back substitution** (solve bottom equation first, then work up), lower triangular by **forward substitution** (solve top equation first, then work down).

## 2.2 From elimination to factorization: A = LU

Recall from §1.3: `E₃₂E₃₁E₂₁A = U`. To "undo" elimination and get back to `A`, we need the **inverses** of the elimination matrices:
```
E₂₁⁻¹E₃₁⁻¹E₃₂⁻¹ U = A
```
The inverse of an elementary matrix `E_ij` is obtained just by **flipping the sign of the multiplier** (changing `−l` to `+l` in that one off-diagonal slot) — this is intuitive because undoing "subtract `l` times row `j`" is "add `l` times row `j`".

Define:
```
L = E₂₁⁻¹E₃₁⁻¹E₃₂⁻¹
```

**Important direction rule** (explicitly flagged in the slides): going from `A → U` you multiply `E₃₂E₃₁E₂₁`. Going back from `U → A` you must apply the inverses **in the reverse order**: `E₂₁⁻¹E₃₁⁻¹E₃₂⁻¹`. This reversal is exactly why `L` collects the multipliers in a clean triangular pattern with 1's on the diagonal.

## 2.3 Formal definition of A = LU

Any square matrix `A` (that doesn't require row exchanges) can be factored as
```
A = LU
```
where
- `L` is **lower triangular**, with **1's on the diagonal**, and has the multipliers `l_ij` sitting *below* the diagonal in exactly the position where they were used during elimination.
- `U` is **upper triangular**, with the **pivots** `u₁₁, u₂₂, u₃₃, …` on the diagonal.

```
L = [ 1    0    0 ]        U = [u₁₁  u₁₂  u₁₃]   = [d₁  u₁₂  u₁₃]
    [l₂₁   1    0 ]            [ 0   u₂₂  u₂₃]     [0   d₂   u₂₃]
    [l₃₁  l₃₂   1 ]            [ 0    0   u₃₃]     [0    0   d₃]
```

**Key shortcut:** you never have to compute `L` separately by inverting each `E`. The entries of `L` below the diagonal are *exactly* the multipliers you used during elimination, placed in the same row/column position, with **no sign change** (unlike the `E` matrices, which had the negative of the multiplier).

## 2.4 Worked example — deriving L and U from scratch

Using the same `A` as before:
```
A = [3  1  2]
    [2 −3 −1]
    [1  2  1]
```
**Elimination (repeated from §1.3):**
- `R₂ − (2/3)R₁`, `R₃ − (1/3)R₁` → `[3 1 2; 0 −11/3 −7/3; 0 5/3 1/3]`
- `R₃ + (5/11)R₂` → `U = [3 1 2; 0 −11/3 −7/3; 0 0 −8/11]`

**Building L directly from the multipliers used** (no sign flip — this is the shortcut):
- Multiplier for `R₂` from `R₁`: `2/3` → goes in `L` position `(2,1)`.
- Multiplier for `R₃` from `R₁`: `1/3` → goes in `L` position `(3,1)`.
- Multiplier for `R₃` from `R₂`: `−5/11` (because the operation used was `R₃ + (5/11)R₂`, i.e. `l₃₂ = −5/11`) → goes in `L` position `(3,2)`.

```
L = [ 1    0    0]
    [2/3   1    0]
    [1/3 −5/11  1]
```

**Verification:**
```
A = LU = [ 1    0    0] [3    1     2 ]
         [2/3   1    0] [0  −11/3  −7/3]
         [1/3 −5/11  1] [0    0   −8/11]
```
Multiplying this out reproduces the original `A` exactly — this is your check that the factorization is correct. (Multiplying row 1 of L by U gives row 1 of A directly since row 1 of L is `(1,0,0)`; multiplying row 2 of L, `(2/3, 1, 0)`, by U's columns reproduces row 2 of A; and similarly for row 3 — you should always do this check on at least one row when solving exam problems.)

## 2.5 The A = LDU factorization (symmetric form)

`A = LU` is **not symmetric in structure**: `L` has 1's on its diagonal, but `U` has the actual pivots on its diagonal. To fix this asymmetry, the lecture introduces:
```
A = LDU
```
where now:
- `D` is a diagonal matrix holding the pivots `d₁, d₂, d₃` (same numbers that were on `U`'s diagonal before).
- `L` is unchanged from before (1's on diagonal, multipliers below).
- `U` here is redefined to also have **1's on the diagonal** — obtained by dividing every row of the old `U` by its own pivot.

```
D = [d₁ 0  0 ]      U(new) = [1  u₁₂/u₁₁  u₁₃/u₁₁]
    [0  d₂ 0 ]               [0     1     u₂₃/u₂₂]
    [0  0  d₃]               [0     0        1   ]
```

### Worked example — full A = LU and A = LDU derivation (4×4)

```
A = [ 2 −1  0  0]
    [−1  2 −1  0]
    [ 0 −1  2 −1]
    [ 0  0 −1  2]
```
**Elimination:**
- `R₂ + (1/2)R₁` → row 2: `(0, 3/2, −1, 0)`
- `R₃ + (2/3)R₂` → row 3: `(0, 0, 4/3, −1)`
- `R₄ + (3/4)R₃` → row 4: `(0, 0, 0, 5/4)`

```
U = [2 −1   0    0 ]
    [0 3/2 −1    0 ]
    [0  0  4/3  −1 ]
    [0  0   0   5/4]
```
**L** (multipliers, remembering `R₂+(1/2)R₁` means `l₂₁ = −1/2`; similarly `l₃₂ = −2/3`, `l₄₃ = −3/4`):
```
L = [ 1    0    0   0]
    [−1/2  1    0   0]
    [ 0  −2/3   1   0]
    [ 0    0  −3/4  1]
```
So `A = LU`.

**For A = LDU**: `D = diag(2, 3/2, 4/3, 5/4)`, and dividing each row of `U` by its own pivot:
```
U(new) = [1 −1/2   0    0 ]
         [0   1  −2/3   0 ]
         [0   0    1  −3/4]
         [0   0    0    1 ]
```
so `A = LDU`.

## 2.6 Solving Ax = b using triangular factorization

**Procedure (from the slides):**
1. Factor `A = LU`.
2. Since `Ax = LUx = b`, define an intermediate vector `c` by `Ux = c`.
3. Then `Lc = b`. Solve this by **forward substitution** (fast, since `L` is lower triangular).
4. Once you have `c`, solve `Ux = c` by **back substitution** to get `x`.

This is more efficient than computing `A⁻¹` directly, especially if you need to solve `Ax = b` for several different `b`'s with the same `A` — you only factor `A` once.

### Worked example
```
L = [1 0 0]   U = [2 4 4]   b = [2]
    [1 1 0]       [0 1 2]       [0]
    [1 0 1]       [0 0 1]       [2]
```
**Step 1 — solve `Lc = b` by forward substitution:**
```
[1 0 0][c₁]   [2]
[1 1 0][c₂] = [0]
[1 0 1][c₃]   [2]
```
- Row 1: `c₁ = 2`
- Row 2: `c₁ + c₂ = 0` → `c₂ = −2`
- Row 3: `c₁ + c₃ = 2` → `c₃ = 0`

**Step 2 — solve `Ux = c` by back substitution:**
```
[2 4 4][x]   [2]
[0 1 2][y] = [−2]
[0 0 1][z]   [0]
```
- Row 3: `z = 0`
- Row 2: `y + 2z = −2` → `y = −2`
- Row 1: `2x + 4y + 4z = 2` → `2x − 8 = 2` → `x = 5`

**Answer: `(x, y, z) = (5, −2, 0)`.** Note the slides explicitly emphasize doing this *without* multiplying `L` and `U` back together to recover `A` — that would defeat the purpose.

### Second full worked example (A = LU and A = LDU together, 4×4, non-symmetric)
```
A = [ 6  −2  −4   4]
    [ 3  −3  −6   1]
    [−12  8  21  −8]
    [−6   0 −10   7]
```
- `R₂ − (1/2)R₁`, `R₃ + 2R₁`, `R₄ + R₁`:
```
[6  −2  −4  4]
[0  −2  −4 −1]
[0   4  13  0]
[0  −2 −14 11]
```
- `R₃ + 2R₂`, `R₄ − R₂`:
```
[6  −2  −4  4]
[0  −2  −4 −1]
[0   0   5 −2]
[0   0 −10 12]
```
- `R₄ + 2R₃`:
```
U = [6  −2  −4  4]
    [0  −2  −4 −1]
    [0   0   5 −2]
    [0   0   0  8]
```
Reading off multipliers directly:
```
L = [ 1    0    0  0]
    [1/2   1    0  0]
    [−2   −2    1  0]
    [−1    1   −2  1]
```
`A = LU` ✓. For `A = LDU`: `D = diag(6, −2, 5, 8)` and
```
U(new) = [1 −1/3 −2/3  2/3]
         [0   1    2   1/2]
         [0   0    1  −2/5]
         [0   0    0    1 ]
```

## 2.7 Exam perspective
- **HIGH PRIORITY:** "Find A = LU (and hence A = LDU)" is a direct, repeated question type in the slides. Practice extracting `L` directly from multipliers (don't invert `E` matrices manually — it's slower and error-prone).
- Verify your answer by re-multiplying `LU` (or checking one row) — this is explicitly worth doing on an exam if time permits.
- Know how `A = LU` is used to solve `Ax = b` via `Lc = b` then `Ux = c`.

## 2.8 Common mistakes
- Sign errors when reading multipliers into `L` — remember, unlike `E_ij`, **L uses the multiplier with no sign flip**, because it's already the *inverse* direction.
- Confusing `A = LU` (unsymmetric, pivots on U's diagonal) with `A = LDU` (symmetric-looking, pivots pulled out into D, both L and U have 1's on the diagonal).
- Trying to solve `Ax=b` by first multiplying `L` and `U` back into `A` — defeats the purpose and wastes exam time.

---

# PART 3 — PERMUTATION MATRICES

## 3.1 Why they're needed

Sometimes, during elimination, you land on a **zero in the pivot position** — but there's a nonzero entry in a row below it. Plain Gaussian elimination as described in Parts 1–2 *fails* here (you can't divide by zero to compute a multiplier). The fix is to **swap rows** so a nonzero entry sits in the pivot position. Permutation matrices are the formal, matrix-multiplication way of representing this row swap.

## 3.2 Definition and construction

**A permutation matrix `P` is the identity matrix `I` with its rows reordered.** Left-multiplying any matrix `M` by `P` performs exactly that row reordering on `M`.

**Worked example (from the slides):** Consider the system `y = b₁`, `2x − 3y = b₂`, i.e.
```
Ax = b  ⟹  [0  1][x]   [b₁]
            [2  3][y] = [b₂]
```
Gaussian elimination fails immediately here because the (1,1) entry is `0`. The fix is a row exchange:
```
[2  3][x]   [b₂]
[0  1][y] = [b₁]
```
This is the same as pre-multiplying both sides of the original system by
```
P = [0 1]
    [1 0]
```
Check: `PA = [0 1; 1 0][0 1; 2 3] = [2 3; 0 1] = U`, and `Pb = [0 1; 1 0][b₁; b₂] = [b₂; b₁]`.
So `PAx = Pb` gives exactly the swapped system above.

## 3.3 Properties of permutation matrices

- The **product of two permutation matrices is also a permutation matrix.**
- The **inverse of a permutation matrix is also a permutation matrix.**
- `P⁻¹ = Pᵀ` **always** (this is a very testable fact — permutation matrices are *orthogonal*).
- The number of distinct `n×n` permutation matrices is `n!` (e.g. order 2 → `2! = 2`; order 3 → `3! = 6`).

**Order 2 (2 total):**
```
I = [1 0]     P₂₁ = [0 1]
    [0 1]           [1 0]
```

**Order 3 (6 total, from the slides):**
```
I  = [1 0 0]     P₂₁ = [0 1 0] = P₂₁⁻¹     P₃₁ = [0 0 1] = P₃₁⁻¹     P₃₂ = [1 0 0] = P₃₂⁻¹
     [0 1 0]           [1 0 0]                    [0 1 0]                    [0 0 1]
     [0 0 1]           [0 0 1]                    [1 0 0]                    [0 1 0]

P₂₁P₃₁ = [0 1 0]  = (P₂₁P₃₂)⁻¹        P₂₁P₃₂ = [0 0 1] = (P₂₁P₃₂)⁻¹  (note: slide labels both products' inverses this way — the two 3-cycle permutation matrices are inverses of each other)
          [0 0 1]                               [1 0 0]
          [1 0 0]                               [0 1 0]
```

## 3.4 Permutation matrices inside triangular factorization: PA = LU

When ordinary elimination fails due to a zero pivot, we can't write `A = LU` directly — but after reordering rows with `P`, elimination succeeds on `PA`, giving:
```
PA = LU
```

**Worked example (from the slides):**
```
A = [2 3 3]
    [6 9 8]
    [0 5 7]
```
- `R₂ − 3R₁` → `[2 3 3; 0 0 −1; 0 5 7]`. Now the pivot in row 2, column 2 is `0`, but row 3 has a `5` there.
- Swap `R₂ ↔ R₃` → `[2 3 3; 0 5 7; 0 0 −1] = U`

If you naively try `LU = [1 0 0; 3 1 0; 0 0 1][2 3 3; 0 5 7; 0 0 −1]`, the result **does not equal A** — because a row swap happened partway through and wasn't accounted for.

**Correct approach:** apply the permutation to `A` *first*, then eliminate normally.
```
P₂₃A = [1 0 0][2 3 3]   [2 3 3]
       [0 0 1][6 9 8] = [0 5 7]
       [0 1 0][0 5 7]   [6 9 8]
```
Now eliminate: `R₃ − 3R₁` → `[2 3 3; 0 5 7; 0 0 −1] = U`.
```
L = [1 0 0]
    [0 1 0]
    [3 0 1]
```
And now `LU = P₂₃A` ✓ — this is the correct factorization.

## 3.5 Exam perspective
- **HIGH PRIORITY:** know `P⁻¹ = Pᵀ` cold — it's stated as a standalone bullet in the slides and is a favorite quick-mark question.
- Know how to construct `P` for a given row swap (it's just `I` with those two rows exchanged).
- Know the distinction: **plain `A = LU` fails when elimination hits a zero pivot with no way to avoid it without swapping; then you need `PA = LU` instead.**

## 3.6 Common mistakes
- Trying to read off `L` from an elimination sequence that included a row swap, without first pre-multiplying `A` by `P`. This produces an `L` that does **not** satisfy `LU = A` (as shown explicitly above).
- Confusing `n!` (number of permutation matrices) with `n` (matrix size).

---

# PART 4 — TRIANGULAR FACTORS AND ROW EXCHANGES

## 4.1 What changes when a row exchange is genuinely necessary

If, partway through elimination, the current pivot position is zero **and every entry below it in that column is also zero**, no row swap can fix it (the matrix is singular in that direction, or a swap is only possible with a row that has a nonzero entry — if none exists, elimination truly fails / A has no LU-type factorization at all in that configuration). If instead a nonzero entry exists somewhere below, a swap always fixes it, and the whole factorization becomes `PA = LU` instead of `A = LU`, exactly as in Part 3.

## 4.2 Worked example where a swap is truly required

**Problem (from the slides):** Explain why `A` below is not factorizable into `LU`. Modify `A` so that the new matrix can be factored, then find `L, D, U`.
```
A = [ 1 −2  2]
    [ 2 −4  5]
    [−2  5 −4]
```
- `R₂ − 2R₁`, `R₃ + 2R₁`:
```
[1 −2 2]
[0  0 1]
[0  1 0]
```
Pivot position (2,2) is `0`, but row 3 has a `1` there — **a row exchange is required.** Plain Gaussian elimination fails on `A` as given.

**Fix:** multiply `A` by the permutation matrix `P₂₃` first:
```
P₂₃A = [1 −2  2]
       [−2  5 −4]
       [2  −4  5]
```
Now eliminate normally: `R₂ + 2R₁`, `R₃ − 2R₁`:
```
U = [1 −2 2]
    [0  1 0]
    [0  0 1]
```
```
L = [1  0 0]
    [−2 1 0]
    [2  0 1]
```
`D = I` (all pivots are 1). Here `U = L^T` for this particular problem — a relationship the lecturer explicitly points out in this worked example (row-reduced upper-triangular result happens to be the transpose of the lower-triangular multiplier matrix here). Don't assume `L = Uᵀ` holds in general — it is a feature of this specific example, not a universal rule (in general it holds when `A` itself is symmetric — see Part 8 for the general version of this relationship, `A = LDLᵀ`).

## 4.3 Exam perspective
- **HIGH PRIORITY question type:** "Explain why A is not factorizable into LU, fix it, and find L, D, U for the fixed matrix." This is a full worked problem in the slides and is very likely to reappear in some form.
- Always show the *failed* elimination attempt first (to justify *why* a swap is needed) before showing the corrected `PA = LU`.

## 4.4 Common mistakes
- Skipping the step of showing why plain elimination fails (a common exam-marking point).
- Forgetting to apply `P` to `A` *before* re-doing elimination (see §3.4 for what goes wrong if you don't).

---

# PART 5 — INVERSE OF A MATRIX

## 5.1 Definition and intuition

For a square matrix `A` of order `n`, the **inverse** is the matrix `B` such that
```
AB = I = BA
```
Here `B = A⁻¹`. Intuitively: `A⁻¹` is the matrix that exactly "undoes" whatever linear transformation `A` represents — applying `A` then `A⁻¹` (in either order) gets you back to where you started (the identity transformation).

## 5.2 When does an inverse exist?

- `A` is **invertible (nonsingular)** if and only if elimination produces **n pivots** (a full set, one per row/column), with or without row exchanges.
- `A` is **singular (non-invertible)** if elimination produces fewer than `n` pivots — i.e., you hit a row of all zeros with no way to fix it via a swap.
- **Not every matrix has an inverse.** Only square matrices can be invertible, and even among square matrices, singular ones are not.

## 5.3 Properties of inverses (from the slides)

- **Uniqueness:** if `AB = BA = I` and `AC = CA = I`, then `B = C` — the inverse, if it exists, is unique.
- **Inverse of a product = product of inverses, in reverse order:**
```
(ABCD)⁻¹ = D⁻¹C⁻¹B⁻¹A⁻¹
```
- **If `A = LU`, then `A⁻¹ = U⁻¹L⁻¹`** (a direct application of the reverse-order rule above).
- Since `E₃₂E₃₁E₂₁A = U`, we also have `E₂₁⁻¹E₃₁⁻¹E₃₂⁻¹U = A`, so `A⁻¹ = U⁻¹E₃₂E₃₁E₂₁`, and correspondingly `L = E₂₁⁻¹E₃₁⁻¹E₃₂⁻¹`.
- If `A` is invertible, `Ax = b` has **exactly one solution**, given directly by `x = A⁻¹b`.
- Elimination itself can solve `Ax = b` **without ever explicitly computing `A⁻¹`** — this is usually faster, and is the practical reason triangular factorization (Part 2) is preferred over inverse computation for solving systems.

## 5.4 Singular vs nonsingular — quick check

| | Singular | Nonsingular |
|---|---|---|
| Pivots from elimination | fewer than n | exactly n |
| Inverse exists? | No | Yes |
| Solutions to Ax=b | none or infinitely many | exactly one, x = A⁻¹b |

## 5.5 Exam perspective
- **HIGH PRIORITY:** `(ABCD)⁻¹ = D⁻¹C⁻¹B⁻¹A⁻¹` — reversal rule for inverses of products, tested constantly across linear algebra courses.
- Know the **pivot criterion** for invertibility cold: n pivots ⟺ invertible.
- Watch matrix dimensions and multiplication order carefully whenever combining inverses.

## 5.6 Common mistakes
- Writing `(AB)⁻¹ = A⁻¹B⁻¹` (forgetting the order reversal) — this is one of the single most common linear algebra errors.
- Assuming every square matrix has an inverse.

---

# PART 6 — FINDING THE INVERSE USING GAUSS–JORDAN

## 6.1 The algorithm, and why it works

**Procedure:**
1. Start with matrix `A` (n×n, invertible).
2. Form the augmented matrix `[A : I]`.
3. Perform elementary row operations on the *entire augmented row* (both halves at once) so that the left block `A` is reduced first to echelon form `U`, and simultaneously the right block `I` becomes some matrix `C`. This gives `[U : C]`.
4. Continue reducing — now working `U` back up into `I` (this second phase uses back-substitution-style row operations) — while applying the same operations to `C`. This turns `C` into `A⁻¹`. Final result: `[I : A⁻¹]`.

**Why this works:** every row operation you perform corresponds to left-multiplying by some elementary matrix `E_k`. If you apply a whole sequence `E_final ⋯ E_2 E_1` to `[A : I]`, you get `[E_final⋯E_1 A : E_final⋯E_1 I]`. By construction, the sequence of operations is chosen precisely so that `E_final⋯E_1 A = I`. But that means `E_final⋯E_1 = A⁻¹` (multiplying both sides of `E_final⋯E_1 A = I` by `A⁻¹` on the right, or just recognizing that the matrix that turns `A` into `I` **is** `A⁻¹` by definition). So the same sequence, applied to the right-hand `I` block, must produce `E_final⋯E_1 · I = A⁻¹`. That is exactly why the right block ends up as `A⁻¹`.

Symbolically: `[A : I] → [U : C] → [I : A⁻¹]`.

## 6.2 Worked example 1 (full, from the slides)

Find `A⁻¹` for
```
A = [ 2 1 1]
    [ 4 3 4]
    [−2 2 2]
```
**Set up augmented matrix:**
```
[A:I] = [2 1 1 : 1 0 0]
        [4 3 4 : 0 1 0]
        [−2 2 2 : 0 0 1]
```
**`R₂ − 2R₁`, `R₃ + R₁`:**
```
[2 1 1 : 1  0 0]
[0 1 2 : −2 1 0]
[0 3 3 : 1  0 1]
```
**`R₃ − 3R₂`:**
```
[U:C] = [2 1  1 : 1  0  0]
        [0 1  2 : −2 1  0]
        [0 0 −3 : 7 −3  1]
```
This completes the forward pass — `A` is now in upper-triangular form `U`.

**`R₁ + (1/3)R₃`, `R₂ + (2/3)R₃`:**
```
[2 1 0 : 10/3 −1  1/3]
[0 1 0 : 8/3  −1  2/3]
[0 0 −3: 7    −3   1 ]
```
**`R₁ − R₂`:**
```
[2 0 0 : 2/3  0 −1/3]
[0 1 0 : 8/3 −1  2/3]
[0 0 −3: 7   −3   1 ]
```
**`R₁ = (1/2)R₁`, `R₃ = (−1/3)R₃`:**
```
[I : B] = [1 0 0 : 1/3  0 −1/6]
          [0 1 0 : 8/3 −1  2/3]
          [0 0 1 : −7/3 1 −1/3]
```
So:
```
A⁻¹ = B = [ 1/3   0  −1/6]
          [ 8/3  −1   2/3]
          [−7/3   1  −1/3]
```
**How to verify:** multiply `A · B`; you should get `I`. (On an exam, verifying at least one row of `AB = I` is strong practice and demonstrates the answer is correct even if you don't have time to check the whole product.)

## 6.2b Note on explicit row-operation notation

Every step above can equivalently be written using standard row-operation notation, as requested:
- `R₂ ← R₂ − 2R₁`
- `R₃ ← R₃ + R₁`
- `R₃ ← R₃ − 3R₂`
- `R₁ ← R₁ + (1/3)R₃`
- `R₂ ← R₂ + (2/3)R₃`
- `R₁ ← R₁ − R₂`
- `R₁ ← R₁ / 2`
- `R₃ ← R₃ / (−3)`

## 6.3 Worked example 2 (structurally the same problem seen from Part 2's perspective — using LU to sanity-check)

The matrix `A` in §2.4,
```
A = [3  1  2]
    [2 −3 −1]
    [1  2  1]
```
can equally have its inverse computed via Gauss–Jordan.

*(**Additional method** — this second example is provided by extending the lecture's own worked matrix to a full inverse computation, since the slides only carried this particular `A` through the LU factorization, not the full Gauss–Jordan process. All row operations follow the exact same algorithm as §6.1.)*

`[A : I] = [3 1 2 : 1 0 0; 2 −3 −1 : 0 1 0; 1 2 1 : 0 0 1]`

`R₂ − (2/3)R₁`, `R₃ − (1/3)R₁`:
`[3 1 2 : 1 0 0; 0 −11/3 −7/3 : −2/3 1 0; 0 5/3 1/3 : −1/3 0 1]`

`R₃ + (5/11)R₂`:
`[3 1 2 : 1 0 0; 0 −11/3 −7/3 : −2/3 1 0; 0 0 −8/11 : −7/11 5/11 1]`

From here, continue the upward pass (normalize the last pivot, clear above it, normalize each row) exactly as in §6.1's second phase to reach `[I : A⁻¹]`. This mirrors the `U` and `L` you already found in §2.4 — a useful cross-check: **the multipliers you use in the Gauss–Jordan forward pass are identical to the multipliers that build `L` in the `A = LU` factorization**, because both processes perform the *same* forward elimination; Gauss–Jordan simply continues further (backward pass) to reach the inverse, instead of stopping at `U`.

## 6.4 Recognizing when A has no inverse

While performing Gauss–Jordan, if at any point an **entire row of the left block (`A`'s side) becomes all zeros** before you've used all `n` pivots, `A` is singular and **does not have an inverse** — stop; no amount of further row operations will produce `I` on the left.

## 6.5 Exam perspective
- **HIGH PRIORITY:** the full two-phase Gauss–Jordan procedure `[A:I] → [U:C] → [I:A⁻¹]` — expect at least one full 3×3 numeric problem.
- Know to state row operations explicitly (`R₂ ← R₂ − 2R₁` style) as you go — most marking schemes give partial credit per correctly identified operation.
- Practice recognizing failure (a zero row appearing) as "no inverse exists," rather than trying to push through.

## 6.6 Common mistakes
- Arithmetic slips in fractions (this example uses many thirds/elevenths — go slowly and double check each row).
- Stopping at `[U : C]` and treating `C` as if it were already `A⁻¹` — it is not; the backward pass is required.
- Applying a row operation to only the left half of the augmented matrix and forgetting to apply it to the right half too.

---

# PART 7 — TRANSPOSE OF A MATRIX

## 7.1 Definition

If `A = [a_ij]` is an `m×n` matrix, its **transpose** `Aᵀ = [a_ji]` is the `n×m` matrix obtained by turning rows into columns (equivalently, columns into rows).

## 7.2 Worked examples

**2×3 → 3×2:**
```
A = [2  1 −3]        Aᵀ = [ 2  4]
    [4  2  0]  (2×3)      [ 1  2]     (3×2)
                           [−3  0]
```
Row 1 of `A`, `(2, 1, −3)`, becomes column 1 of `Aᵀ`. Row 2 of `A`, `(4, 2, 0)`, becomes column 2 of `Aᵀ`.

**Additional small examples (2×2 and 3×2), Additional method (extending the slide's pattern for completeness):**
```
A = [1 2]   Aᵀ = [1 3]         A = [1 4]   Aᵀ = [1 2 5]
    [3 4]        [2 4]  (2×2)      [2 5]        [4 5 6]  (2×3)
                                    [4 6] (3×2)
```

## 7.3 Properties (from the slides)

- **Transpose of a lower triangular matrix is upper triangular** (and vice versa) — a direct consequence of "rows become columns": if all entries above the diagonal were zero, after transposing they're now below the diagonal, and vice versa.
- `(Aᵀ)ᵀ = A` — transposing twice returns the original.
- `(AB)ᵀ = BᵀAᵀ` — **note the order reversal**, same pattern as inverses.
- `(A⁻¹)ᵀ = (Aᵀ)⁻¹` — transpose and inverse commute (in the sense that it doesn't matter which you do first, as long as you're consistent about which operation's result you're taking).
- `(A ± B)ᵀ = Aᵀ ± Bᵀ` — transpose distributes over addition/subtraction.
- `(A⁻¹)ᵀAᵀ = (AA⁻¹)ᵀ = I` — a combination check, confirms `Aᵀ` and `(A⁻¹)ᵀ` are inverses of each other, consistent with the bullet above.

**Numerical check of `(AB)ᵀ = BᵀAᵀ`** *(Additional method — small illustrative example not in the slides, added to let you verify the rule yourself)*:
```
A = [1 2]   B = [0 1]
    [3 4]       [1 0]

AB = [2 1]        (AB)ᵀ = [2 4]
     [4 3]                [1 3]

Aᵀ = [1 3]   Bᵀ = [0 1]
     [2 4]        [1 0]

BᵀAᵀ = [0 1][1 3] = [2 4]   ✓ matches (AB)ᵀ
        [1 0][2 4]   [1 3]
```

## 7.4 Exam perspective
- **HIGH PRIORITY:** the reversal rule `(AB)ᵀ = BᵀAᵀ` — combined with the analogous inverse rule, this pairing is a classic "state and verify with an example" question.
- Know the triangular-matrix transpose fact — it's used again in Part 8's symmetric-matrix material.

## 7.5 Common mistakes
- Writing `(AB)ᵀ = AᵀBᵀ` (forgetting the reversal).
- Confusing dimensions: if `A` is `m×n`, `Aᵀ` is `n×m` — always re-check dimensions before multiplying two transposed matrices.

---

# PART 8 — SYMMETRIC MATRICES

## 8.1 Definition

A square matrix `A` (order `n`) is **symmetric** if
```
Aᵀ = A
```
i.e., the matrix is its own transpose — equivalently, `a_ij = a_ji` for every `i, j` (the matrix is a mirror image of itself across the main diagonal).

**Example (from the slides):**
```
A = [ 2 −3]     Aᵀ = [ 2 −3]     A = Aᵀ  →  symmetric
    [−3  5]          [−3  5]
```

## 8.2 Properties (from the slides)

- If `A` is symmetric, `A⁻¹` **may or may not exist** (symmetry alone doesn't guarantee invertibility — you still need `n` pivots).
- **If `A⁻¹` exists for a symmetric `A`, then `A⁻¹` is also symmetric.**
- For a symmetric `A`: `(A⁻¹)ᵀ = (A)⁻¹` (follows directly from `Aᵀ = A` plugged into the general rule `(A⁻¹)ᵀ = (Aᵀ)⁻¹` from Part 7).

## 8.3 Symmetric products: AAᵀ, AᵀA, and LDLᵀ

**Key fact:** for *any* matrix `A` (not necessarily square), both `AAᵀ` and `AᵀA` are always symmetric.

**Worked example:**
```
A = [2  1 −3]  (2×3)
    [4  2  0]

AAᵀ = [2  1 −3][ 2  4]   = [4+1+9    8+2+0 ] = [14 10]
      [4  2  0][ 1  2]     [8+2+0  16+4+0 ]   [10 20]
                [−3  0]
```
```
AᵀA = [ 2  4][2  1 −3]   [ 4+16   2+8    −6+0]   [20  10  −6]
      [ 1  2][4  2  0] = [ 2+8    1+4    −3+0] = [10   5  −3]
      [−3  0]            [−6+0   −3+0     9+0]   [−6  −3   9]
```
Both results are symmetric (check: entry (1,2) = entry (2,1) in each), confirming the general fact above.

### A = LDLᵀ — the symmetric version of triangular factorization

If `A` is symmetric **and** `A = LDU` (no row exchanges needed), then a very clean relationship holds:
```
U = Lᵀ,  so  A = LDLᵀ
```
This is a direct consequence of `A` being symmetric — because the elimination steps used to build `U` are "mirror images" of the steps used to build `L`, when `A = Aᵀ` to begin with.

**Worked example (from the slides):**
```
A = [ 1  2 −1]
    [ 2  3  0]
    [−1  0  4]
```
(Check: this `A` is symmetric — `a₁₂=2=a₂₁`, `a₁₃=−1=a₃₁`, `a₂₃=0=a₃₂`.)

`R₂ − 2R₁`, `R₃ + R₁`:
```
[1  2 −1]
[0 −1  2]
[0  2  3]
```
`R₃ + 2R₂`:
```
U = [1  2 −1]
    [0 −1  2]
    [0  0  7]
```
```
L = [ 1  0 0]
    [ 2  1 0]
    [−1 −2 1]
```
`D = diag(1, −1, 7)`.

Check `U = Lᵀ`: `Lᵀ = [1 2 −1; 0 1 −2; 0 0 1]`. Comparing to the *normalized* `U` (divided by pivots to get 1's on the diagonal, as required for the `LDU` form): `U(new) = [1 2 −1; 0 1 −2; 0 0 1]`. **These match exactly** — confirming `U = Lᵀ` and so `A = LDLᵀ`. ✓

## 8.4 Exam perspective
- **HIGH PRIORITY:** the identification test `Aᵀ = A` and being able to instantly check it on a given matrix.
- **HIGH PRIORITY:** `AAᵀ` and `AᵀA` are always symmetric, regardless of whether `A` itself is square or symmetric — a favorite "prove/verify with an example" question.
- **MEDIUM PRIORITY:** `A = LDLᵀ` for symmetric `A` — know the relationship `U = Lᵀ` and be ready to verify it numerically as shown above.

## 8.5 Common mistakes
- Checking only `a₁₂ = a₂₁` and assuming the whole matrix is symmetric without checking every pair.
- Assuming `A = LDLᵀ` holds for *any* factorization — it only holds when `A` itself is symmetric to begin with, and no row exchange was needed.

---

# COMPARISONS

## Gaussian Elimination vs Gauss–Jordan Elimination

| | Gaussian Elimination | Gauss–Jordan Elimination |
|---|---|---|
| **Goal** | Reduce `A` to upper triangular `U` | Reduce `A` all the way to identity `I` |
| **Row operations** | Only "forward pass" (eliminate below pivots) | Forward pass *and* backward pass (eliminate above pivots too), plus normalizing pivots to 1 |
| **Resulting form** | Upper triangular `U` | Identity `I` (with `A⁻¹` appearing on the augmented side) |
| **Typical use** | Solving `Ax=b` via back substitution; building `A=LU` | Computing `A⁻¹` explicitly |
| **Exam distinction** | Stop once triangular — don't over-reduce | Must continue all the way to `I` — stopping at `U` is an incomplete answer |

## Elementary Matrix vs Elimination Matrix vs Permutation Matrix

| | Elementary Matrix | Elimination Matrix | Permutation Matrix |
|---|---|---|---|
| **Definition** | `I` modified by one row operation `R_i − l_ij R_j` | Same as elementary matrix, used specifically during elimination | `I` with its rows reordered |
| **Purpose** | General building block linking row ops to matrix multiplication | Zero out a specific sub-pivot entry during Gaussian elimination | Handle a zero-pivot situation by swapping rows |
| **Construction** | Apply one row operation to `I` | Apply the specific elimination row operation to `I` | Reorder rows of `I` |
| **Effect on another matrix M** | Performs that one row operation on `M` | Performs that elimination step on `M` | Performs the row swap on `M` |
| **Example** | `E₃₂ = [1 0 0; 0 1 0; 0 −2 1]` | Same style, e.g. `E₂₁` in §1.3 | `P₂₁ = [0 1; 1 0]` |

*(Note: "elementary matrix" and "elimination matrix" are effectively the same object in this course — the second term is just used specifically in the elimination context. Permutation matrices are a special case of elementary matrices restricted to pure row swaps.)*

## Upper Triangular vs Lower Triangular Matrix

```
Upper Triangular U          Lower Triangular L
[u₁₁ u₁₂ u₁₃]                [l₁₁  0   0 ]
[ 0  u₂₂ u₂₃]                [l₂₁ l₂₂  0 ]
[ 0   0  u₃₃]                [l₃₁ l₃₂ l₃₃]
```
- Upper: all entries **below** the diagonal are zero. Solved by **back substitution**.
- Lower: all entries **above** the diagonal are zero. Solved by **forward substitution**.
- Transposing one type always gives the other type (Part 7).

## Singular vs Nonsingular Matrix

| | Singular | Nonsingular |
|---|---|---|
| **Meaning** | Matrix has fewer than `n` pivots | Matrix has a full set of `n` pivots |
| **Invertibility** | Not invertible | Invertible |
| **How to identify** | Elimination produces a zero row that can't be fixed by any row swap | Elimination (with or without row swaps) produces `n` nonzero pivots |
| **Numerical example** | `A = [2 c c; c c c; 8 7 c]` is singular exactly when `c = 0, 2,` or `7` (see §8.6-style problem below) | Any matrix reducing cleanly to `n` nonzero pivots, e.g. the `A` in §2.4 |

---

# PROBLEM-SOLVING SECTION — RELIABLE PROCEDURES

### "Perform Gaussian elimination"
1. Identify the current pivot (top-left nonzero entry of the remaining submatrix).
2. For each row below the pivot row, compute the multiplier = (entry to eliminate) ÷ (pivot).
3. Apply `R_i ← R_i − multiplier·R_pivot` to zero out that entry.
4. Move to the next pivot (next row/column) and repeat.
5. If you hit a zero pivot with a nonzero entry below it, swap rows (record this as a permutation matrix `P`) and continue.
6. Stop when the matrix is upper triangular = `U`.

### "Find an inverse using Gauss–Jordan"
1. Form `[A : I]`.
2. Forward pass: eliminate below each pivot (exactly like Gaussian elimination) to reach `[U : C]`.
3. Backward pass: eliminate above each pivot too, working from the bottom row upward.
4. Normalize: divide each row by its own pivot so the left block becomes `I`.
5. The right block is now `A⁻¹`.
6. If any left-block row becomes all zero at any point, `A` has no inverse — stop.

### "Find L and U" (and LDU)
1. Perform Gaussian elimination on `A`, recording every multiplier used.
2. Place each multiplier directly (no sign change) into `L` at the position `(row eliminated, row used)`.
3. `L` has 1's on the diagonal; the final triangular result is `U`.
4. For `LDU`: pull the diagonal entries of `U` out into a diagonal matrix `D`; divide each row of `U` by its own pivot to get a new `U` with 1's on the diagonal.
5. Verify by re-multiplying `LU` (or `LDU`) and comparing to `A`.

### "Construct an elementary matrix"
1. Start with the identity matrix of the same size as the system.
2. Decide the row operation `R_i ← R_i − l_ij R_j`.
3. Apply just that one operation to `I`.
4. The result is `E_ij`.

### "Construct a permutation matrix"
1. Start with the identity matrix.
2. Decide which two (or more) rows need to be swapped.
3. Reorder those rows in `I` accordingly.
4. The result is `P`; remember `P⁻¹ = Pᵀ`.

### "Determine whether a matrix is symmetric"
1. Compute `Aᵀ` (or just check entries: `a_ij` vs `a_ji` for every pair).
2. If `Aᵀ = A` exactly, `A` is symmetric.
3. If even one pair `a_ij ≠ a_ji`, it is not symmetric.

### "Determine whether an inverse exists"
1. Attempt Gaussian elimination (with row swaps as needed).
2. Count the pivots obtained.
3. If you get `n` pivots (full house), `A` is invertible.
4. If you hit a zero row that cannot be fixed by any swap, `A` is singular — no inverse.

---

# EXAM FOCUS

## HIGH PRIORITY
- Constructing elementary/elimination matrices `E_ij` from a stated row operation, and the master identity `E_(last)⋯E_(first) A = U`.
- Full derivation of `A = LU` (and `A = LDU`) from scratch, reading multipliers directly into `L`.
- Solving `Ax = b` via `Lc = b` (forward sub.) then `Ux = c` (back sub.).
- `P⁻¹ = Pᵀ`, and constructing `PA = LU` when a row exchange is required.
- The pivot criterion for invertibility (`n` pivots ⟺ invertible).
- `(ABCD)⁻¹ = D⁻¹C⁻¹B⁻¹A⁻¹` and `(AB)ᵀ = BᵀAᵀ` — order-reversal rules.
- Full Gauss–Jordan `[A:I] → [I:A⁻¹]` numeric problems.
- Symmetry test `Aᵀ = A`; the fact that `AAᵀ` and `AᵀA` are always symmetric.
- `A = LDLᵀ` for symmetric `A` (`U = Lᵀ`).

## MEDIUM PRIORITY
- Transpose of triangular matrices (upper ↔ lower).
- Counting permutation matrices (`n!`).
- Recognizing "why A is not factorizable into LU" style explanation questions.
- Singular-matrix parameter problems (finding values of `c` that make `A` singular).

## COMMON TRAPS
- **Sign errors** in elimination matrices vs. `L` (the sign convention differs between the two, as detailed in §1.6 and §2.8).
- **Incorrect row operations** — misreading which row is being modified vs. which is the source row.
- **Incorrect multiplication order** for both `(ABCD)⁻¹` and `(AB)ᵀ` — always reverse the order.
- **Dimension mismatches** when multiplying `Aᵀ` with other matrices — always double check shape before multiplying.
- **Confusing Gaussian elimination with Gauss–Jordan** — stopping too early or continuing too far depending on which was asked.
- **Incorrect inverse calculations** from arithmetic slips with fractions — go slowly, verify with `AB = I` on at least one row/column.
- **Incorrect transpose calculations**, especially forgetting that dimensions swap (`m×n → n×m`).

---

# PRACTICE QUESTIONS

## Conceptual Questions
1. Explain, in your own words, why every row operation used in Gaussian elimination can be represented as multiplication by an elementary matrix.
2. Why does `L` in `A = LU` always have 1's on its diagonal?
3. Explain the difference between `A = LU` and `A = LDU`. Why is the second form considered "more symmetric" in structure?
4. Why does elimination sometimes require a row exchange, and how is this represented using a permutation matrix?
5. Explain why `P⁻¹ = Pᵀ` for any permutation matrix `P`.
6. What does it mean, geometrically or algebraically, for `Ax = b` to have exactly one solution in terms of pivots?
7. Explain why the Gauss–Jordan method, applied to `[A : I]`, produces `A⁻¹` in the right-hand block.
8. Why is `AAᵀ` always symmetric, even when `A` itself is not square?
9. Explain the relationship `U = Lᵀ` that holds when a symmetric matrix `A` is factored as `A = LDU`.
10. Why can't a singular matrix have an inverse, in terms of the pivot count?

## MCQs (university-exam difficulty)
1. The elementary matrix corresponding to `R₃ ← R₃ − 4R₁` (3×3 case) has which entry equal to `−4`?
   (a) (1,3)  (b) (3,1)  (c) (3,3)  (d) (1,1)
2. If `A = LU`, then `L` is:
   (a) Upper triangular with pivots on diagonal
   (b) Lower triangular with 1's on diagonal
   (c) A permutation matrix
   (d) Always symmetric
3. `(ABC)⁻¹` equals:
   (a) `A⁻¹B⁻¹C⁻¹`  (b) `C⁻¹B⁻¹A⁻¹`  (c) `C⁻¹A⁻¹B⁻¹`  (d) `A⁻¹C⁻¹B⁻¹`
4. A matrix has an inverse if and only if elimination produces:
   (a) At least one pivot  (b) n pivots  (c) A row of zeros  (d) A symmetric U
5. For any permutation matrix `P`:
   (a) `P⁻¹ = P`  (b) `P⁻¹ = Pᵀ`  (c) `P` is never invertible  (d) `Pᵀ = −P`
6. If `A` is symmetric and invertible, then `A⁻¹` is:
   (a) Never symmetric  (b) Always symmetric  (c) Singular  (d) Equal to `A`
7. `(AB)ᵀ` equals:
   (a) `AᵀBᵀ`  (b) `BᵀAᵀ`  (c) `BA`  (d) `(BA)⁻¹`
8. The number of distinct 4×4 permutation matrices is:
   (a) 4  (b) 16  (c) 24  (d) 256
9. In Gauss–Jordan elimination, the process stops when the augmented matrix reaches the form:
   (a) `[U : C]`  (b) `[I : A⁻¹]`  (c) `[A : U]`  (d) `[L : U]`
10. If elimination on `A` requires a row swap, the correct factorization is written as:
    (a) `A = LU`  (b) `A = UL`  (c) `PA = LU`  (d) `A = PLU⁻¹`

## Numerical Problems
1. Construct the elementary matrix `E₃₁` for `R₃ ← R₃ − 5R₁` (order 3).
2. Given `A = [4 2; 8 7]`, find the elimination matrix that reduces it to upper triangular form, and state `U`.
3. Factor `A = [4 3; 6 3]` into `A = LU`.
4. Using your answer to Q3, also write `A = LDU`.
5. For `A = [0 1; 3 4]`, explain why a row exchange is needed, construct the permutation matrix `P`, and find `L, U` such that `PA = LU`.
6. Construct all `3! = 6` permutation matrices of order 3 (you may reference §3.3 to check your work, but write them out yourself first).
7. Find `A⁻¹` for `A = [1 2; 3 4]` using Gauss–Jordan elimination, showing every row operation.
8. Find `A⁻¹` for `A = [2 0 0; 0 3 0; 0 0 4]` using Gauss–Jordan (this should be quick — think about why).
9. If `A = [1 3; 2 4]`, compute `Aᵀ`, then verify `(A⁻¹)ᵀ = (Aᵀ)⁻¹` by computing both sides.
10. Determine whether `A = [4 2; 2 1]` is symmetric, and if it's invertible.

## Challenging Problems
1. Given `A = [1 −2 2; 2 −4 5; −2 5 −4]` (the Part 4 example), independently re-derive the permutation, `L`, `D`, and `U`, and verify `PA = LU` numerically by full matrix multiplication.
2. For `A = [6 −2 −4 4; 3 −3 −6 1; −12 8 21 −8; −6 0 −10 7]` (the Part 2 example), verify the claimed `L` and `U` by computing `LU` fully and confirming it equals `A`.
3. Find the values of `c` for which `A = [2 c c; c c c; 8 7 c]` is singular (work through the full elimination, don't just quote the answer).
4. If `A` is symmetric and `A = LDLᵀ`, show algebraically that `A⁻¹` is also symmetric, using the property `(A⁻¹)ᵀ = (Aᵀ)⁻¹`.
5. Given `A = [3 1 2; 2 −3 −1; 1 2 1]` (the Part 1/2 running example), compute `A⁻¹` fully via Gauss–Jordan (continuing from §6.3), and separately verify `A⁻¹ = U⁻¹L⁻¹` is consistent using the `L` and `U` already found in §2.4.

---

# ANSWER KEY + DETAILED SOLUTIONS

## MCQ Answers
1. **(b)** — the multiplier for `R₃ − 4R₁` sits at position (3,1).
2. **(b)** — by definition of `A = LU`.
3. **(b)** `C⁻¹B⁻¹A⁻¹` — reversal rule.
4. **(b)** n pivots.
5. **(b)** `P⁻¹ = Pᵀ`.
6. **(b)** Always symmetric.
7. **(b)** `BᵀAᵀ` — reversal rule.
8. **(c)** `4! = 24`.
9. **(b)** `[I : A⁻¹]` — Gauss–Jordan goes all the way to `I`.
10. **(c)** `PA = LU`.

## Numerical Problem Solutions

**1.** `E₃₁` for `R₃ ← R₃ − 5R₁` (order 3):
```
E₃₁ = [1  0  0]
      [0  1  0]
      [−5 0  1]
```

**2.** `A = [4 2; 8 7]`. Multiplier for R2: `8/4 = 2`. `R₂ − 2R₁` → `[4 2; 0 3] = U`.
```
E₂₁ = [1  0]
      [−2 1]
```

**3.** `A = [4 3; 6 3]`. Multiplier: `6/4 = 3/2`. `R₂ − (3/2)R₁` → `[4 3; 0 3−4.5] = [4 3; 0 −1.5]`.
```
L = [1    0]     U = [4    3]
    [3/2  1]         [0  −1.5]
```
Check: `L·U = [4 3; 6+0  4.5−1.5] = [4 3; 6 3]` ✓ (row 2: `3/2·4=6` ✓, `3/2·3 + 1·(−1.5) = 4.5−1.5=3` ✓).

**4.** `D = diag(4, −1.5)`. `U(new) = [1  3/4; 0  1]` (divide row 1 of U by 4, row 2 by −1.5). So `A = LDU` with those three matrices.

**5.** `A = [0 1; 3 4]`. Pivot (1,1) is `0`, and row 2 has a nonzero `3` below it — row exchange needed.
```
P = [0 1]     PA = [3 4]
    [1 0]          [0 1]
```
`PA` is already upper triangular, so `U = [3 4; 0 1]`, `L = I = [1 0; 0 1]` (no further elimination needed after the swap). Check: `LU = [3 4; 0 1] = PA` ✓.

**6.** The six order-3 permutation matrices are exactly as listed in §3.3: `I`, `P₂₁`, `P₃₁`, `P₃₂`, `P₂₁P₃₁`, `P₂₁P₃₂`.

**7.** `A = [1 2; 3 4]`.
`[A:I] = [1 2 : 1 0; 3 4 : 0 1]`
`R₂ − 3R₁` → `[1 2 : 1 0; 0 −2 : −3 1]`
`R₂ = R₂/(−2)` → `[1 2 : 1 0; 0 1 : 3/2 −1/2]`
`R₁ − 2R₂` → `[1 0 : 1−3 = −2, 0+1 = 1; 0 1 : 3/2  −1/2]`
```
A⁻¹ = [−2    1 ]
      [ 3/2 −1/2]
```
Check: `AA⁻¹ = [1·(−2)+2·1.5,  1·1+2·(−0.5); 3·(−2)+4·1.5, 3·1+4·(−0.5)] = [1, 0; 0, 1]` ✓.

**8.** `A = diag(2,3,4)` — a diagonal matrix is already both upper and lower triangular with no off-diagonal elimination needed. `A⁻¹ = diag(1/2, 1/3, 1/4)` directly (divide `I` rows by the corresponding pivot). This is quick because diagonal matrices invert entrywise — no elimination required at all beyond normalizing.

**9.** `A = [1 3; 2 4]`. `Aᵀ = [1 2; 3 4]`.
`A⁻¹`: `det(A) = 4−6 = −2`. `A⁻¹ = (1/−2)[4 −3; −2 1] = [−2  1.5; 1  −0.5]`.
`(A⁻¹)ᵀ = [−2  1; 1.5  −0.5]`.
`(Aᵀ)⁻¹`: `det(Aᵀ) = 4−6 = −2` also. `(Aᵀ)⁻¹ = (1/−2)[4 −2; −3 1] = [−2 1; 1.5 −0.5]`.
Both sides match: `[−2 1; 1.5 −0.5]` ✓.

**10.** `A = [4 2; 2 1]`. `Aᵀ = [4 2; 2 1] = A` → **symmetric**. Elimination: `R₂ − (1/2)R₁` → `[4 2; 0 0]` — only **one** pivot obtained (row 2 becomes entirely zero) → **singular, not invertible.**

## Challenging Problem Solutions

**1.** Already fully worked in §4.2 — `P = P₂₃`, `L = [1 0 0; −2 1 0; 2 0 1]`, `D = I`, `U = [1 −2 2; 0 1 0; 0 0 1]`. Full multiplication `LU`: row 1 `(1,0,0)·U = (1,−2,2)` ✓; row 2 `(−2,1,0)·U = (−2, 4+1, −4+0) = (−2, 5, −4)` ✓ matches `PA` row 2 `(−2,5,−4)`; row 3 `(2,0,1)·U = (2, −4+0, 4+1) = (2,−4,5)` ✓ matches `PA` row 3. Confirmed.

**2.** Already fully worked in §2.6 (second example) and verifiable by multiplying `L` and `U` from that section row by row — each row of `LU` reproduces the corresponding row of `A` exactly (e.g., row 3 of `L` is `(−2,−2,1,0)`; dotted with `U`'s columns gives `(−12, 16−8=8, 8−13+5=... )` — carry through the same style of check as shown in §2.4 for the smaller example).

**3.** `A = [2 c c; c c c; 8 7 c]`. Full elimination (matching §8.6-style singular problem):
`R₂ − (c/2)R₁`, `R₃ − 4R₁` → `[2 c c; 0  c−c²/2  c−c²/2; 0  7−4c  −3c]`
`R₃ − ((7−4c)/(2c−c²))·2R₂` → last pivot becomes `c−7`.
Matrix is singular whenever any pivot is zero: `c − c²/2 = 0 ⟹ c(1−c/2)=0 ⟹ c=0` or `c=2`; and `c−7=0 ⟹ c=7`. **So A is singular for c = 0, 2, 7.**

**4.** For symmetric `A` with `A = LDLᵀ`: `A⁻¹ = (LDLᵀ)⁻¹ = (Lᵀ)⁻¹D⁻¹L⁻¹ = (L⁻¹)ᵀD⁻¹L⁻¹` (using `(Lᵀ)⁻¹=(L⁻¹)ᵀ` from Part 7). Let `M = L⁻¹`. Then `A⁻¹ = MᵀD⁻¹M`. Taking the transpose: `(A⁻¹)ᵀ = Mᵀ(D⁻¹)ᵀ(Mᵀ)ᵀ = MᵀD⁻¹M` (since `D` is diagonal, `D⁻¹` is automatically symmetric, i.e. `(D⁻¹)ᵀ=D⁻¹`, and `(Mᵀ)ᵀ=M`). This is identical to `A⁻¹` itself — so `(A⁻¹)ᵀ = A⁻¹`, proving `A⁻¹` is symmetric.

**5.** Continuing §6.3's forward pass `[3 1 2:1 0 0; 0 −11/3 −7/3:−2/3 1 0; 0 0 −8/11:−7/11 5/11 1]`, normalize row 3 (`×(−11/8)`): `(0,0,1 : 7/8, −5/8, −11/8)`. Clear column 3 from rows 1–2 using this row, then clear column 2 from row 1 using the (normalized) row 2, then normalize rows 1–2 by their pivots. Carrying this through yields `A⁻¹`; as a consistency check, this must equal `U⁻¹L⁻¹` using the `U` and `L` already found in §2.4 — i.e. compute `U⁻¹` and `L⁻¹` separately (each is straightforward since both are triangular) and confirm the product matches your Gauss–Jordan result.

---

# FINAL REVISION SHEET

**Elementary matrix `E_ij`:** `I` modified by `R_i ← R_i − l_ij R_j`. One off-diagonal nonzero entry only.

**Master elimination identity:** `E_(last)⋯E_(first) A = U` (apply left to right in the order operations were performed; last operation = leftmost matrix).

**Triangular factorization `A = LU`:** `L` = lower triangular, 1's on diagonal, multipliers (no sign flip) below diagonal in the position used. `U` = upper triangular, pivots on diagonal.

**Symmetric factorization `A = LDU`:** `D` = diagonal matrix of pivots; both `L` and `U` now have 1's on diagonal (divide old `U`'s rows by their pivots to get new `U`).

**Solving `Ax=b` via `LU`:** `Lc=b` (forward sub.) → `Ux=c` (back sub.) → `x`.

**Permutation matrix `P`:** `I` with rows reordered. `P⁻¹ = Pᵀ` always. `n!` distinct `n×n` permutation matrices. `PA = LU` when a row exchange is required during elimination.

**Row exchanges & factors:** if elimination hits a zero pivot with a nonzero entry below, swap first (`P`), then eliminate; `L`, `U` are then read off from `PA`, not `A` directly.

**Inverse `A⁻¹`:** `AB=I=BA ⟹ B=A⁻¹`. Unique when it exists. `(ABCD)⁻¹=D⁻¹C⁻¹B⁻¹A⁻¹`. `A` invertible ⟺ elimination yields `n` pivots. `x=A⁻¹b` is the unique solution when `A` is invertible.

**Gauss–Jordan inverse algorithm:** `[A:I]` → (forward pass) `[U:C]` → (backward pass + normalize) `[I:A⁻¹]`. A zero row on the left at any point ⟹ no inverse.

**Transpose rules:** `(Aᵀ)ᵀ=A`; `(AB)ᵀ=BᵀAᵀ`; `(A⁻¹)ᵀ=(Aᵀ)⁻¹`; `(A±B)ᵀ=Aᵀ±Bᵀ`; transpose of upper triangular = lower triangular (and vice versa).

**Symmetric matrix condition:** `Aᵀ = A`. `AAᵀ` and `AᵀA` are always symmetric for any `A`. If `A` symmetric and `A=LDU`, then `U=Lᵀ`, i.e. `A=LDLᵀ`. Symmetric invertible `A` has symmetric `A⁻¹`.

**Singular / nonsingular condition:** singular = fewer than `n` pivots (no inverse); nonsingular = full `n` pivots (invertible, unique solution to `Ax=b`).

**Common mistakes to actively guard against:** reversing multiplication order in `(ABCD)⁻¹` or `(AB)ᵀ`; sign confusion between `E_ij` (negative of multiplier) and `L` (multiplier as-is); stopping Gauss–Jordan at `[U:C]` instead of continuing to `[I:A⁻¹]`; forgetting to pre-multiply by `P` before reading off `L,U` when a row swap occurred.

**Exam shortcuts:**
- To check any factorization (`LU`, `LDU`, `PA=LU`, `A⁻¹`), verify by re-multiplying at least one row — much faster than redoing the whole problem, and it catches most arithmetic slips.
- Diagonal matrices invert entrywise — no elimination needed (`§numerical Q8`).
- If you already found `A=LU`, you get `A⁻¹=U⁻¹L⁻¹` "for free" without redoing Gauss–Jordan from scratch — useful as a cross-check, per the Part 6 challenging problem.
- If `A` is symmetric, you only need to compute `L` and `D` — `U` is just `Lᵀ`, saving half the work.

---

# WHAT I SHOULD BE ABLE TO DO AFTER STUDYING THIS

- [ ] Construct any elementary matrix `E_ij` directly from a stated row operation, in either direction (subtraction or addition form), without sign errors.
- [ ] Write down the full elimination identity `E_(last)⋯E_(first)A = U` for a 3×3 or 4×4 matrix and correctly order the product.
- [ ] Derive `A = LU` from scratch for a given square matrix, reading `L`'s entries directly off the multipliers used (no separate matrix inversion needed).
- [ ] Convert an `A = LU` factorization into `A = LDU` by pulling out the diagonal pivots.
- [ ] Solve `Ax = b` using `Lc=b` then `Ux=c`, without ever multiplying `L` and `U` back together.
- [ ] Recognize when a row exchange is required during elimination, construct the correct permutation matrix `P`, and correctly write `PA = LU`.
- [ ] State and apply `P⁻¹ = Pᵀ`, and count the number of `n×n` permutation matrices as `n!`.
- [ ] Determine whether a given square matrix is invertible purely by counting pivots from elimination.
- [ ] Apply `(ABCD)⁻¹ = D⁻¹C⁻¹B⁻¹A⁻¹` correctly, including to expressions involving `L`, `U`, and `E` matrices.
- [ ] Perform a complete Gauss–Jordan inversion `[A:I] → [I:A⁻¹]` for a 3×3 matrix, showing every row operation explicitly, and recognize immediately if a zero row means no inverse exists.
- [ ] Compute the transpose of any matrix (including non-square ones) and correctly state/apply `(AB)ᵀ = BᵀAᵀ` and `(A⁻¹)ᵀ = (Aᵀ)⁻¹`.
- [ ] Test whether a matrix is symmetric, and explain why `AAᵀ` and `AᵀA` are always symmetric for any matrix `A`.
- [ ] Derive `A = LDLᵀ` for a symmetric matrix and verify `U = Lᵀ` numerically.
- [ ] Solve "for which values of a parameter is this matrix singular" problems by tracking pivots symbolically through elimination.
- [ ] Look at any unfamiliar numerical problem from this unit and immediately identify which of the above procedures applies.
