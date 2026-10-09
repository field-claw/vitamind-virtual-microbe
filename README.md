# vitamind-virtual-microbe — VitaMind 虚拟微生物系列

Minimal, license-clean GitHub pilot slices of the VitaMind virtual-microbe
project. Each pilot ships one third-party or curated genome-scale model plus a
zero-dependency Python wrapper and a fail-closed `verify.py` (clone-and-run,
**5/5 checks**).

## Pilots

| # | Organism | Model | Growth (if comparable) | Repo | Quality |
|---|---|---|---|---|---|
| 1 | *E. coli* | iJB1325 (BiGG) | 0.9399 h⁻¹ | `field-claw/ijb1325-ecoli-pilot` | published full GEM |
| 2 | *S. cerevisiae* | ecYeastGEM (BiGG, GECKO) | 0.087974 h⁻¹ | `field-claw/ecYeastGEM-yeast-pilot` | published full GEM |
| 3 | *A. niger* | aniger_ccm_refined (curated, in-house) | 18.95 (model units, **not** h⁻¹) | `field-claw/aniger-ccm-refined-pilot` | curated CCM · real BIOMASS · carbon guardrail 100% |

> **iMA871 paused / superseded.** The originally-planned `iMA871-aniger-pilot`
> (BioModels) was paused: it ships `gene=0`, an artificial biomass sink, and a
> carbon guardrail that is **NOT** verifiable, so it cannot serve as a
> trustworthy reference. It has been **superseded** by
> `aniger-ccm-refined-pilot` — a curated in-house CCM model with a *real*
> BIOMASS reaction (SBO:0000629, real precursors + GAM) and a *verifiable*
> carbon guardrail (closing 5 organic-carbon exchanges collapses growth to 0,
> drop ~100%). The empty `iMA871-aniger-pilot` repository is being removed.

## Reproduce any pilot

```bash
git clone <pilot-repo>
cd <pilot-repo>
pip install -r requirements.txt
python verify.py   # exit 0 = all checks passed
```

## Honesty discipline

Every pilot discloses its model-quality limits openly: license status,
artificial vs real biomass, carbon-guardrail verifiability, and units. Numbers
in different units (h⁻¹ vs model units) are **never** cross-compared. See each
pilot's `NOTICE.md` / `README.md`.

## License

Per-pilot: see each repository's `LICENSE` / `NOTICE.md`
(`field-claw/ijb1325-ecoli-pilot` and `field-claw/ecYeastGEM-yeast-pilot` carry
the BiGG non-profit license for the model; `aniger-ccm-refined-pilot` is MIT,
model + code).
