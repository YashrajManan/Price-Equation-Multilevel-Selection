# The Price Equation and the Limits of Group Selection

Under what combination of group size, cost/benefit ratio, and migration rate can group-level selection favor
an individually costly, group-beneficial trait — decomposed exactly via George Price's 1972 covariance
identity, not asserted qualitatively.

## 1. Aim

Convert "group selection can work" from a disputed, qualitative claim into a precise, quantified result: an
exact hierarchical Price-equation decomposition confirmed against real simulated data, an independently
derived Hamilton's-rule-equivalent threshold, and a genuine (not assumed) search for how migration rate
shapes the outcome.

## 2. Objective

1. Build a subdivided trait-group population model with a binary altruism trait, individual cost `c`, and
   group-shared benefit `b`.
2. Compute the exact hierarchical Price decomposition of the population's real change in mean trait
   frequency into within-group and between-group components, and confirm it exactly reproduces the real
   simulated outcome.
3. Derive, from scratch, the expected covariance terms under random group formation, simplify to a
   closed-form threshold, and cross-check it numerically.
4. Run the full multi-generation simulation across a parameter grid to map where altruism increases vs.
   decreases.
5. Search directly for how migration rate affects group selection's long-run effectiveness, without
   assuming the shape in advance.

## 3. Data fetch

None required: the real deliverable here is an exact algebraic decomposition (the Price equation's
within-group/between-group split) confirmed against simulated data with a known cost, benefit, and group
structure — the kind of controlled ground truth no real dataset of group-selection outcomes could supply,
since the true individual cost and group benefit driving a real trait's spread are rarely directly
measurable.

## 4. Data describe

Not applicable in the usual sense — the "data" is the model's own parameter space: group size `N`, the
individual cost `c` and group-shared benefit `b` of the altruistic trait, and migration rate `m`, explored
via direct simulation and an independent analytical derivation.

## 5. Methods / Workflow

1. Implement within-group fitness functions and one full generation's lifecycle (within-group selection →
   fitness-weighted between-group pooling → migration blending → finite-population binomial regrouping).
2. Implement the exact hierarchical Price decomposition and confirm it reproduces the real simulated
   post-selection population mean for a single generation — an exact numerical-identity check.
3. Derive, from scratch, the expected within- and between-group covariance terms under random group
   formation, simplifying to the closed-form threshold `b/c > N`.
4. Cross-check the derived threshold against a direct Monte Carlo estimate across a range of `b/c` ratios.
5. Run the full multi-generation simulation across a `(N, b/c)` parameter grid.
6. Map which parameter combinations favor altruism and confirm the boundary tracks the derived threshold.
7. Sweep migration rate `m` and search for its real effect on group selection's long-run outcome.
8. Export all results as CSV; plot the hero figures.

## 6. Results

**Exact-match checkpoint:** the hierarchical Price decomposition reproduces the real simulated
post-selection population mean exactly (`difference = 0.00e+00`), not approximately — `p0=0.458000`,
Price-predicted `p1=0.484026`, real simulated `p_pool=0.484026`.

**Hamilton threshold cross-check:** the analytically-derived threshold (`Total_cov ∝ b - c*N`) matches the
empirical Monte Carlo estimate across the full swept range, crossing zero at exactly `b/c=N=10`, with the
expected sign-matching noise immediately around the boundary itself.

**Multi-generation consequence:** below threshold (`b/c=5`), altruist frequency went extinct
(`p: 0.290 → 0.000` over 300 generations); above threshold (`b/c=15`), it increased (`p: 0.290 → 0.590`).

**Parameter sweep:** the altruism-favored region across `(N, b/c)` tracks the `b/c=N` diagonal, with a real
finite-sampling staircase texture near the boundary.

**Migration-rate search:** a trough shape, not a simple monotonic relationship or single interior peak —
altruism was most favored at very low migration (`m=0.01 → final p=0.095`) and very high migration
(`m=1.0 → final p=0.822`), and suppressed to near-zero in the middle range (`m=0.1–0.3 → final p≈0.000`).

![Price threshold cross-check](results/hero_price_threshold.png)
![Migration rate effect](results/migration_rate_effect.png)

## 7. Biology interpretation of results

Altruists are always at a strict local disadvantage within any group (`w_alt<w_non` whenever `c>0`), yet a
group's mean fitness increases with altruist frequency whenever `b>c` — the Price decomposition makes this
tension exact rather than qualitative, splitting the population's real change in trait frequency into a
negative within-group term and a (sometimes larger) positive between-group term. The confirmed `b/c>N`
threshold converts "group selection can overcome individual selection" into a specific, falsifiable number
for this model. The migration-rate trough is the most biologically interesting finding: migration both
propagates a successful group's advantage (needed for between-group selection to have any effect at all) and
erodes the between-group variance that selection acts on — these two opposing roles don't cancel into a
smooth single optimum, they produce a double-favored, middle-suppressed outcome instead.

## 8. Learning through project

A deliberately re-derived threshold (not cited from the literature) is a genuinely stronger, more defensible
result than a quoted formula — the from-scratch derivation here surfaced a real implementation detail
(the Price equation's between-group covariance term requires the population, not sample, convention) that a
literature citation alone would never have caught. The migration-rate search is a concrete lesson in
resisting the pull toward whichever hypothesis sounds more interesting before the evidence is in: an interior
optimum is the more "textbook-shaped" guess, but the real simulation showed the opposite structure, favoring
both extremes instead.

## Caveats

This result holds for one specific, minimal model: a binary altruism trait, linear individual-cost/
group-benefit fitness functions, and migration implemented as frequency-blending with finite-population
binomial regrouping noise. Real populations can have continuous cooperation levels, nonlinear group benefits,
and non-random (kin- or spatially-structured) group formation — all of which would change the exact threshold
and could change the migration-rate relationship's shape — named explicitly as real, disclosed scope
boundaries, not covered by this build's specific numbers.
