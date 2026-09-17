# Speaker notes — KRAS confidence before chemistry

## Slide 1 — Confidence before chemistry

**Title:** This project is about confidence before chemistry. Before choosing where to grow compounds, we need to know which part of the predicted KRAS pocket is most trustworthy.

**Question card:** Dr. Yuki Tanaka’s team needs one region for the Friday compound-design review: the P-loop, switch I, or switch II. The answer changes which residues the team discusses first.

**Three-region card:** I compare the three candidate regions, not the whole protein.

**Owner’s decision card:** The owner was relying on predicted pocket geometry. My conclusion is the P-loop, residues 10–17, but this is design guidance—not proof that a compound binds there.

## Slide 2 — From interview to audit

**Interview:** I first recorded the decision, the protein construct, the relevant regions, and what a useful answer would look like.

**AGENTS.md:** This gave the working rules: use the supplied files, use the reproducible `uv` environment, and match confidence to the claim.

**spec.md:** This turned the interview into a testable specification: P-loop 10–17, switch I 30–38, and switch II 59–76, with the 59–67 core also checked.

**Read files:** I mapped residues from the explicit mmCIF records, compared the model sequence with the FASTA, and then calculated the confidence values.

**Why these metrics:** pLDDT asks whether a local region is confidently folded. PAE asks how uncertain the placement is between regions. A whole-protein average would hide local differences.

## Slide 3 — Live model and matched confidence

**3D viewer:** This is the actual AlphaFold model, rotating during the presentation. It is coloured by pLDDT: greener means higher confidence. The coloured highlights mark the P-loop, switch I, and switch II; the grey tail is not used.

**Confidence panel:** The P-loop has median pLDDT **96.53**. Switch I is lower at **83.31**. Switch II is **87.07** overall, but its core is weaker at **78.75**. The P-loop is therefore the most uniform and confident region in this model.

## Slide 4 — The trap

**G12 versus G12D:** The model contains G12, while the assay contains D12. Because residue 12 is near the P-loop, the model’s high confidence does not guarantee the same geometry in the assay mutant.

**189 versus 169:** The model includes residues 170–189, while the assay stops at 169. I excluded that tail from the comparison.

**Main lesson:** A confident prediction of the wrong construct is not automatically a confident prediction of the experiment. The model has one chain, so an interface analysis is not relevant.

## Slide 5 — The answer

**Recommendation:** I would prioritise P-loop residues **10–17** for the next design discussion.

**Why:** It has the highest local pLDDT, a minimum of **93.94**, and low internal PAE of about **1 Å**. Its PAE to switch I and switch II is also low, at **5 Å** and **4 Å**.

**Comparison:** Switch I and switch II are much less certain relative to one another, with median PAE **13 Å**.

**Trust boundary:** This is the best-supported region in the supplied model. It is not a proven binding site and does not prove that a compound binds.

## Slide 6 — Limits and next week

**Biological limit:** The model has no defined GDP or GTP state, and it contains no ligand or partner proteins. Switch geometry may therefore differ from the state relevant to the owner’s experiment. This could make the P-loop look safer to prioritise than the switches really are in that state.

**Next checks:** I would fold the actual G12D 1–169 sequence, compare it with an experimental KRAS structure in the relevant nucleotide state, and test binding with appropriately nucleotide-loaded KRAS G12D.

**AI-use disclosure:** The coding agent helped set up the environment, map the structure, calculate pLDDT and PAE, make the figures, build the viewer, and draft the report. I checked the sequence mismatch, chain and residue coverage, matrix dimensions, calculations, and visual agreement against the files.

**Closing:** The P-loop is provisional model-reliability and design guidance—not evidence that a compound binds there.
