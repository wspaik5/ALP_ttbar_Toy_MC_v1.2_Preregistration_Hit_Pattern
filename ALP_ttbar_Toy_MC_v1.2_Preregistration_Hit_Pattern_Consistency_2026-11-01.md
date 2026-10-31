# ALP-ttbar Toy MC v1.2 — Preregistration: Hit-Pattern Consistency, Resolution Anchoring and Lifetime Scan

**Author:** Won Shik Paik (independent, personal project)
**Date frozen:** 2026-11-01 (this file is committed to GitHub before any v1.2 analysis code is run)
**Builds on:** v1.1, https://doi.org/10.5281/zenodo.23833475 (companion to arXiv:2610.38932)
**Status:** plan only. No v1.2 result exists at the time of this commit.

---

## 0. Why preregister a toy study

v1.1 showed that the 17 MeV result depends mainly on assumptions (resolution core and tails) and that the
material-veto width was optimised on the same samples. To avoid tuning v1.2 choices after seeing the outcome,
the question, model, parameters, metrics and predictions below are fixed in advance. Any deviation will be
listed explicitly in the v1.2 report under "Deviations from preregistration".

## 1. Questions

- **Q1 (primary).** Does a hit-pattern consistency requirement reduce the sensitivity of the photon-conversion
  veto to non-Gaussian vertex-resolution tails at 17–50 MeV?
- **Q2 (anchoring).** Can public CMS information fix which of the four v1.1 resolution scenarios is realistic?
- **Q3 (secondary).** Do the v1.1 conclusions hold at cτ = 0.1 mm and 10 mm, not only at cτ = 1 mm?

## 2. Step 1 — Resolution anchoring (done before freezing; outcome recorded here)

A search of accessible public sources (CMS photon performance paper arXiv:1502.02702; CMS tracker
commissioning arXiv:1007.1988; PoS proceedings on tracker material) found:

- CMS conversion vertices are fitted with a constraint that the two tracks are **parallel** at the vertex.
  This matches the v1.1 "bending/tangent" term, so the model structure is supported.
- Conversion vertices image the beam pipe and pixel layers in data, so the pixel-region core resolution is at
  the mm level or better for the conversion population as a whole.
- **No public quantitative radial resolution or tail fraction** was found for high-pT (> 20 GeV), nearly
  collinear pairs, which is the regime relevant here. The CMS material-budget PAS (TRK-10-003) could not be
  retrieved during this check.

**Consequence (frozen):** v1.2 does not tune the resolution to a single value. All four v1.1 scenarios are kept
and reported side by side. Q2 is answered "not determinable from accessible public sources" unless the
TRK-10-003 note can be read before the v1.2 analysis starts; if it is read, its numbers are only quoted for
comparison and do not change the frozen parameters below.

## 3. Step 2 — Hit-pattern model (frozen)

- Barrel sensor layers: the 14 radii in `mag.LAYERS` (29, 68, 109, 160, 255, 339, 418, 498, 608, 692, 780,
  868, 965, 1080 mm). Non-sensor material (beam pipe 22 mm, supports 185 and 550 mm) produces no hits.
- **First hit:** for each track, the first sensor layer with radius greater than the *true* vertex radius.
  Per-layer hit efficiency ε = 0.99 (independent per track and layer, fixed seed); a missed hit moves the first
  hit outward. The pair's first-hit layer is the inner of the two tracks' first-hit layers.
- **Allowed interval:** with R_first the first-hit radius and R_prev the previous sensor layer (0 if none), a
  pair is hit-pattern consistent if
  `R_prev − t·σ_r < r_meas < R_first + t·σ_r`, where σ_r is the per-pair (or fixed) resolution reported by the fit.
- **Primary tolerance:** t = 1. Sensitivity variants: t = 0 and t = 2 (reported, not used to choose).
- Applied identically to ALP signal and conversions, after the paper-like baseline cuts.

## 4. Step 3 — Analysis (frozen)

- Masses: 17, 50, 100, 200 MeV. Primary cτ = 1 mm; secondary cτ = 0.1 and 10 mm (Q3).
- Resolution scenarios: the four v1.1 scenarios (fixed / per-pair × Gaussian / 10% tails at 3×).
- Selection: v1.1 `scan_v11` (mass window and veto width n_σ ∈ {none, 0.5, 1, 1.5, 2, 3, 4, 5} optimised
  jointly), run **with and without** the hit-pattern requirement. No new tunable parameters are added.
- Seeds and sample sizes as in v1.1 (conversions seed 5, n = 1.5×10^6; signal seed 11, n = 3×10^5).
  The hit-efficiency draw uses a new fixed seed 23.
- **Primary metric:** limit degradation relative to the no-conversion case (v1.1 definition).

## 5. Predictions (written before running)

- **P1.** In the per-pair-with-tails scenario at 17 MeV (v1.1: 64×), the hit-pattern requirement gives a
  **partial** improvement: degradation between 13× and 43× (reduction factor 1.5–5).
  Reason: hit patterns only bound the vertex to an inter-layer gap (30–110 mm wide), so tails that keep a
  conversion inside its gap are not caught; conversions on supports (185, 550 mm) are not constrained at all.
- **P2.** In the two Gaussian scenarios, the change is small: within a factor 1.5 of the v1.1 values at every
  mass.
- **P3.** At 100–200 MeV the change is within a factor 1.5 in all scenarios.
- **P4 (Q3).** 17 MeV remains the most affected mass at cτ = 0.1 and 10 mm in every scenario.

**Falsification:** P1 fails if the 17 MeV tails value is < 13× (hit patterns help more than expected) or
> 43× (they barely help). Every prediction is reported as confirmed or refuted; refuted predictions are not
re-framed after the fact.

## 6. Reporting plan

- v1.2 note (same format as v1.1) with a new Figure 5 and table: four scenarios × with/without hit pattern ×
  four masses, plus the cτ scan; v1.0/v1.1 numbers reproduced unchanged by default settings.
- Sections: "Preregistered predictions vs. outcome", "Deviations from preregistration", "Limitations",
  and a clearly labelled **"Future directions"** section (below).

## 7. Future directions (to be written out in the v1.2 report)

Beyond v1.2 the toy approach reaches its limit. The report will state explicitly that the following require
detector-level simulation or experiment-internal information and are not attempted here:

1. Realistic conversion-vertex resolution and tails for high-pT, nearly collinear pairs (full tracking,
   Gaussian-sum-filter electron fits, bremsstrahlung).
2. Track-finding and reconstruction efficiency for displaced, collimated electrons (the paper identifies this as
   the dominant effect on its sensitivity).
3. Endcap region and the Phase-2 high-granularity calorimeter mentioned by the paper's authors.
4. The full material map, including services and support structures, and nuclear-interaction backgrounds.
5. Other backgrounds treated in the paper (e.g. heavy-flavour decays) and pile-up.

## 8. Limitations known in advance

Toy kinematics and detector numbers; barrel only; hit model ignores sensor geometry, dead modules, overlap
regions and pixel-cluster merging of collimated tracks; tail model illustrative; selection optimised on the same
samples (same as v1.1). Only trends and ratios are meaningful.
