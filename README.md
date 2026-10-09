# vitamind-virtual-microbe — VitaMind 虚拟微生物系列

Minimal, license-clean GitHub pilot slices of the VitaMind virtual-microbe
project. Each pilot ships one third-party or curated model plus a
zero-dependency Python wrapper and a fail-closed `verify.py` (clone-and-run,
**all hard checks PASS**).

## Pilots

| # | Organism | Model | Growth (if comparable) | Repo | Quality |
|---|---|---|---|---|---|
| 1 | *Aspergillus niger* | iJB1325 / ATCC 1015 (BiGG) | 0.9399 h⁻¹ | `field-claw/aniger-pilot` → `models/genome_scale/iJB1325/` | published full GEM |
| 2 | *S. cerevisiae* | ecYeastGEM (BiGG, GECKO) | 0.087974 h⁻¹ | `field-claw/ecYeastGEM-yeast-pilot` | published full GEM |
| 3 | *A. niger* | aniger_ccm (curated, in-house) | 18.95 (model units, **not** h⁻¹) | `field-claw/aniger-pilot` → `models/curated_ccm/` | curated CCM · real BIOMASS · carbon guardrail 100% · phosphate-switch citrate phenotype |

Entries 1 and 3 are the **same organism at two scales** and now live in one
repo, `field-claw/aniger-pilot`, linked by an explicit annotation crosswalk
rather than merged into one SBML file (measured: 0 id collisions in all three
layers, but 13 of the CCM's 28 reactions have no genome-scale counterpart, so
merging would fabricate reactions that exist in neither model). One
`verify.py` there covers both models and the crosswalk — 12 hard checks.

> **Correction on pilot 1.** It was originally published as
> `ijb1325-ecoli-pilot`, labelled *E. coli*. Verifying the model's own embedded
> annotations (*A. niger* x606 vs *Escherichia* x4, the latter only inside
> literature titles about expressing *A. niger* enzymes in *E. coli*) plus the
> ATCC 1015 strain name proved **iJB1325 is the *A. niger* ATCC 1015
> genome-scale model**. Every measured number was unchanged — only the label
> was wrong.
>
> The lesson is now enforced in code: each pilot's `verify.py` carries an
> **organism assertion that is independent of every numeric check**, precisely
> because all numeric checks were green while the identity was wrong.

### Archived (superseded)

- `field-claw/ijb1325-aniger-archive` — standalone iJB1325 repo, now part of
  `aniger-pilot`
- `field-claw/aniger-ccm-pilot-archive` — standalone CCM repo, same

> **iMA871 abandoned.** The originally-planned `iMA871-aniger-pilot`
> (BioModels) is not usable: it ships `gene=0`, an artificial biomass sink, and
> a carbon guardrail that is **NOT** verifiable. The curated CCM in
> `aniger-pilot` supersedes it — a real BIOMASS reaction (SBO:0000629, real
> precursors + GAM), a verifiable carbon guardrail (closing 5 organic-carbon
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
(`field-claw/aniger-pilot` ships MIT for its code and curated CCM while iJB1325
remains under the BiGG non-profit licence; `field-claw/ecYeastGEM-yeast-pilot`
carries the BiGG non-profit licence for the model).
