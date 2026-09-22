# Improving the solubility of YpfH esterase by rational design

Computational redesign of the *Escherichia coli* esterase **YpfH** to reduce
aggregation propensity and improve predicted solubility, without disturbing the
catalytic machinery.


---

## Target

| | |
|---|---|
| Protein | Esterase YpfH |
| Organism | *Escherichia coli* str. K-12 substr. MG1655 |
| RefSeq | [NP_416968.2](https://www.ncbi.nlm.nih.gov/protein/NP_416968.2) |
| UniProt | [P76561](https://www.uniprot.org/uniprotkb/P76561/entry) |
| Length | 232 aa |
| Catalytic triad | Ser111, Asp159, His191 (Ser-His-Asp charge relay) |
| Function | Esterase activity toward palmitoyl-CoA and pNP-butyrate |

## Result

Three single point mutations were selected from 37 scored candidates:

| Design | ΔA3D | FoldX ΔΔG (kcal/mol) | ΔCamSol intrinsic | ΔCamSol struct. corr. | DynaMut ΔΔG (kcal/mol) | Distance to triad |
|---|---|---|---|---|---|---|
| L218R | -0.0370 | -0.6365 | +0.1566 | +9.49 | -0.279 | 32.9 Å |
| V228E | -0.0305 | -0.3700 | +0.1214 | +5.74 | -0.082 | 29.7 Å |
| Y207E | -0.0247 | +0.4400 | +0.0389 | +4.24 | +0.422 | 21.9 Å |

All three sit in the exposed, aggregation-prone C-terminal region and are
between 22 Å and 33 Å from the nearest catalytic residue, so none of them
contacts the active site.

> **Sign conventions differ between the two stability predictors.** FoldX
> ΔΔG is negative for stabilizing substitutions. DynaMut ΔΔG is positive for
> stabilizing substitutions. The two columns above must not be compared
> directly.

## Mutation notation

Aggrescan3D labels mutations as `[WT][MUT][POSITION][CHAIN]`, so **`LR218A`
means L218R in chain A.** The original analysis was
carried out in that notation and it appears throughout the presentation and the
scoring spreadsheet. Everything in `data/scoring/mutation_scores.csv` and in
this README uses standard notation instead, with the original label kept in the
`a3d_label` column so the two can be matched.

## Method

1. **Homolog collection.** BLASTp of YpfH against SwissProt and ClusteredNR.
   The working set is 31 sequences (the query plus 30 homologs, 5 of them from
   SwissProt), at roughly 25 % to 60 % identity to the query.
2. **Conservation.** Clustal Omega multiple sequence alignment over that set.
   Ser111, Asp159 and His191 are conserved in all 31 sequences, as are 18L,
   23H, 24G, 65W, 95Q, 105T, 109G, 110F, 112Q, 113G, 136G, 155H and 156G.
   These positions were excluded from mutagenesis.
3. **Structure.** AlphaFold3 model of the wild type (pTM 0.92, mean pLDDT 91.7,
   5 seeds, no clashes). Validated with MolProbity: score 1.28 (99th
   percentile), clashscore 5.21 (93rd percentile), 0 Ramachandran outliers,
   98.26 % Ramachandran favored.
4. **Active site check.** The triad geometry in the model is consistent with a
   functional Ser-His-Asp relay: Ser111 OG to His191 NE2 is 2.79 Å, and
   Asp159 OD2 to His191 ND1 is 2.62 Å, both within hydrogen-bonding distance.
5. **Aggregation profiling.** Aggrescan3D on the wild-type structure.
   Wild-type average A3D score -0.5513, minimum -3.534, maximum +2.2961, total
   -127.9095. Aggregation-prone residues near the active site were discarded.
6. **Target region.** The C-terminal tail is the least ordered part of the
   protein and carries the highest A3D scores while being fully solvent
   exposed. Per-residue pLDDT falls below 70 from residue 221 and below 50 from
   residue 225. 25 of the 37 candidate mutations fall at position 200 or later.
7. **Scoring.** Each candidate was scored on Aggrescan3D average score, FoldX
   ΔΔG (via Aggrescan3D), CamSol intrinsic solubility, CamSol structurally
   corrected solubility, DynaMut ΔΔG, and SASA-derived exposure before and
   after substitution.
8. **Filtering.** 37 candidates were ranked by A3D score and cut to 16, then to
   12 by requiring ΔCamSol structurally corrected > 0, then to 11 by discarding
   I229D on inspection of the APBS surface potential of the mutant model, and
   finally to the 3 designs above.
9. **Surface electrostatics.** APBS electrostatic potential maps of wild type
   and mutants in PyMOL, used to confirm that each accepted substitution
   changes the local surface charge in the intended direction without opening
   a new hydrophobic patch.

## Repository layout

```
data/
  sequences/
    ypfH_wildtype.fasta                  query sequence
    mutants.fasta                        12 modelled mutants, standard notation
    blastp_homologs_clusterednr.fasta    31-sequence working set used for the MSA
    blastp_hits_swissprot.fasta          SwissProt hits
  structures/
    wildtype_af3/                        AlphaFold3 job: 5 models, confidences,
                                         PAE, MSAs, templates, job request
    mutants_af3/                         12 mutant models, named by mutation
  scoring/
    mutation_scores_master.xlsx          raw scoring spreadsheet (source of truth)
    mutation_scores_earlier_version.xlsx earlier snapshot, kept for provenance
    mutation_scores.csv                  generated: standard notation + distances
    camsol/                              CamSol raw output
    apbs_io.mc                           APBS run log
figures/                                 PyMOL and analysis figures
docs/
  project_presentation.pdf               full slide deck
  report.md                              written report
```


## Structure provenance

Two structures of the wild type were used at different stages, and they are not
the same model:

- **AlphaFold3 model** (`data/structures/wildtype_af3/`): used for MolProbity
  validation, the CamSol structurally corrected calculation, the catalytic
  triad measurements, the PyMOL figures and the APBS surface potential maps.
  `ypfH_wildtype_af3_model_0.pdb` is seed model 0 of that job in PDB format.
- **AlphaFold Database entry AF-P76561-F1**: the structure loaded into the
  Aggrescan3D web server, as shown in the A3D interface screenshots in the
  presentation. The per-residue confidence values reported in the candidate
  tables come from that entry, and they do not match any of the five AlphaFold3
  seeds in this repository.

The two models agree on the fold and on the location of the disordered tail, so
the conclusions are unaffected, but anyone re-running Aggrescan3D should note
which input was used.


## Status

Computational design only. None of the three designs has been expressed or
assayed, and the solubility and stability figures are predictions from
Aggrescan3D, FoldX, CamSol and DynaMut rather than measurements.

## Tools

AlphaFold3 · Aggrescan3D · FoldX (via Aggrescan3D) · CamSol · DynaMut ·
MolProbity · PyMOL · APBS · Clustal Omega · PSIPRED · NCBI BLASTp

# Authors

Department of Biotechnology, University of Tehran.

- Reyhane Esmaili
- Diba Hamzavi
- Mohadese Rasouli
- Hossein Teimouri
