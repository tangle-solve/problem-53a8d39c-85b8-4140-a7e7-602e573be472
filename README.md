<!-- solve-problem:53a8d39c-85b8-4140-a7e7-602e573be472 -->
# Strongly polynomial linear programming

A [Tangle Solve problem](https://solve.tangle.tools/problems/53a8d39c-85b8-4140-a7e7-602e573be472).

## What is the problem?

Given a linear program in standard form — maximize/minimize cᵀx subject to Ax ≤ b (or Ax = b, x ≥ 0), where A ∈ ℚ^{m×n}, b ∈ ℚ^m, c ∈ ℚ^n — find an exact optimal solution (or certify infeasibility/unboundedness) using a number of arithmetic operations on rationals that is polynomial in m and n **only**, independent of the encoding lengths L(A, b, c) of the input numbers. This is the strongly polynomial complexity notion of Mead (or Blum–Shub–Smale-style unit-cost arithmetic over ℚ combined with polynomially bounded bit lengths of intermediate numbers): an algorithm is strongly polynomial if (a) it performs poly(m, n) arithmetic operations, comparisons, and rounding-to-integer steps, and (b) the intermediate numbers have bit length polynomially bounded in the input encoding length.

The subtlety: "controlling intermediate bit lengths" means the algorithm cannot cheat by hiding exponential blowup in the sizes of rationals it manipulates. Interior-point methods achieve poly(L) iterations but depend on L; simplex with pivoting rules depends on bit lengths of the pivots. The census notes duplicate "same-as" links (COMB-000209) — treat this as a single problem.

A caveat on the exact formulation: some statements restrict to LPs with integer data, or demand the running time depend only on the "combinatorial" part of A. Smale's 1998 problem list poses it for general real/rational data.

## Why does it matter?

It is Problem 9 of Smale's "Mathematical Problems for the Next Century" (1998) and arguably the central open question in LP complexity. A solution would make LP complexity invariant under numerical rescaling of rows/columns of A, matching the practical behavior of pivoting methods, and would resolve whether LP is in "strongly polynomial time" in the sense relevant to combinatorial optimization, game theory, and the simplex method's exponential-pivot mystery. Many algorithms using LP as a subroutine (e.g., submodular function minimization, flow algorithms) would inherit cleaner bounds.

## What's already known?

- **Weakly polynomial algorithms exist**: Khachiyan's ellipsoid method (1979) and Karmarkar's interior-point method (1984) run in time poly(m, n, L) — polynomial in encoding length, but iteration counts depend on L.
- **Tardos (1986)**: strongly polynomial algorithms when A has small integer entries — running time poly(m, n, Δ) where Δ bounds entries of A, independent of b and c. This covers most combinatorial LPs. (Related: Vavasis–Ye layered-step methods; Kitamura–Takabe classes.)
- **Special cases**: fixed dimension n (Megiddo's linear-time algorithm, n fixed); network flow and transportation problems have strongly polynomial algorithms (Tardos, Orlin, Goldberg–Tarjan); Tardos's framework reduces many combinatorial problems to LPs with bounded-Δ matrices.
- **Lower bounds**: essentially nothing nontrivial is known; no super-polynomial arithmetic-operation lower bound is known for any LP model, and even the (weaker) Hirsch conjecture / diameter questions relevant to simplex pivoting remain unresolved (Santos's counterexample shows the Hirsch conjecture is false).
- Interior-point with a fixed number of iterations remains open; recent work on "short step" IPs and on the condition-number parameterization (Dunagan–Spielman on smoothed analysis of LP) gives partial evidence but not strongly polynomial bounds.

## What would count as solving it?

1. **Upper bound (the main problem)**: an algorithm that, given rational A, b, c with encoding length L, decides feasibility/optimality and outputs an exact optimal vertex solution using poly(m, n) arithmetic operations, with all intermediate rationals of bit length poly(L). Proof of correctness and complexity in a standard refereed venue.
2. **Lower bound variant**: a proof that no such algorithm exists (e.g., a family of LPs forcing Ω(f(L)) arithmetic operations), likely via an algebraic computation model.
3. **Intermediate targets a referee would value**: strongly polynomial algorithms for broader input classes than Tardos's (e.g., dependence only on the "mixed" or layered condition measures); or poly(m,n) iterations for a primal–dual method with exact termination.

## How to start

1. Read Tardos (1986) carefully — her rounding-in-the-dual technique (solving rounded versions of the LP so that optimal bases become robust to perturbation in b and c) is the only general machinery producing strong polynomiality; understand exactly where it needs entries of A bounded.
2. Read Smale (1998) for the precise problem statement and Schrijver's textbook (Ch. 10–15) for the complexity framework of strongly vs. weakly polynomial algorithms.
3. Natural approaches: (a) find an interior-point variant whose iteration count depends on a condition measure invariant under scaling and bounded by poly(m,n) combinatorially; (b) extend Tardos-style rounding to general A via factorizations/parameter searches over A alone; (c) study LP formulations where optimal bases are determined by sign patterns of bounded-precision roundings.
4. Main obstacle: no one knows how to decouple the geometry of the polytope from the bit lengths of its defining inequalities; the known methods all localize precision requirements to either dimension or entry size.

## Key references

- L. G. Khachiyan, "A polynomial algorithm in linear programming" (Doklady Akademii Nauk SSSR, 1979). First (weakly) polynomial LP algorithm via the ellipsoid method. (No stable link; widely reprinted.)
- N. Karmarkar, "A new polynomial-time algorithm for linear programming," STOC 1984 / Combinatorica 4 (1984), 373–395. Interior-point method with improved polynomial dependence on L. https://doi.org/10.1007/BF02579150
- É. Tardos, "A strongly polynomial algorithm to solve combinatoratorial linear programs," Operations Research 34 (1986), 250–256. Strongly polynomial algorithm when A's entries are bounded integers, independent of b and c. https://doi.org/10.1287/opre.34.2.250
- S. Smale, "Mathematical problems for the next century," Mathematical Intelligencer 20 (1998), 7–15. Poses strongly polynomial LP as Problem 9 of the century list. https://doi.org/10.1007/BF03024498
- A. Schrijver, *Theory of Linear and Integer Programming*, Wiley, 1986. Standard reference for LP complexity, the ellipsoid method, and strongly polynomial notions.
- R. M. Freund and J. R. Vera, "On the complexity of estimating condition measures for linear conic systems" and related conditioning literature — relevant to condition-based complexity (verify exact relevance).

## Warnings

- "Strongly polynomial" has two inequivalent definitions in circulation: (i) unit-cost arithmetic count polynomial in dimension with polynomially bounded bit lengths (the standard, and the one here), vs. (ii) polynomial time even when numbers grow. Don't conflate.
- The bit-length condition in the census is essential: an algorithm doing poly(m,n) exact rational operations with exponentially growing operands would not count.
- Known dead ends: proving Hirsch-type diameter bounds as a route (Santos disproved the Hirsch conjecture in 2010); claiming fixed-dimension results (Megiddo) as progress on the general problem.
- Tardos's algorithm is sometimes misread as solving the whole problem — it requires A to have bounded integer entries (or running time depending on that bound).
- No meaningful lower bounds exist; any "solution" claiming impossibility would need a rigorous arithmetic model.

---
*`ALGO-000162` · unverified*

## Acceptance guidance

1. **Upper bound (the main problem)**: an algorithm that, given rational A, b, c with encoding length L, decides feasibility/optimality and outputs an exact optimal vertex solution using poly(m, n) arithmetic operations, with all intermediate rationals of bit length poly(L). Proof of correctness and complexity in a standard refereed venue.
2. **Lower bound variant**: a proof that no such algorithm exists (e.g., a family of LPs forcing Ω(f(L)) arithmetic operations), likely via an algebraic computation model.
3. **Intermediate targets a referee would value**: strongly polynomial algorithms for broader input classes than Tardos's (e.g., dependence only on the "mixed" or layered condition measures); or poly(m,n) iterations for a primal–dual method with exact termination.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/53a8d39c-85b8-4140-a7e7-602e573be472?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000162.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000162)
