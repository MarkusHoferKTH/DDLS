# KRAS pocket-region reliability specification

## Decision

The owner needs a defensible choice of which of three G-domain regions—the P-loop/glycine-rich loop, switch I, or switch II—is the most reliable primary pocket region for the nucleotide-pocket neighbourhood. This is a model-reliability/design hypothesis, not proof that an inhibitor binds. The output must recommend one region, or explicitly report a tie or failure.

## Protein and model

The assay protein is one KRAS polypeptide chain, approximately 169 residues in the supplied construct, specifically KRAS G12D, residues 1–169. It is not a multi-chain assembly. The protein functions as a single-chain, membrane-associated GTPase; interactions with GEFs, GAPs, and effectors are transient and are not represented as a stable assembly here.

The supplied structure is a predicted, single-chain ordinary KRAS AlphaFold DB model, not an experimental structure and not a nucleotide-loaded experimental state. It does **not** exactly match the bench construct: the assay protein has the G12D mutation and is truncated to residues 1–169 by removal of the floppy C-terminal tail, whereas the downloaded model is the ordinary KRAS entry and includes the additional tail. The CIF contains one KRAS chain rather than the assay purification tag; the owner does not remember that tag, and it is not part of the supplied KRAS sequence. Do not assume the tag is represented in either sequence or structure. KRAS identity alone is not sufficient to claim an exact construct match; explicitly record this sequence/construct mismatch and limit the comparison to the G-domain through residue 169, where the owner says the pocket-relevant biology lies. Verify the actual mapping in the files and exclude the extra tail.

Use these intended regions:

- P-loop/glycine-rich loop: residues 10–17 (`GxxxxGKS`); residue 12 is the assay G12D position.
- Switch I: approximately residues 30–38.
- Switch II: residues 59–76, with 59–67 as the core comparison interval; report the broader interval if useful.

The AlphaFold model has no defined GDP/GTP state in the supplied information. Treat switch conformations as a static structural hypothesis, not definitive nucleotide-state biology.

## Files

- `data/my_construct.fasta`: FASTA sequence used in the assay, with a header identifying KRAS G12D residues 1–169 and an amino-acid sequence.
- `data/KRAS_alphafold_model.cif`: mmCIF coordinates and chain/residue records for the single-chain predicted KRAS model. Its local pLDDT quality records are present, and pLDDT is represented in the atom B-factor column.
- `data/KRAS_alphafold_pae.json`: one-item JSON list containing `predicted_aligned_error` and `max_predicted_aligned_error`. The matrix is 189 × 189 in this copy; its maximum-value field is 31.75. Confirm position mapping from the file rather than assuming array positions.

The CIF-to-FASTA mapping must be checked using explicit chain, residue, sequence, gap, insertion-code, and alternate-record information. Do not infer mapping from array position alone. Exclude positions beyond 169, missing residues, and non-protein records.

## Exact claim and scope

The owner’s claim is: compare the P-loop, switch I, and switch II as candidate primary regions in the nucleotide pocket, and identify which is most reliable for guiding compounds. The claim concerns the three listed residue ranges in the KRAS G-domain, not the floppy C-terminal tail and not a protein-protein interface.

The result must not claim that the compound binds, that a region is the biological ligand site, or that the predicted switch arrangement proves a GDP- or GTP-state conformation. The immediate wet-lab use is to choose the region and residues to prioritise in the shared compound-design protocol for the Friday review.

## Matching confidence to the claim

For fold or region reliability, report local per-residue pLDDT for each region: the median plus a lower-tail statistic (minimum or lower quartile), so a bad stretch is visible. Use the pLDDT values mapped to explicit residue numbers and exclude the tail.

For how parts sit together, report PAE between regions: for every region pair, calculate the median PAE across all residue pairs and an upper-tail statistic (upper quartile or 90th percentile). PAE is in Ångström; lower values indicate lower positional uncertainty. Also report within-region uncertainty where needed to assess consistency.

Rank using high local confidence first, then low within-region uncertainty, then low uncertainty relative to the other two regions. If metrics conflict, report the conflict rather than silently combining unlike measures. Do not invent a universal cutoff without examining the distributions.

A tie is appropriate when leading regions overlap within their regional spread, no clear separation exists, or one wins local confidence but clearly loses on relative uncertainty. A failure is appropriate if residue indices cannot be mapped, required confidence entries are missing, or all three regions have poor confidence/large uncertainty. In either case, do not force a winner.

## Goal and definition of done

Done means a short table for **P-loop, switch I, and switch II** with local confidence and between-region uncertainty, restricted to the G-domain and mapped correctly to residues **1–169**, followed by one recommendation—or an explicit tie/failure.

The important qualification is that this ranks **model reliability**, not whether the compound truly binds there. The report must also record that the assay construct is **KRAS G12D, residues 1–169**, while the downloaded model is the **ordinary KRAS AlphaFold entry** with the additional tail, and that the model has **no defined nucleotide state**. These construct and state differences affect interpretation of switch geometry. The report must confirm that the model is KRAS, label the result as provisional model-reliability/design guidance, and identify experimental structure or binding/functional assays as needed validation, since none is supplied.

Write generated outputs to `results/`.
