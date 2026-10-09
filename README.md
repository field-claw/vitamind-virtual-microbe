# vitamind-virtual-microbe — VitaMind 虚拟微生物系列

Minimal, license-clean GitHub pilot slices of the VitaMind virtual-microbe
project. Each pilot ships one third-party or curated model plus a
zero-dependency Python wrapper and a fail-closed `verify.py` (clone-and-run,
**all hard checks PASS**).

## Pilots

| # | Organism | Model | Growth (if comparable) | Repo | Quality |
|---|---|---|---|---|---|
| 1 | *Aspergillus niger* | iJB1325 / ATCC 1015 (BiGG) | 0.9399 h⁻¹ | `field-claw/ijb1325-aniger-pilot` | published full GEM |
| 2 | *S. cerevisiae* | ecYeastGEM (BiGG, GECKO) | 0.087974 h⁻¹ | `field-claw/ecYeastGEM-yeast-pilot` | published full GEM |
| 3 | *A. niger* | aniger_ccm (curated, in-house) | 18.95 (model units, **not** h⁻¹) | `field-claw/aniger-ccm-pilot` | curated CCM · real BIOMASS · carbon guardrail 100% · phosphate-switch citrate phenotype |

> **Correction on pilot 1.** It was originally published as
> `ijb1325-ecoli-pilot`, labelled *E. coli*. Verifying the model's own embedded
> annotations (*A. niger* x606 vs *Escherichia* x4, the latter only inside
> literature titles about expressing *A. niger* enzymes in *E. coli*) plus the
> ATCC 1015 strain name proved **iJB1325 is the *A. niger* ATCC 1015
> genome-scale model**, so the repo was renamed to `ijb1325-aniger-pilot`. Every
> measured number was unchanged — only the label was wrong.
>
> The lesson is now enforced in code: each pilot's `verify.py` carries an
> **organism assertion that is independent of every numeric check**, precisely
> because all numeric checks were green while the identity was wrong.

> **iMA871 superseded.** The originally-planned `iMA871-aniger-pilot`
> (BioModels) is abandoned: it ships `gene=0`, an artificial biomass sink, and a
> carbon guardrail that is **NOT** verifiable, so it cannot serve as a
> trustworthy reference. It has been **superseded** by `aniger-ccm-pilot` — a
> curated in-house CCM model with a *real* BIOMASS reaction (SBO:0000629, real
> precursors + GAM), a *verifiable* carbon guardrail (closing 5 organic-carbon
> exchanges collapses growth to 0, drop 100%), and the native phosphate-switch
> citrate phenotype (growth 18.95 → 0 while citrate secretion capacity doubles
> 6.00 → 12.00).

## Reproduce any pilot

```bash
git clone <pilot-repo>
cd <pilot-repo>
pip install -r requirements.txt
python verify.py   # exit 0 = all checks passed
```

## Honesty discipline

Every pilot discloses its model-quality limits openly: license status,
artificial vs real biomass, carbon-guardrail verifiability, organism identity,
and units. Numbers in different units (h⁻¹ vs model units) are **never**
cross-compared. See each pilot's `NOTICE.md` / `README.md`.

Two rules learned the hard way, now enforced in code:

1. **An identity assertion must never ride on the numeric checks.** A wrong
   species label shipped once with every metric green. Each `verify.py` now
   asserts the organism and the growth unit separately from the measurements.
2. **Overflow products are invisible under the "natural" objective.** Reading
   citrate secretion off a biomass-maximising FBA solution returns exactly 0 in
   every condition — a false negative that made a real phenotype look absent.
   Overflow must be solved as its own objective with a growth floor.

## Not yet published

A genuine *E. coli* pilot is **not** released. The only local *E. coli* model
file (`eciML1515`) fails SBML validation under COBRApy, and the previously
published "E. coli" pilot turned out to be *A. niger*. No *E. coli* repo will
be published until a model is independently species-verified and its
`verify.py` organism assertion passes.

## License

Per-pilot: see each repository's `LICENSE` / `NOTICE.md`
(`field-claw/ijb1325-aniger-pilot` and `field-claw/ecYeastGEM-yeast-pilot` carry
the BiGG non-profit license for the model; `field-claw/aniger-ccm-pilot` is MIT,
model + code).
