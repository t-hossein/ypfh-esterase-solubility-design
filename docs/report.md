# Improving solubility and reducing aggregation of YpfH esterase by rational protein design

Reyhane Esmaili, Diba Hamzavi, Mohadese Rasouli, Hossein Teimouri

Department of Biotechnology, University of Tehran

---

## Abstract

YpfH is a 232-residue esterase of *Escherichia coli* K-12 with a Ser-His-Asp
catalytic triad and a solvent-exposed, poorly ordered C-terminal tail that
carries the protein's highest predicted aggregation propensity. We set out to
lower that aggregation propensity by single point substitutions, subject to the
constraint that catalysis must not be disturbed. Starting from an AlphaFold3
model of the wild type and an Aggrescan3D profile, we enumerated 37 candidate
substitutions at exposed aggregation-prone positions, excluded every position
conserved across a 31-sequence homolog alignment, and scored each candidate on
aggregation propensity, two independent estimates of folding stability, two
CamSol solubility scores, solvent exposure, and surface electrostatics. Three
designs survived the full filter: **L218R, V228E and Y207E**. Each improves
both CamSol scores relative to wild type, each lowers the Aggrescan3D average
score, and each lies between 22 Å and 33 Å from the nearest catalytic residue.
The work is computational; no design has been expressed or assayed.

## 1. Introduction

Recombinant expression of bacterial esterases is frequently limited less by
catalytic competence than by solubility. Aggregation during or after expression
reduces usable yield and can be addressed by protein engineering, provided the
substitutions introduced do not compromise the active site.

YpfH (RefSeq NP_416968.2, UniProt P76561) is annotated as displaying esterase
activity toward palmitoyl-CoA and pNP-butyrate. Like other members of the
lipase/esterase family it uses a charge relay system in which a serine acts as
the nucleophile that attacks the ester bond, a histidine acts as the general
base that activates that serine, and an acidic residue stabilizes the
histidine. UniProt assigns this role to Ser111, Asp159 and His191.

The engineering problem is therefore well posed: find substitutions that reduce
predicted aggregation and raise predicted solubility, at positions that are
neither catalytic, nor conserved, nor structurally load-bearing.

## 2. Methods

### 2.1 Homolog collection and conservation

The YpfH sequence was used as a BLASTp query against SwissProt and against
ClusteredNR. The working set retained for alignment comprises 31 sequences: the
query plus 30 homologs, five of which come from the SwissProt search. Sequence
identity to the query across the set runs from roughly 25 % to 60 %, which is
wide enough to distinguish genuinely conserved positions from positions that
are merely conserved within the Enterobacteriaceae.

A multiple sequence alignment was built with Clustal Omega. Ser111, Asp159 and
His191 are conserved in all 31 sequences, independently confirming the UniProt
active-site annotation. A further thirteen positions are conserved across the
set: 18L, 23H, 24G, 65W, 95Q, 105T, 109G, 110F, 112Q, 113G, 136G, 155H and
156G. All sixteen positions were excluded from mutagenesis.

### 2.2 Structure prediction and validation

The tertiary structure of the wild type was predicted with AlphaFold3 (five
seed models, ten recycles). The top-ranked model has pTM 0.92 and a mean
per-residue pLDDT of 91.7, with no steric clashes; the five seeds agree closely
with one another.

Model quality was assessed with MolProbity. The model scores 1.28 overall
(99th percentile for comparable structures), with a clashscore of 5.21 (93rd
percentile), zero Ramachandran outliers, 98.26 % of residues in favored
Ramachandran regions, zero poor rotamers, zero Cβ deviations and no bad bonds.
Three bad angles (0.12 %) and three CaBLAM outliers (1.3 %) are the only
flagged features.

The catalytic triad was validated geometrically in PyMOL. In the model, the
Ser111 OG atom lies 2.79 Å from His191 NE2, and the Asp159 OD2 atom lies 2.62 Å
from His191 ND1. Both distances fall inside the 2.5 Å to 3.5 Å hydrogen-bonding
window expected for a functional charge relay, so the model reproduces an
intact active site and is a reasonable basis for design.

### 2.3 Aggregation profiling and the target region

Aggregation propensity was profiled with Aggrescan3D, which scores each residue
in its structural context rather than from sequence alone and therefore
distinguishes buried hydrophobic residues, which are harmless, from exposed
ones, which are not. The wild type has an average A3D score of -0.5513, with a
per-residue minimum of -3.534 and a maximum of +2.2961, and a total of
-127.9095. More negative values indicate higher predicted solubility.

Two observations directed the design effort to the C terminus. First, the
C-terminal tail is the least ordered region of the model: per-residue pLDDT
falls below 70 at residue 221 and below 50 from residue 225 onward, against a
mean of 94.8 over residues 1 to 199. Second, that same region combines high A3D
scores with full solvent exposure, which is precisely the combination that
drives intermolecular association. Inspection of the model confirms that the
tail projects away from the body of the protein rather than packing against it.

Candidate positions were therefore drawn preferentially from the tail: 25 of
the 37 candidates finally scored lie at position 200 or later. Candidates were
selected on three criteria in combination: A3D score, AlphaFold confidence, and
secondary structure assignment, with positions in well-formed secondary
structure elements deprioritized to avoid perturbing the fold.

### 2.4 Scoring

Each candidate substitution was evaluated on six quantities:

| Quantity | Tool | Interpretation |
|---|---|---|
| A3D average score | Aggrescan3D | more negative is more soluble |
| ΔΔG | FoldX, via Aggrescan3D | negative is stabilizing |
| Intrinsic solubility | CamSol | higher is more soluble |
| Structurally corrected solubility | CamSol | higher is more soluble |
| ΔΔG | DynaMut | positive is stabilizing |
| SASA and exposure | PyMOL `get_area` | exposure = SASA / max ASA |

The two ΔΔG columns use opposite sign conventions. This is a property of the
tools, not of the data, and it is the single easiest thing to misread in the
tables that follow.

Surface electrostatics were computed with APBS after solvent removal and
hydrogen addition in PyMOL, and rendered as potential maps on the molecular
surface for the wild type and for each surviving mutant.

### 2.5 Filtering

Filtering proceeded in stages:

1. 37 candidates ranked by A3D average score; the top 16 retained.
2. ΔCamSol intrinsic score required to be greater than zero.
3. ΔCamSol structurally corrected score required to be greater than zero. This
   removed I175N, I175Q, I209Q and I209N, leaving 12.
4. DynaMut ΔΔG and FoldX ΔΔG examined jointly, to avoid accepting a
   substitution that buys solubility at the cost of the fold.
5. I229D discarded after its APBS surface potential map showed an unfavourable
   redistribution of surface charge relative to wild type, leaving 11.
6. Three designs selected on overall performance across the retained metrics.

## 3. Results

### 3.1 Selected designs

| Design | ΔA3D | FoldX ΔΔG | ΔCamSol intrinsic | ΔCamSol struct. corr. | DynaMut ΔΔG | Nearest triad residue |
|---|---|---|---|---|---|---|
| L218R | -0.0370 | -0.6365 | +0.1566 | +9.49 | -0.279 | 32.9 Å (Ser111) |
| V228E | -0.0305 | -0.3700 | +0.1214 | +5.74 | -0.082 | 29.7 Å (Ser111) |
| Y207E | -0.0247 | +0.4400 | +0.0389 | +4.24 | +0.422 | 21.9 Å (Ser111) |

All three substitutions replace a hydrophobic or aromatic side chain with a
charged one at an exposed position in the C-terminal region, which is the
canonical way to break up an aggregation-prone surface patch.

**L218R** is the strongest result. It carries the largest improvement in both
CamSol scores of any candidate, by a wide margin in the structurally corrected
score (+9.49 against +6.60 for the next best), combined with a stabilizing
FoldX ΔΔG. Its CamSol and Aggrescan profiles show the improvement localized to
the 210 to 232 window, exactly the region targeted, with the rest of the
protein unchanged.

**V228E** improves both CamSol scores and is stabilizing by FoldX. Position 228
sits in the most disordered part of the tail, where pLDDT is below 50, so the
substitution is made in a region with no defined structure to disturb.

**Y207E** has the weakest solubility gain of the three but is the only design
that is stabilizing by DynaMut, and the loss of an exposed aromatic side chain
at 207 is chemically the clearest single intervention against surface
aggregation.

### 3.2 Non-selected candidates of interest

V228R carries the largest A3D improvement in the whole set (-0.0533) and the
most stabilizing FoldX ΔΔG (-0.9943), but a smaller CamSol structurally
corrected gain than L218R or V228E. V30K also performs well (+6.60 structurally
corrected, -0.4689 FoldX). Neither was carried forward. Both are reasonable
starting points if the three primary designs fail experimentally.

### 3.3 Active-site integrity

The minimum heavy-atom distance from each design to the nearest catalytic
residue was measured on the wild-type model. L218R, V228E and Y207E are 32.9 Å,
29.7 Å and 21.9 Å from Ser111 respectively, and further still from Asp159 and
His191. No selected design is within interaction range of the active site.

This check also flags two candidates that appeared in the scored set but were
eliminated on other grounds: V161E and V161N place a charged side chain 2.8 Å
from Asp159 and 3.6 Å from His191, directly against the catalytic machinery.
They were removed during ranking rather than by an explicit proximity rule.

## 4. Discussion

The design logic rests on a single structural observation: in YpfH, predicted
aggregation propensity and structural disorder coincide in the same region, and
that region is far from the active site. This is a favourable situation for
solubility engineering, because it means the aggregation problem can be
addressed without any trade-off against catalysis. Where the two overlap, as
they do in many enzymes, the design space is much narrower.

The three designs are consistent across four independent predictors, which is
the main argument for taking them seriously. Aggrescan3D and CamSol use
different models of solubility, and FoldX and DynaMut use different models of
stability; agreement between them is more informative than a strong score from
any one tool.

Two methodological points are worth recording. First, the exclusion of the
active site was implemented by excluding the three catalytic residues
themselves, together with the conserved positions from the alignment. A
distance-based exclusion shell, for instance 8 Å to 10 Å from any triad atom,
would be a stronger rule and would have removed V161E and V161N at the
enumeration stage rather than later. Second, the Aggrescan3D profiling was run
on the AlphaFold Database entry for P76561, while the structural validation,
CamSol structural correction, geometry measurements and electrostatics were
computed on the project's own AlphaFold3 model. The two models agree on the
fold and on the location of the disordered tail, so the conclusions hold, but a
single structure throughout would be cleaner.

## 5. Limitations

- Every figure reported here is a prediction. No design has been expressed,
  purified or assayed, and no solubility or activity measurement supports any
  claim in this report.
- Aggregation predictors are trained largely on globular, ordered proteins. The
  C-terminal tail of YpfH is disordered, which is precisely where these methods
  are least reliable, and it is where all three designs sit.
- FoldX and DynaMut disagree in sign for most candidates in the set. Both are
  approximations, and the disagreement is a reason to treat the stability
  estimates as weak evidence rather than as a filter that either design passes
  cleanly.
- Single point mutations were considered in isolation. Whether the three
  designs are additive, redundant or antagonistic in combination is untested.
- The effect of these substitutions on catalytic activity is inferred from
  distance to the active site alone. Long-range effects on dynamics are not
  excluded.

## 7. Software and web servers

| Tool | Purpose | URL |
|---|---|---|
| NCBI BLASTp | homolog search | https://blast.ncbi.nlm.nih.gov/Blast.cgi |
| Clustal Omega | multiple sequence alignment | https://www.ebi.ac.uk/jdispatcher/msa/clustalo |
| AlphaFold3 | structure prediction | https://alphafoldserver.com |
| MolProbity | model validation | http://molprobity.biochem.duke.edu |
| Aggrescan3D | aggregation propensity, FoldX ΔΔG | https://biocomp.chem.uw.edu.pl/A3D2 |
| CamSol | solubility prediction | https://www-cohsoftware.ch.cam.ac.uk |
| DynaMut | stability prediction | https://biosig.unimelb.edu.au/dynamut |
| PSIPRED | secondary structure | https://bioinf.cs.ucl.ac.uk/psipred |
| PyMOL | visualization, SASA, distances | https://pymol.org |
| APBS | electrostatic potential | https://server.poissonboltzmann.org |

The presentation cites Kuriata et al., 2019 for Aggrescan3D. Primary citations
for the remaining tools should be taken from each tool's own citation page
before this report is submitted anywhere formal; they are deliberately not
reproduced from memory here.

## 8. Data availability

All sequences, structures, scoring data and figures underlying this report are
in the repository that contains it. 