# Transcriptional evidence for autophagy induction by 15-PGDH inhibition in aged muscle

A targeted reanalysis of GSE149924 (Palla *et al.*, *Science* 2021,
[doi:10.1126/science.abc8059](https://doi.org/10.1126/science.abc8059)): mouse quadriceps from aged
animals treated for one month with the 15-PGDH inhibitor SW033291, with aged and young vehicle controls,
three animals per arm.

## Background

Palla *et al.* report that inhibiting 15-PGDH raises PGE2 in aged muscle and restores mass, strength and
mitochondrial function, and that autophagy increases alongside these changes. The claim is stated as an
increase in autophagy in the abstract, the section heading and the discussion; flux is asserted once, in
the results text, and rests on a p62 immunoblot under colchicine (Fig. S13C). The accompanying RNA-seq is used in that paper mainly to establish
the mitochondrial and ubiquitin-ligase arms of the phenotype. Whether the autophagy component is
reflected in the transcriptome has not been examined directly, and it bears on mechanism: a
transcriptionally driven increase in autophagy implies a different upstream biology, and a different set
of follow-up experiments, than one mediated entirely by post-translational control.

This reanalysis tests that question at the level of gene sets, using both the deposited differential
expression statistics and an independent re-quantification of the drug contrast from raw reads.

## Results

No autophagy-associated gene set responds to the drug. The FOXO-dependent arm, the core autophagosome
machinery, the CLEAR network and TFEB family, cathepsins, v-ATPase subunits and mitophagy receptors are
all indistinguishable from the transcriptome-wide background; the FOXO-dependent set sits at the 50th
percentile of the ranked distribution, which is the definition of no shift. Effect sizes across these
sets fall between −0.03 and +0.03 log2, and no set survives correction. The result is unchanged when the
analysis is repeated on counts generated here from the raw FASTQs rather than on the authors' pipeline
output, and unchanged again after regressing out a per-sample myeloid score, which excludes a shift in
immune-cell content as the explanation.

![Group-mean expression](figures/fig13_group_mean_heatmap.png)

The same six libraries show a large, coherent increase in respiratory-chain subunits — complexes I
through V, 96 genes moving together. This reproduces the mitochondrial finding reported in the original
study and serves as an internal positive control: the comparison resolves a coordinated shift of roughly
a quarter of a log2 unit across a gene set of this size, separating the two arms by twenty times the
difference seen in any autophagy block. The autophagy nulls are therefore measurements rather than
failures of detection.

A second internal control was less informative. The atrogene set, chosen because the original study
reports these genes falling under treatment, shows a weak downward trend that proves on inspection to
rest on *Traf6* and *Mstn* — a signalling adapter and a secreted growth inhibitor — while *Fbxo32* and
*Trim63*, the canonical muscle E3 ligases and the two most precisely measured genes in the set, do not
move. The discrepancy is explained by assay rather than biology: the atrogene decrease in the original
work is RT-qPCR on separate animals, so it neither contradicts nor validates the sequencing contrast.

## Interpretation

The absence of a transcriptional signature is not evidence against increased autophagy, however it is measured. Autophagy
is regulated predominantly downstream of transcription — through ULK1 and mTORC1 phosphorylation, LC3
lipidation, and assembly and trafficking of pre-existing protein — and flux can rise several-fold
without new transcription of autophagy genes. The finding therefore constrains mechanism rather than
outcome: if PGE2 signalling enhances autophagy in aged muscle, it acts on protein already present.

That interpretation is consistent with the other transcriptional observations here. Neither the TFEB
family nor its CLEAR target network responds, so lysosomal biogenesis is not being transcriptionally
induced. The cAMP–CREB axis through which PGE2 is thought to act shows no transcriptional activation
either, with *Ppargc1a* flat despite a reported qPCR increase. Of the pathways examined, only the
respiratory chain and a denervation-associated programme move, the latter as part of a broader return of
age-elevated genes toward their young-animal levels.

The possibility that the p62 immunoblot reflects altered p62 synthesis rather than altered flux was
examined and is not supported: *Sqstm1* transcript is unchanged within a tight interval, and NRF2 target
genes, which would indicate an oxidative-stress route to p62 induction, are flat. The proteasome
subunits lean upward rather than downward, so reduced non-autophagic p62 clearance does not account for
the blot either. These checks narrow the space of alternative explanations for the published flux
result.

Resolving the mechanism requires protein-level and kinetic measurements — LC3-II and p62 with and
without lysosomal blockade, ideally alongside phospho-ULK1 or phospho-mTOR — rather than deeper
sequencing of the same material.

## Limitations

The drug contrast comprises three animals per arm, and the conclusion is restricted to coordinated
set-level responses of the magnitude the respiratory-chain control demonstrates to be detectable.
Smaller effects, and effects confined to individual genes, cannot be excluded.

The aged-versus-young comparison is available only as deposited, filtered to genes passing the authors'
significance threshold, so questions about how the autophagy programme behaves with age are answered
weakly here. Within that constraint, none of the nine core autophagy effectors appears among the
age-responsive genes, while nine of ten denervation genes do.

The smallest sets examined — three TFEB-family genes, five fission genes — carry little statistical
weight individually and are reported as such. Sequencing depth is also uneven: the two arms of the drug
contrast were sequenced roughly four-fold less deeply than the aging arm, which limits inference for
low-abundance transcripts, immune markers in particular.

One earlier conclusion of this reanalysis has been **retracted**. An apparent coordinated decline in
autophagy *regulatory* genes, significant on the deposited statistics, weakened substantially on
re-quantification and no longer survives multiple-testing correction. It is documented in the report
rather than removed.

## Notes on the deposited data

The supplementary TPM matrix in GSE149924 carries gene identifiers that do not correspond to its
expression values: myoglobin registers as near-absent from skeletal muscle, and rank correlation between
that matrix and the count-based abundances from the same submission is approximately zero. No row offset
recovers alignment. The two deposited DESeq2 tables are internally consistent and agree closely with the
re-quantification performed here, so the problem appears confined to that one file.

## Repository contents

| | |
|---|---|
| `docs/REPORT.md` | full analysis with all statistics, tests and per-item verdicts |
| `notebooks/01_reanalysis.ipynb` | code; Part 1 deposited statistics, Part 2 re-quantification |
| `figures/`, `results/tables/` | 15 figures and the tables behind them |
| `data/` | deposited inputs and gene-set definitions |

## Methods summary

Gene sets were defined specifically for the hypotheses under test rather than taken from GO, because the
GO autophagy category combines FOXO-dependent, TFEB-dependent and constitutive genes whose separation is
the object of the analysis; GO-derived sets are labelled where used. Inference is at set level, since at
three animals per group single-gene results are unstable. Every set-level claim was tested against a
background matched on expression level, and multiple testing accounted across all sets examined. The
drug contrast was re-quantified with salmon against Ensembl GRCm39 and analysed with DESeq2; fold changes
agree with the deposited values at a rank correlation of 0.78. Full detail is in `docs/REPORT.md`, and
`environment.yml` reproduces the software environment.

## Citation

`CITATION.cff`. The original study should be cited alongside any use of this reanalysis.
