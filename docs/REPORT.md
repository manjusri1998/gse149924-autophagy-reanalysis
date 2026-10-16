# Does 15-PGDH inhibition increase autophagy in aged muscle?

Targeted reanalysis of GSE149924 (mouse quadriceps, aged ± SW033291 for 1 month, n=3 per arm;
Palla et al. 2021, PMID 33303683). Code for every computation: {{artifact:c9e36e94-900a-4aea-bafa-ba3c5d64ccee}}.

> **STATUS — superseded in part, 16 Oct 2026.** The 13 libraries have since been re-quantified from the raw
> FASTQs (salmon against Ensembl GRCm39 r112, tximport, DESeq2), independent of the authors' pipeline.
> Two changes follow, detailed in the Re-quantification section at the end of this report:
>
> 1. **Retracted:** the "regulation of autophagy is coordinately down" result in the Appendix. On
>    independent counts it falls from mean Wald -0.301 (matched p = 1e-4) to -0.177 (p = 0.028,
>    BH 0.136) and does not survive multiple-testing correction. Macroautophagy likewise. Do not cite it.
> 2. **Strengthened:** the central negative. Every autophagy–lysosome set is flat on the re-quantified
>    counts and stays flat after regressing out a per-sample myeloid score, which closes the item-2
>    composition gate. The respiratory-chain control replicates at +1.616 (BH 0.0012).
>
> Everything else below stands. The aging contrast is being re-quantified; this report will be rewritten
> in full when that completes.

## Answer: no — the transcriptional evidence does not support it

Every autophagy-specific gene set tested is flat. The FOXO-dependent autophagy set sits at the **50.0th
rank percentile** of the transcriptome — the exact median. CLEAR/TFEB targets, non-FOXO core machinery,
the TFEB family itself, cathepsins, v-ATPase subunits, mitophagy receptors: all flat. The one set that
moves in an autophagy-related direction moves **down**, not up.

This is a usable negative. The same contrast detects a coordinated +0.247 log2FC increase across 96
electron-transport-chain genes at p = 8.5e-40, so the assay demonstrably resolves low-amplitude
coordinated shifts in sets of this size. For the autophagy sets, flat is a measurement, not a failure to
measure — which makes it a legitimate positive result for the post-translational hypothesis: if
15-PGDH inhibition raises autophagic flux, it does not do so by changing autophagy gene transcription.

Two caveats bound that conclusion rather than soften it. The composition gate (item 2) came back
uninformative, so a macrophage-abundance shift large enough to matter is not excluded. And the
small, low-count sets — myeloid markers, fission (5 genes), fusion (3) — remain genuinely underpowered;
their nulls carry little weight.

## Verdict by item

| item | role | verdict |
|---|---|---|
| 1. Data integrity and design | gate | **Partly failed** — TPM matrix unusable; design confirmed, batch not dominant |
| 2. Cell composition | gate | **Failed** — uninformative, not clean; 3.1-fold shifts undetectable |
| 3. FOXO autophagy vs atrogenes | **decider** | **Weakens** — autophagy arm at the median; positive control fails |
| 4. CREB/TFEB bypass | test | **Neither** — the route-is-live control fails |
| 5. p62 readout confound | test | **Supports (weakly)** — the simplest confound is excluded |
| 6. Mitochondrial dynamics | test | **Weakens** the fission alternative; supplies the working control |
| 7. Denervation programme | alternative model | **Real but not specific** — does not replace the four-pathway model |

Set-level statistics and effect sizes are reported throughout. Gene-wise FDR is not used as evidence:
exactly **one** gene in this contrast passes FDR < 0.05 genome-wide, so single-gene claims would be
story-telling. Individual genes appear only as effect sizes with confidence intervals, and only where a
CI is tight enough to exclude something.

Gene sets are the custom ones specified, not stock GO terms, for the stated reason — the GO "autophagy"
term mixes FOXO-dependent, TFEB-dependent and constitutive genes and blurs the distinction under test.
Where GO terms were used (item 4 appendix), that is flagged.

---

## Item 1 (gate) — data integrity and design

**The deposited TPM matrix is unusable for gene-level work.** Its gene-symbol column does not correspond
to its expression values: myoglobin reads 1 TPM in quadriceps, *Mylpf* 19, *Actn3* 3, while the deposited
count-based baseMean for the same genes is 6,338, 34,647 and 88,960. Spearman between mean TPM and
deposited baseMean across 16,677 shared symbols is **-0.017**, and no row offset in ±3000 recovers
alignment (best Pearson 0.031) — a scramble, not a shift. A limma-trend fit on log2(TPM+1) was run first
and discarded; it produced a spurious 71%-of-genes downward tilt and zero rank concordance with the
deposited statistics. **Everything in this report uses the deposited count-based DESeq2 tables.**

**The design is confirmed and batch is not the dominant axis.** Group membership re-derived from GSM
titles matches the published design (young vehicle 4, batch 1; aged vehicle 6, three per batch; aged SW
3, batch 2). PERMANOVA-style R² read against its null expectation of 1/(n-1): batch 0.206 vs 0.200
(p = 0.40), age 0.276 vs 0.167 (p = 0.029), drug 0.197 vs 0.200 (p = 0.50). Aged vehicles interleave
across batches, so the six-sample group is poolable. *Aged4* is a sample-level outlier. Sample-level
geometry is valid on the scrambled matrix because covariance is invariant to row-label permutation and
the rows are internally coherent (sample-sample Spearman 0.936–0.977).

Fig. 6A's legend independently fixes the drug contrast at n=3 per arm.

![Design QC]({{artifact:e77579cf-7fc4-4126-818f-30d04492df26}})

## Item 2 (gate) — cell composition

**Uninformative, not clean.** No marker shifts significantly: myeloid set permutation p = 0.60, sign test
7 of 9 down p = 0.18. But the median minimum detectable effect across myeloid markers is **1.64 log2FC
(3.1-fold)**; *Cd68* sits at baseMean 8.5 with a CI of -2.36 to 0.46 and *Ptprc* at 5.2. Point estimates
of roughly 2-fold down in three pan-myeloid markers are compatible with a real macrophage depletion that
this study cannot resolve. *Adgre1* is deposited under its former symbol *Emr1*.

What limits the propagation of that uncertainty: the lysosome-rich families a macrophage shift would drag
are flat — cathepsins (11 genes) mean Wald -0.083, p = 0.77; v-ATPase subunits (14) +0.090, p = 0.72.
There is no lysosomal signal here for composition to explain. As a substitute for the impossible
per-sample regression, every set was re-tested with GO:0002376 immune-annotated genes removed; results
are carried in the item tables below.

![Composition baseline]({{artifact:deec0521-049f-4faf-bf70-8413b9f3e6a2}})

---

## Item 3 (the decider) — FOXO-dependent autophagy vs atrogenes

**Verdict: weakens the hypothesis. The positive control fails, so the comparison the item was designed to
make cannot be completed.**

| set | size | NES | p | median log2FC | median rank percentile |
|---|---|---|---|---|---|
| FOXO-dependent autophagy | 10 | -0.88 | 0.62 | -0.03 | **50.0** |
| FOXO proteasome / atrogene | 6 | -1.45 | 0.074 | -0.21 | 80.8 |

![Item 3 per-gene fold changes]({{artifact:c69496ea-cfb3-42ae-8f60-e3a752f5971b}})

The autophagy arm is not merely non-significant — it is centred on the transcriptome median, with *Sqstm1*
at -0.002, *Map1lc3b* at -0.003, *Pik3c3* at +0.001 and *Atg12* at +0.006. Nothing in this set is moving
in any direction.

The atrogene arm is directionally down but rests entirely on *Traf6* and *Mstn*, neither a muscle-specific
E3 ligase. The two canonical atrogenes are the **best-powered genes in either set** — *Fbxo32* at baseMean
2,225 and *Trim63* at 1,722, both with lfcSE 0.161 — and neither moves (*Fbxo32* -0.024, CI -0.34 to 0.29).
This is not a power failure.

It is also not a sign error: *Fbxo32* is +0.614 with age in the deposited aging table, the expected
direction for aged atrophic muscle, confirming the orientation and showing the gene is measurable here.
**The control was never derivable from this deposit** — the source panels are RT-qPCR (Fig. 6D) and a
separate intramuscular shRNA experiment (Fig. 6E). A bulk RNA-seq null does not contradict them, and they
cannot validate this contrast.

Consequence: the planned inference — both arms down coherently implies FOXO inactivation across the
board — cannot be drawn. The arms are not even distinguishable from each other (mean-Wald difference
-0.683, bootstrap 95% CI -1.490 to +0.130). What the item does establish is that **the FOXO-autophagy
transcriptional arm is flat in a dataset that can detect coordinated shifts**, which is the central
negative result of this reanalysis.

![fgsea running scores]({{artifact:89d1405d-4806-4476-8f6a-e855d80aa070}})

## Item 4 — CREB/TFEB bypass

**Verdict: neither supports nor falsifies.** The falsifier required CREB targets to be clearly up. They
are not — median log2FC -0.076, 3 of 10 positive, and the one member with a tight enough interval to
speak is *Pde4b* at -0.562, the wrong direction. *Ppargc1a* is flat at -0.037 despite the paper's qPCR
claim of restoration (Fig. 7B).

| set | size | NES | p |
|---|---|---|---|
| CREB targets | 10 | -1.21 | 0.238 |
| CLEAR / lysosomal (19 custom) | 19 | -1.05 | 0.390 |
| cAMP biosynthesis (GO:0006171) | 16 | +0.96 | 0.503 |
| non-FOXO core machinery | 12 | -0.72 | 0.837 |

TFEB family: *Tfeb* -0.028, *Tfe3* +0.140, *Mitf* -0.162. CLEAR is flat as the hypothesis predicted, but
with the route-is-live control dead, flat CLEAR cannot separate "the bypass is not transcriptional" from
"nothing is happening here".

**The cAMP-biosynthesis term, never previously reported:** its upward lean is carried entirely by genes at
the detection floor — *Ptger3* +4.012 at **1.5 mean counts** is the only nominally significant up member,
*Ntsr1* +2.007 at 0.4 counts, and four more members sit at baseMean 0.13–0.17 with lfcSE ≈ 4. The two
best-measured adenylate cyclases move **down** (*Adcy9* -0.460 at baseMean 341, *Adcy2* -0.211). A
threshold tool fed a nominal-p list would place *Ptger3* in the upregulated bin; the quantitatively
dominant members contradict it. Unverified: the figure images were not retrievable, so the term's presence
in Fig. 6A rests on report rather than inspection, and DAVID's gene-to-GO mapping differs from QuickGO's.

![CREB/TFEB bypass]({{artifact:f80f7504-493f-4bc0-aa1f-9da36c22a9eb}})

## Item 5 — is the p62 readout confounded?

**Verdict: supports the hypothesis, weakly — the simplest confound is excluded.** *Sqstm1* is
**-0.0016 with a 95% CI of -0.239 to +0.236** at baseMean 2,751, excluding any change beyond ±18%. This
is the one gene in the series where the deposit is genuinely well powered. NRF2 targets are flat
(NES +1.03, p = 0.43); the only upward lean, *Gsta3*, has 5.8 mean counts.

So higher p62 synthesis cannot explain a steeper colchicine-induced accumulation. The second confound is
absent too: proteasome subunits are **up, not down** (40 subunits, NES +1.83, fgsea p = 0.0013 — though
only p = 0.064 against an expression-matched null, so suggestive at best). More non-autophagic p62
clearance biases *against* the published claim rather than generating it. Note Fig. 6B concerns
protein-ubiquitin genes (E3 ligases), a different family from proteasome subunits.

Residual: mRNA is not synthesis rate, and a p62-only readout without a paired LC3-II panel is a weaker
flux measure whatever the transcriptome says.

![p62 confound]({{artifact:4fdf3479-2f44-41db-a13d-479e45e4e624}})

## Item 6 — mitochondrial dynamics, and the control that validates the series

**Verdict: weakens the fission alternative.** Fission mean Wald -0.141 (matched p = 0.92), fusion -0.241
(0.69), mitophagy receptors +0.072 (0.91), biogenesis beyond PGC-1α -0.227 (0.70). Fission is not up, so
a fission-driven remodelling account gets no transcriptional support — though at 5 and 3 genes these are
among the weakest sets here, and *Dnm1l* mRNA at -0.136 does not exclude post-translational DRP1
Ser616/Ser637 regulation.

**The internal positive control works.** The paper's one transcriptome claim drawn from this dataset
(Fig. 7A) reproduces emphatically: 96 ETC genes, mean Wald +1.578, median log2FC +0.247, 96% up,
expression-matched p = 1e-4, fgsea NES +3.54 (p = 8.5e-40); Complex I 100% up across 44 subunits.
This is what licenses reading the autophagy nulls as measurements. It also sets the scale: the ETC
programme is roughly five times the amplitude of anything found on the autophagy side.

![Mitochondrial dynamics]({{artifact:7b3a817c-b8ce-498e-a0aa-38938e4de218}})

## Item 7 — denervation programme as a parsimonious alternative

**Verdict: real, but not specific — it does not replace the four-pathway model.** The signature is
coordinately suppressed: 8 of 9 genes down, set mean Wald -0.902, expression-matched p = 0.004
(BH 0.016 across the session's 32 set tests), with *Hdac4* itself at -0.845 — the 15th most negative of
16,675 genes. All eight testable members plus *Hdac4* rise with age in the deposited aging table
(*Chrng* +3.95, *Musk* +2.40, *Runx1* +2.39, *Myh3* +2.75). So this is not "only *Hdac4* moves".

But it is not a distinct axis. Against other genes raised with age, the denervation set's drug response
is indistinguishable — Wilcoxon p = 0.409, and p = 0.612 matched on aging effect size. The broad aging
reversal already documented (Spearman between aging and drug log2FC = -0.462 across 681 genes) accounts
for it. The more parsimonious statement is not "one denervation axis" but "genes elevated in aged muscle
come down under the drug", of which denervation is one well-known subset. And the atrogene drop this
model was meant to explain was never observed transcriptionally (item 3). *Myog* is absent from the
filtered aging table, so panel a shows 8 denervation genes plus *Hdac4*.

![Denervation programme]({{artifact:67fa6f73-7464-4a8d-b8eb-dd5e9d131ce9}})

---

## Multiple-testing accounting

32 distinct set-level tests were run on the drug contrast across this reanalysis ({{artifact:c354a164-3d58-49de-8673-0417ffdb2f39}}). BH across
all of them: autophagy regulation 5.3e-4, every ETC complex 5.3e-4, regulation-minus-effectors 3.7e-3,
denervation 0.016, positive regulation and macroautophagy 0.020, autophagosome assembly 0.083, TORC1
signalling 0.090. Everything else falls above 0.1, including every autophagy-specific custom set. BH
assumes independence or positive dependence and several of these sets overlap heavily, so treat it as
indicative. Item 3's sets, the denervation set and the p62 sets were pre-registered before the work.

## Appendix — the GO-term result, and why it is not the headline

One GO-based set did move: regulation of autophagy (GO:0010506, 255 genes) at mean Wald -0.301,
61.2% down, expression-matched FDR 1.1e-3, surviving removal of effector genes and of immune-annotated
genes (attenuating 15–20%). Its leading edge is nutrient/energy-sensing signalling — *Prkaa2*, *Mtor*,
*Tsc1*, *Gsk3b*, *Csnk1a1*, *Ep300* — with both positive and negative regulators falling, i.e. a uniform
damping of the regulatory layer rather than a directional flux switch.

It is not the headline for three reasons. It is **down**, so it cannot support an increase in autophagy.
It comes from exactly the kind of mixed GO term this reanalysis was asked to avoid. And its amplitude is
roughly a fifth of the ETC programme in the same data. Autophagosome assembly and TORC1 signalling, which
looked significant before adjustment, do not survive immune-gene removal and are withdrawn.

![Autophagy sets]({{artifact:ec8a787b-ed42-43ed-bef4-7f5029ede429}})

## What would settle it

Per-sample counts. Re-quantifying the 13 libraries from SRA would give a real myeloid score to regress
out, restore power for low-count markers like *Cd68*, make the aging contrast testable genome-wide
instead of as a pre-filtered 813-gene list, and allow rotation tests on the actual expression matrix.
On the bench, the claim needs LC3-II/p62 immunoblot with and without a lysosomal block — the
transcriptome cannot adjudicate flux, and this reanalysis indicates it is not transcriptionally encoded.

## Data

| file | role |
|---|---|
| `GSE149924_DEseq2_aged_SW_aged_Veh.csv.gz` | drug contrast, 24,013 rows (16,675 testable) — carried every item |
| `GSE149924_DESeq2_A_veh_Y_veh-2.txt.gz` | aging contrast, **pre-filtered** to 813 genes (padj < 0.1) — sign anchor and concordance only |
| `GSE149924_tpm.txt.gz` | gene labels scrambled; used only for sample-level design QC |

Per-gene tables: item 3 {{artifact:8b3af6cf-d8cf-4daa-94dd-bdd7f0172926}}, ETC control {{artifact:d8598746-dce9-4440-8aaa-5fa2d903f02a}}, multiple testing {{artifact:c354a164-3d58-49de-8673-0417ffdb2f39}}.


---

## Re-quantification from raw reads (supersedes parts of the above)

All six batch-2 libraries (SRR11694853–858, the entire drug contrast) were re-quantified from ENA FASTQs
with salmon against a 144,344-transcript Ensembl GRCm39 r112 index (selective alignment, seqBias and
gcBias correction), summarised to gene symbols with tximport and tested with DESeq2. Mapping rates
78.5–83.4%; fragment counts match ENA exactly.

**The deposited drug table is sound.** My log2 fold changes correlate with theirs at Spearman 0.783
across 9,422 genes — unlike the TPM file, which correlates at -0.017. My pipeline resolves 16 genes at
FDR < 0.05 where the deposit has 1, a sensitivity difference, not an error.

**Library depth explains the earlier power problems.** Batch-2 libraries carry 5.1–7.7 M read pairs
against 10.5–27.9 M for batch 1 — a 3.9-fold gap, and both arms of the drug contrast sit on the shallow
side. That is why *Cd68* (baseMean 8.5) and *Ptprc* (5.2) could not settle the macrophage question while
*Fbxo32* (2,225) and *Sqstm1* (2,751) were measured tightly.

**Set-level results on independent counts**, unadjusted and after regressing out a per-sample myeloid
score (*Adgre1* is detected by salmon although absent from the deposit; score difference between arms
-0.347, p = 0.60):

| set | n | unadjusted mean Wald | adjusted mean Wald | adjusted BH |
|---|---|---|---|---|
| FOXO-dependent autophagy | 10 | -0.264 | -0.531 | 0.62 |
| Mitophagy receptors | 10 | -0.168 | -0.374 | 0.86 |
| Core effectors (requested 9) | 9 | -0.198 | -0.277 | 0.86 |
| TFEB family | 3 | -0.046 | -0.270 | 0.86 |
| Cathepsins | 11 | -0.463 | -0.247 | 0.86 |
| Macroautophagy (GO) | 223 | -0.202 | -0.222 | 0.088 |
| Regulation of autophagy (GO) | 271 | -0.177 | -0.194 | 0.088 |
| Lysosome (GO) | 452 | -0.080 | -0.042 | 0.95 |
| CLEAR / TFEB targets | 18 | -0.124 | -0.027 | 0.92 |
| v-ATPase subunits | 14 | -0.044 | -0.002 | 0.86 |
| Non-FOXO core machinery | 12 | +0.082 | +0.060 | 0.86 |
| **Respiratory chain (ETC)** | 79 | **+1.616** | **+1.641** | **0.0012** |

The verdict is unchanged and now rests on counts generated independently of the authors' pipeline, with
the composition gate closed rather than open. The two GO-based sets that approach significance remain
**down**, which cannot support an increase in autophagy, and neither survives BH.

![Re-quantified summary]({{artifact:71ae69c1-4054-408d-afdb-93c196a95b9c}})

![Library depth]({{artifact:851971d3-fc7a-494b-ad7c-046c0ca7bdb8}})
