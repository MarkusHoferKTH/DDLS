# KRAS pocket-region reliability report

## 1. The question

Dr. Yuki Tanaka needs one defensible region to prioritise in Friday’s medicinal-chemistry design review: the P-loop, switch I, or switch II around the KRAS nucleotide pocket. The answer will guide which residues the team records in its compound-design protocol; it is a model-reliability decision, not a claim that a compound binds.

**Recommendation up front:** on the supplied model, prioritise the **P-loop, residues 10–17**, as the most reliable of the three candidate regions. Treat this as provisional design guidance only.

## 2. The protein & the files

The owner supplied one KRAS polypeptide chain and an assay FASTA for **KRAS G12D, residues 1–169**. The structure is `data/KRAS_alphafold_model.cif`, a predicted single-chain ordinary KRAS AlphaFold model covering residues 1–189, not an experimental nucleotide-loaded structure. `data/KRAS_alphafold_pae.json` supplies the 189 × 189 predicted-aligned-error matrix. The sequence reference is `data/my_construct.fasta` (169 residues).

The comparison used only residues 1–169. The main limitation is that the supplied model is not the exact assay construct and has no defined GDP/GTP state. No multi-chain assembly or interface analysis is applicable.

## 3. The right confidence, stated plainly

For this fold/region claim, the matching evidence is **per-residue pLDDT**, not a whole-protein average. The relevant values are:

| Region | Residues | pLDDT median | Mean | Minimum | Q1 |
|---|---:|---:|---:|---:|---:|
| P-loop | 10–17 | **96.53** | **95.97** | **93.94** | 95.00 |
| Switch I | 30–38 | 83.31 | 83.47 | 77.75 | 81.62 |
| Switch II | 59–76 | 87.07 | 85.85 | 73.06 | 78.84 |
| Switch II core | 59–67 | 78.75 | — | 73.06 | 76.81 |

For relative placement, PAE gives: P-loop–switch I **5 Å median, 6 Å P90**; P-loop–switch II **4 Å median, 6 Å P90**; switch I–switch II **13 Å median, 17 Å P90**. Within-region off-diagonal PAE is **1 Å** for the P-loop, **6 Å** for switch I, **5 Å** for switch II, and **6 Å** for the switch II core.

The pLDDT-coloured structure in the viewer and the PAE heatmap agree with these numbers: the P-loop is uniformly high-confidence and internally tight; switch I is lower; switch II is mixed, with its 59–67 core weakest.

## 4. The structure check

This is a KRAS identity match, not an exact construct match. The model is one chain, chain A, with residues 1–189. The assay FASTA is one chain of 169 residues. Direct residue-number mapping is clean through 169: no gaps, insertion codes, or alternate records affect these regions.

There is one sequence difference within residues 1–169:

```text
position 12: model G  | assay D  (G12D)
```

The model also has residues 170–189, absent from the trimmed assay construct; these were excluded. The model is a monomeric chain, and this dataset does not support a functional dimer or protein–protein interface interpretation.

## 5. The trap and the honest truth

The biggest trap is treating an ordinary, nucleotide-state-undefined KRAS model as if it were the owner’s G12D 1–169 protein in the relevant GDP- or GTP-loaded state. I tested this by reconstructing the model sequence from explicit mmCIF residue records and aligning it directly to the FASTA, then checking the pLDDT and PAE at the specified residue ranges. The result is uncomfortable but clear: the P-loop wins the supplied model-reliability comparison, but residue 12 is **G12 in the model and D12 in the assay**. Its high confidence does not certify the same local geometry for G12D.

The undefined nucleotide state also weakens interpretation of switch geometry. A switch-I/switch-II comparison may not represent the conformation used in the owner’s compound-design experiment. The data support a ranking of this model’s confidence, not biological truth about the pocket.

## 6. The answer / recommendation

Prioritise **P-loop residues 10–17** for the next design discussion, with residue 12 explicitly flagged as a construct-mismatch position. It has the highest local pLDDT (median 96.53; minimum 93.94), the lowest within-region PAE (1 Å off-diagonal median), and low relative PAE to both switch regions.

This recommendation can be trusted only as **provisional model-reliability/design guidance** for the supplied AlphaFold model. It does not establish a ligand site, compound contact, binding, affinity, inhibition, or a GDP/GTP-specific switch arrangement.

## 7. Caveats & next steps

The key biological caveat is the missing nucleotide state. KRAS switch regions can change with GDP versus GTP or analogue loading, so the model’s switch placement may be less representative of the state relevant to the compound series; this uncertainty can make the P-loop appear safer to prioritise than the state-dependent switches really are.

Next, fold the actual G12D 1–169 sequence with the course fold service and compare its regional confidence and geometry. Then compare against an experimental KRAS structure in a matching nucleotide state, and test the compound with purified KRAS G12D under appropriate nucleotide-loading conditions. Use RAF-RBD recruitment/displacement or another functional assay, and test proposed contacts by mutation. None of these validations is present in the supplied files.

## 8. AI-use disclosure

The coding agent created the Git/uv setup, inspected the transcript and files, reconstructed and checked the mmCIF sequence mapping, calculated the pLDDT and PAE summaries, rendered the pLDDT structure and PAE heatmap, and built the local FastAPI viewer. I verified the reported identity, chain count, residue coverage, FASTA length, G12D difference, tail overhang, confidence values, matrix dimensions, and visual agreement against the files in this folder. The owner’s requirements and biological context come from `ddls-week4-interview.md`; the analysis scope is specified in `spec.md`. The viewer outputs are in `results/`, including `results/kras_plddt_structure.png` and `results/kras_pae_heatmap.png`.
