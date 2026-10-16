# Draft — correspondence to the Blau lab re: GSE149924

Two separate matters, deliberately ordered: a concrete deposit issue that is easy to act on, then an
analytical observation that is open to discussion. Tone is collegial and the second point is framed as a
question rather than a correction, because the underlying claim is supported by assays this reanalysis
cannot evaluate.

---

**Subject:** GSE149924 — a problem with the deposited TPM matrix, and a question about the autophagy
transcriptional signature

Dear Dr Palla / Prof. Blau,

I have been re-analysing GSE149924 from your 2021 *Science* paper on 15-PGDH inhibition in aged muscle,
and I wanted to raise two things — one that I think is a straightforward deposit problem, and one that is
a genuine question about interpretation.

**1. The deposited TPM matrix appears to have mismatched row labels.**

In `GSE149924_tpm.txt.gz`, the gene-symbol column does not appear to correspond to the expression values.
Myoglobin reads 1 TPM in quadriceps, *Mylpf* 19 and *Actn3* 3, while the same genes in your deposited
DESeq2 output have baseMean values of 6,338, 34,647 and 88,960. Across the 16,677 shared symbols, mean
TPM and deposited baseMean correlate at Spearman -0.017, and I could not recover alignment with any row
offset between -3000 and +3000. The highest-TPM rows are *Sgta*, *Glb1l3* and *Cypt9*, which carry 0–412
mean counts in your own count-based table.

Your two DESeq2 tables look entirely sound — when I re-quantified the raw FASTQs from PRJNA630522 with
salmon, my fold changes agreed with your deposited drug contrast at Spearman 0.78 — so this looks
specific to the TPM file, most likely a sort or merge step applied to one column. It would be worth
replacing, since anyone reusing the series at gene level would get noise.

**2. A question about the autophagy signature in the RNA-seq.**

Working from both your deposited statistics and my own re-quantification of the six batch-2 libraries, I
cannot detect a transcriptional autophagy signature in the SW033291 contrast. Testing at set level rather
than gene by gene — appropriate at n=3 — the core machinery, CLEAR/TFEB targets, FOXO-dependent autophagy
genes, cathepsins, v-ATPase subunits and mitophagy receptors are all flat, with mean Wald statistics
between -0.26 and +0.08 and no set surviving correction. This holds after regressing out a per-sample
myeloid score, so it is not a cell-composition effect.

The reason I think this is informative rather than a null from an underpowered comparison is that the
same libraries show the respiratory-chain programme clearly: 79–96 ETC genes with a mean Wald of +1.6 and
a median increase of 0.25 log2, which reproduces your Fig. 7A. So the comparison does resolve coordinated
low-amplitude shifts; the autophagy sets simply do not show one.

I am not questioning the flux result in Fig. S13C — a colchicine p62 experiment measures something the
transcriptome cannot, and I checked the obvious transcriptional confounds and found none: *Sqstm1* mRNA
is unchanged with a tight interval (-0.002, 95% CI -0.24 to +0.24) and NRF2 targets are flat, so p62
synthesis does not appear to be elevated. My question is whether you would interpret the combination as
indicating that the autophagy effect is post-translational — PGE2 acting through existing protein rather
than through transcription. That reading seems consistent with everything in the deposit, and it would be
useful to know whether it matches what you have seen at the protein level.

One observation that may be relevant to both points: the batch-2 libraries that make up the drug contrast
carry 5.1–7.7 M read pairs, against 10.5–27.9 M for the batch-1 libraries. Low-abundance genes such as
*Cd68* and *Ptprc* sit near the detection floor in that arm, which limits what can be asked of it.

I would be glad to share the full reanalysis — code, gene sets and per-gene tables — if that is useful,
and equally glad to be told where I have misread the design.

With thanks for making the data available,
[name]

---

## Notes before sending

- **Check first:** whether the TPM file has already been corrected or flagged, and whether a later
  corrigendum or follow-up paper addresses the autophagy mechanism. This reanalysis predates no such check.
- **Deliberately omitted:** the atrogene and Pgc1α discordances. Both are RT-qPCR results from separate
  animals (Figs. 6D, 6E, 7B), so a bulk RNA-seq null does not contradict them, and raising them would read
  as a challenge to experiments this analysis cannot speak to.
- **Also omitted:** the cAMP-biosynthesis GO term observation. It depends on which genes DAVID used and on
  thresholds not stated in the methods, and I could not retrieve the figure images to confirm the term is
  in Fig. 6A. Worth holding until that is verified.
- **Already retracted from my own analysis:** an earlier "regulation of autophagy is coordinately down"
  finding, which did not survive re-quantification. It is not in this draft and should not be raised.
