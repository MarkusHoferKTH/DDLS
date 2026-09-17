# KRAS construct-specific reliability report

## 1. The question

Dr. Yuki Tanaka needs a defensible region to prioritise in Friday’s compound-design review: the P-loop, switch I, or switch II around the KRAS nucleotide site. The answer is about model reliability and provisional design guidance; it does **not** assign a primary ligand pocket.

## 2. The headline mismatch

Before ranking the regions, the models must be distinguished:

| | WT AlphaFold reference | Actual assay construct fold |
|---|---|---|
| Sequence at 12 | KRAS **G12** | KRAS **G12D**: **D12** |
| Coverage | residues **1–189** | residues **1–169** |
| Model type | computational single-chain prediction | computational single-chain fold-service prediction |
| Nucleotide state | not defined | not defined |

D12 lies inside the P-loop comparison range, residues 10–17. The two computational predictions therefore do not represent exactly the same sequence or construct. This difference supports caution; it does not by itself establish a biological conformational change.

## 3. The right confidence, stated plainly

For a local fold claim, the relevant evidence is per-residue pLDDT. PAE answers a different question: uncertainty in relative placement. A pLDDT difference is not a PAE difference, and neither is a measurement of biological mechanism.

### WT versus actual G12D construct

| Region | WT AlphaFold median / mean / minimum pLDDT | G12D construct median / mean / minimum pLDDT |
|---|---:|---:|
| P-loop 10–17 | **96.53 / 95.97 / 93.94** | **87.05 / 86.94 / 80.13** |
| Switch I 30–38 | **83.31 / 83.47 / 77.75** | **68.17 / 68.27 / 62.00** |
| Switch II 59–76 | **87.07 / 85.85 / 73.06** | **65.59 / 67.26 / 45.12** |
| Switch II core 59–67 | **78.75 / not previously reported / 73.06** | **57.44 / 55.84 / 45.12** |

WT G12 has pLDDT **93.94**; construct D12 has pLDDT **80.13**. The WT-to-construct P-loop change is therefore −9.49 in the median and −13.81 in the minimum. These are differences between two predictions, not proof that G12D causes a conformational change.

## 4. PAE comparison

The construct fold returned one chain of 169 residues and a **169 × 169** PAE matrix. Residue numbers map directly to matrix index `residue − 1`.

### Between-region PAE, Å

| Region pair | WT median / Q3 / P90 | G12D median / Q3 / P90 |
|---|---:|---:|
| P-loop × switch I | 5 / 5 / 6 | **3.10 / 3.34 / 3.44** |
| P-loop × switch II | 4 / 5 / 6 | **3.20 / 4.36 / 4.80** |
| Switch I × switch II | 13 / 15 / 17 | **8.53 / 9.38 / 10.45** |

### Within-region PAE, off-diagonal median, Å

| Region | WT | G12D construct (median / Q3 / P90) |
|---|---:|---:|
| P-loop 10–17 | 1.0 | **1.03 / 1.22 / 1.40** |
| Switch I 30–38 | 6.0 | **3.47 / 5.52 / 6.75** |
| Switch II 59–76 | 5.0 | **4.47 / 7.16 / 12.35** |
| Switch II core 59–67 | 6.0 | **5.58 / 7.53 / 9.43** |

The construct PAE supports a tightly placed P-loop, with a within-region median of 1.03 Å. Its PAE values between candidate regions are also lower than the WT values. This does not cancel the lower construct pLDDT in the P-loop or switches: pLDDT and PAE answer different questions.

## 5. Structure and sequence check

The WT mmCIF is chain A, residues 1–189. The assay FASTA and construct fold are chain A, residues 1–169. Mapping through residue 169 is direct. The only sequence difference in that range is position 12: WT G versus construct D. WT residues 170–189 are absent from the assay construct and were excluded.

The construct PDB contains coordinates and pLDDT in its B-factor column. The corrected WT PDB used for the static viewer also contains the mmCIF pLDDT values in its B-factor column; the earlier exported zero-B-factor PDB is not used as evidence.

Coordinate orientations differ between predictions, so raw XYZ values were not interpreted as a conformational change. The observable local difference is that the construct model contains a D12 side chain while the WT model contains glycine. Both models retain a continuous, modelled P-loop; this supports a local geometry observation but not a biological mechanism.

## 6. The answer / recommendation

**P-loop 10–17 is the leading construct-specific model-reliability region, but no primary ligand pocket can be assigned from these predictions alone.**

The P-loop remains the leading region for model-reliability and provisional design-guidance purposes in the G12D 1–169 construct: its median pLDDT is 87.05, above switch I at 68.17 and switch II at 65.59, and its within-region PAE is 1.03 Å. However, this is not a claim that the P-loop is the primary inhibitor pocket. A nucleotide-state-undefined prediction cannot establish which groove or region is occupied by the compound.

Switch II may still be chemically relevant even though its predicted geometry is less reliable. Lower pLDDT or PAE values must not be converted into a claim about biological mechanism.

## 7. The trap, caveat, and next steps

The central trap was treating the WT AlphaFold reference as the owner’s G12D assay protein. The P-loop is part of the nucleotide-site region and D12 lies within it, so the reduced construct-specific confidence is relevant to interpreting the model. However, because the prediction does not define nucleotide-loading state or compound occupancy, it cannot determine whether the P-loop or switch II is the chemically relevant ligand region.

The interview and supplied files do **not** document the assay’s actual nucleotide-loading condition. Whether the experiment uses GDP, GTP, or a nucleotide analogue remains an open experimental/context question; no GDP/GTP state is inferred from either prediction.

Next, compare these predictions with an experimental KRAS structure in the relevant nucleotide state and test purified G12D KRAS under the documented or deliberately controlled loading conditions. Binding and functional assays, including RAF-RBD recruitment/displacement where appropriate, are still needed.

## 8. AI-use disclosure

The coding agent set up the environment, inspected the transcript and files, submitted the actual FASTA to the course fold service, saved the construct fold, calculated the focused comparisons, corrected the WT viewer PDB B-factors, and updated the report and presentation. I verified sequence identity, chain counts, residue coverage, D12/G12 mapping, matrix dimensions, confidence statistics, and the limits of coordinate comparison. Sources are `ddls-week4-interview.md` and `spec.md`.
