# DDLS 2026 — Week 4 interview transcript

**Lab:** Interview a Structural-Biology Data Owner
**Data owner:** Dr. Yuki Tanaka — Medicinal chemist / drug-discovery researcher
**Student:** Markus Hofer <mackan.hofer@hotmail.com>
**Exported:** 2026-09-16T13:18:54+00:00

> This is the record of your interview with the data owner. Hand it to your analyst agent as the raw source for `AGENTS.md` and `spec.md`.

---

**You**:

I understand you have a protein that you want me to analyze - walk me through what it is and what you are trying to decide from its structure.

**Dr. Yuki Tanaka**:

I’m Dr Yuki Tanaka, a medicinal chemist at Uppsala, working on a small-molecule inhibitor for KRAS. I’ve given you the AlphaFold structure, the sequence of the KRAS protein used in our assay, and the accompanying confidence file. I need the nucleotide-pocket neighbourhood read properly—especially the glycine-rich loop, switch I, and switch II—so we know which residues line the groove my compounds should occupy. Ultimately, I need to decide which of those three regions is safest to record as the primary inhibitor pocket; can you extract that comparison from the files?

**You**:

I will get back to you about your dataset in a minute, but first tell me what molecule you have - is it one protein or is it several chains that act together? Roughly how many residues is it? Do you have the sequence (FASTA)?

**Dr. Yuki Tanaka**:

It’s **one KRAS polypeptide chain**, not several chains acting together. Think roughly **180 residues**, with the GTPase domain and a short C-terminal tail. Yes, `my_construct.fasta` is the sequence used in our assay; the CIF is the structure I want you to read. Which file are you starting from?

**You**:

Is there an experimental structure, or is it only a prediction? If it is a prediction, is it from the AlphaFold database or is it something that you ran yourself? Also, is the model a single chain or an assembly (a dimer / complex)?

**Dr. Yuki Tanaka**:

It’s **a prediction**, not an experimental structure—I downloaded the single-chain KRAS entry from the AlphaFold database; I didn’t run it myself. It’s just one KRAS chain, no dimer or protein complex. The relevant chemistry is the nucleotide-pocket side near the switch regions, not an interface between chains.

**You**:

What is the functional unit in the cell - does this protein work alone or does it partner up? If it does partner up, please describe the process and the partners it interacts with.

**Dr. Yuki Tanaka**:

KRAS functions mainly as a **single-chain, membrane-associated GTPase**, not as a stable dimer. GEFs such as SOS1 promote GDP release and GTP loading; GAPs such as NF1 accelerate GTP hydrolysis, and the GTP-bound form recruits effectors including RAF, PI3K, and RalGDS. Those are transient regulatory or signalling interactions—the protein itself is still one chain. Do you want the pocket analysis focused on the active GTP-bound state or the GDP-bound state?

**You**:

I am very interested in the biology of this project, but I am not very informed. Can you explain the theoretical background to me as if I was a child?

**Dr. Yuki Tanaka**:

KRAS is like a tiny **on/off switch** inside a cell. With GDP attached, it is mostly off; when a signal causes GTP to replace GDP, KRAS turns on and tells proteins such as RAF, PI3K, and RalGDS to make the cell grow or move.

Normally, helper proteins turn the switch on and off at the right time. In cancer, KRAS can become stuck on, so the cell keeps receiving “grow” instructions. My molecule is meant to sit in the groove around the nucleotide pocket and interfere with that switch—rather than binding to another protein partner. Do you want the child-level explanation of how an inhibitor blocks the switch, or of what the switch regions are?

**You**:

No, I think that is fine for now. I am ready to start analyzing your dataset. What files do you have - FASTA (for sequence), PDB / mmCIF (for structure)? What is in each file? Also please inform me how these files are structured.

**Dr. Yuki Tanaka**:

Three files:

- `my_construct.fasta` — the assay protein sequence in FASTA format: a header line beginning `>` followed by the amino-acid sequence.
- `KRAS_alphafold_model.cif` — the single-chain KRAS three-dimensional model in mmCIF format, with atom coordinates and chain/residue records.
- `KRAS_alphafold_pae.json` — the numerical confidence data bundled with that model.

The CIF is the structure file you need; the FASTA is there for reference, and the JSON is for judging how much confidence to put in different parts of the model. Do you want to start with the model contents or the confidence data?

**You**:

So this CIF file, does this file carry a per-residue confidence score? Also is there a PAE / error matrix?

**Dr. Yuki Tanaka**:

The **pairwise error matrix is in `KRAS_alphafold_pae.json`**, not in the CIF. The CIF may carry one confidence value per residue in its atom records, but I’m not the person to certify the exact column or encoding—read the file headers. For the pocket comparison, use the confidence data for the G-domain, not the tail; are you checking the three regions separately?

**You**:

Okay so the files need to be checked. What do you mean by the three regions? And why would they be checked separately or not? What would you recommend?

**Dr. Yuki Tanaka**:

By the three regions I mean:

- **P-loop / glycine-rich loop** at the start of the G-domain, which grips the phosphate end of GDP or GTP.
- **Switch I**, which helps sense the nucleotide state and contacts effectors.
- **Switch II**, another nucleotide-sensitive region and often the more useful groove for inhibitor contacts.

Check them separately because they are different stretches with different local confidence and relative positional uncertainty; averaging the whole protein would hide that. I’d compare their per-residue confidence and between-region error values, exclude the floppy tail, then rank the three—but I can’t recommend one before seeing those numbers. Are you extracting the values by residue or just reading a global summary?

**You**:

I think a global summary can be done but extracting values by residue is important to get a local fit score. But let me ask you this, does the model's sequence match the exact construct they use at the bench - Same length? Any mutations? Any tags or truncations?

**Dr. Yuki Tanaka**:

No—not exactly. **My assay construct is KRAS G12D, residues 1–169**; I trimmed off the floppy C-terminal tail. The downloaded AlphaFold model is the ordinary KRAS entry, and the CIF contains one KRAS chain rather than my assay tag; I don’t remember the purification tag, since it isn’t part of the KRAS sequence I gave you. KRAS is KRAS, though—the G-domain is what matters for the pocket. Are you checking the construct details before ranking the three regions?

**You**:

Okay, so what do you actually want to know from this - Is this region a real, stable fold? Where is the pocket? Does this binder bind? Which residues should you mutate? Finally, what wet-lab decision hangs on this?

**Dr. Yuki Tanaka**:

The immediate question is narrower: **which of the P-loop, switch I, or switch II is the most reliable primary pocket region** in the G-domain. I want the local confidence and between-region uncertainty compared, excluding the tail; that tells us whether the fold and pocket geometry are credible enough to guide compounds.

It will not prove that my binder binds, nor tell us which mutations to make—that needs assays and probably structures. The wet-lab decision is what region and residues to put into the shared design protocol before Friday, so the team aims the next compounds at the same groove. Can you rank those three regions with their numbers?

**You**:

Who will act on the answer, and how costly is being wrong?

**Dr. Yuki Tanaka**:

The medicinal-chemistry team will use it in Friday’s compound-design review to choose where to grow the hit series and which contacts to prioritise. Being wrong would waste a design cycle and compounds—not endanger a patient—but it could send us toward the wrong part of the nucleotide pocket, so I need a defensible region choice rather than a vague global confidence statement. Can you give me the three-region ranking with the local values?

**You**:

Yes, I can try. But before I try I want to check some things. How sure are you that the model is right for what you are trying to decide? Is there any experimental check I can do so I can make sure that the result I will be giving you does correspond to the biological truth? For instance if there is any crystal structure, an assay, or established biology I can use as a control? I just want to make sure that what I will be giving you corresponds to the biological truth.

**Dr. Yuki Tanaka**:

I’m **not sure enough to call the model biological truth**—it is a useful hypothesis for the G-domain, not evidence that the binder occupies that groove. The sensible controls are:

- Compare the P-loop and switch regions with an **experimental KRAS structure**, ideally G12D in the same nucleotide state; check whether the pocket geometry and switch conformations agree.
- Test the compound against purified KRAS G12D in a direct binding assay, with GDP- and GTP- or analogue-loaded protein as appropriate.
- Use a functional control such as **RAF-RBD recruitment or displacement**, and compare G12D with a suitable KRAS control; a structure alone cannot establish binding.
- If a contact is important, mutate that pocket residue and see whether binding and activity change—though that is a follow-up, not proof from the model.

For Friday, I would label the structural result as a **design hypothesis**, and not claim that the highest-confidence region is necessarily the ligand site. Do you have an experimental KRAS structure or a known reference binder available for comparison?

**You**:

No, I do not have an experimental KRAS structure or a known reference binder available for comparison.

**Dr. Yuki Tanaka**:

Then the structure-only result should stay provisional. You can still rank the three regions using the local confidence and between-region uncertainty, but that supports **model reliability**, not actual inhibitor binding; KRAS switch regions are nucleotide-state dependent and can move. For the design meeting, record the best-supported region as a hypothesis and flag experimental binding or a reference structure as the next validation step. Are you proceeding with the local comparison now?

**You**:

Yes, I think I understand what to do, but first let me ask you these final questions: Do you know the residue ranges and numbering for the P-loop, switch I, and switch II (especially accross the G12D 1-169 construct and ordinary AlphaFold sequence. Also, is there nucleotide state and construct-state assumptions for comparing switch conformations? I would also like a specification for this: What is the meaning, units, residue mapping, and aggregation method for local confidence and between-region PAE values? Also regarding the intended ranking output, how should per-region values and exclusion rules be set and what counts as a defensible tie or failure?

**You**:

Yes, I think I understand what to do, but first let me ask you these final questions: Do you know the residue ranges and numbering for the P-loop, switch I, and switch II (especially accross the G12D 1-169 construct and ordinary AlphaFold sequence. Also, is there nucleotide state and construct-state assumptions for comparing switch conformations? I would also like a specification for this: What is the meaning, units, residue mapping, and aggregation method for local confidence and between-region PAE values? Also regarding the intended ranking output, how should per-region values and exclusion rules be set and what counts as a defensible tie or failure?

**You**:

Yes, I think I understand what to do, but first let me ask you these final questions: Do you know the residue ranges and numbering for the P-loop, switch I, and switch II (especially accross the G12D 1-169 construct and ordinary AlphaFold sequence. Also, is there nucleotide state and construct-state assumptions for comparing switch conformations? I would also like a specification for this: What is the meaning, units, residue mapping, and aggregation method for local confidence and between-region PAE values? Also regarding the intended ranking output, how should per-region values and exclusion rules be set and what counts as a defensible tie or failure?

**Dr. Yuki Tanaka**:

Yes. For the usual KRAS numbering—and for my **G12D, residues 1–169 construct**—use:

- **P-loop:** residues **10–17** (`GxxxxGKS`)
- **Switch I:** approximately **30–38**
- **Switch II:** approximately **59–76**; use **59–67** for the core comparison and report the broader interval if needed

The G12D substitution is at position 12, inside the P-loop vicinity, but the numbering does not shift. The ordinary AlphaFold chain should use the same numbering through residue 169; its extra C-terminal residues should not enter this comparison.

For the state assumptions: AlphaFold is not a GDP- or GTP-loaded experimental structure, so do not treat its switch conformations as definitive. Compare the three regions as a **static structural hypothesis**, and do not interpret a switch-I versus switch-II difference as nucleotide-state biology unless the model or metadata explicitly defines a nucleotide state.

For the numerical specification, use the file’s residue indices—not array position by guesswork—and state the mapping explicitly:

- **Local per-residue value:** report the median across each region, plus the minimum or lower quartile so a single bad stretch is visible.
- **Pairwise positional uncertainty:** values are in **Å**; for each region pair, report the median across all residue pairs in the two regions, with the upper quartile or 90th percentile.
- Exclude residues beyond **169**, missing residues, and any non-protein records. Do not average the floppy tail into a G-domain result.
- Rank using high local confidence first, then low internal uncertainty, then low uncertainty between that region and the other pocket regions. Do not invent a universal cutoff without seeing the distributions.

A defensible tie is when the leading regions overlap within the spread of their regional values and no clear separation exists. A failure is an unmappable residue index, missing confidence entries, or uniformly poor confidence/large uncertainty across all three regions. In that case the protocol should say **no defensible primary region**, not force a winner.

**You**:

More regarding the files, is there exact sequence-to-structure residue mapping including gaps, insertion codes, and the G12D position? Also regarding interpretation of switch conformations and pocket geometry, is there nucleotide-state and construct mismatch and how should this be considered? Also please tell me more about the details in JSON file, tell me about the schema, confidence scale, and aggregation - Can this be confirmed against the file headers and indices? Finally regarding the ranking rule - this is only qualitative so far: should there be explicit handling of conflicting metrics and near-ties and how should it then be?

**Dr. Yuki Tanaka**:

I can tell you the biological mapping, but **I cannot certify the exact CIF-to-FASTA mapping or JSON headers without opening the files**—that is precisely your side of this.

For the intended G-domain comparison, use positions **1–169** and treat the usual KRAS labels as follows: P-loop **10–17**, switch I **30–38**, and switch II **59–76**. Position **12 is G12D** in my assay protein. The AlphaFold entry is the ordinary KRAS chain, so the labels should correspond through this domain, but you must verify whether the CIF contains gaps, insertion labels, alternate atoms, or a shifted starting position; do not infer that from array position alone. There should not be an insertion in these canonical KRAS segments, but check the actual coordinate records.

The construct and state issue matters. My assay protein is **G12D, residues 1–169**; the downloaded model is the ordinary AlphaFold KRAS entry and is not a nucleotide-loaded experimental structure. Therefore its switch-I/switch-II arrangement is a static prediction, not proof of either a GDP-like or GTP-like conformation. Compare pocket geometry only as a hypothesis, and avoid treating a small difference between switch regions as meaningful state biology.

For the JSON: the usual AlphaFold confidence file contains a square pairwise-value matrix, position lists for its two axes, and a maximum-value field. The matrix values are normally in **Ångström**, describing uncertainty in the placement of one position relative to another; lower is better. The per-residue confidence is generally carried separately in the CIF’s atom records, commonly repeated for atoms belonging to the same residue. But **confirm the actual keys, matrix dimensions, and position lists from this file’s headers**—I have not inspected your copy, so I will not pretend otherwise.

For ranking, make the rule explicit:

1. Exclude positions beyond 169 and any unmapped or missing records.
2. For each region, report the median local confidence and its lower-tail value.
3. For each region pair, report the median and high-tail pairwise uncertainty.
4. Rank first by local confidence, then by within-region consistency, then by uncertainty relative to the other two regions.
5. If those measures disagree, report the conflict rather than silently combining them.

Call it a **tie** when the leading regions are indistinguishable within the regional spread or when one wins local confidence but loses clearly on relative uncertainty. Call it a **failure** when the position lists do not map cleanly, required values are absent, or all three regions are poorly supported. In that case the protocol should say no primary region—not manufacture a winner because Friday is approaching.

**You**:

Okay, let me get this straight about what you want me to do - you want me to find out which of the P-loop, switch 1, and switch 2 that is the most reliable primary pocket region in the G-domain. You want the local confidence and between-region uncertainty compared (excluding the tail), and that tells us whether the fold and pocket geometry are credible enough to guide compunds?

**Dr. Yuki Tanaka**:

Yes, that’s the decision. Compare the three G-domain regions—P-loop, switch I, and switch II—using their local confidence and between-region uncertainty, excluding residues beyond 169. It gives us a **model-reliability ranking**, not proof that a compound binds; the practical output is one primary region, or an explicit tie/failure if the metrics do not separate them.

**You**:

I sent you this message:

**Dr. Yuki Tanaka**:

I don’t see the message itself—only that you sent one. Paste it here.

**You**:

Okay, let me get this straight about what you want me to do - you want me to find out which of the P-loop, switch 1, and switch 2 that is the most reliable primary pocket region in the G-domain. You want the local confidence and between-region uncertainty compared (excluding the tail), and that tells us whether the fold and pocket geometry are credible enough to guide compunds? Is this what done looks like? Have I missed anything?

**Dr. Yuki Tanaka**:

Yes, that is the core job. Done means a short table for **P-loop, switch I, and switch II** with local confidence and between-region uncertainty, restricted to the G-domain and mapped correctly to residues 1–169, followed by one recommendation—or an explicit tie/failure.

The important qualification is that this ranks **model reliability**, not whether the compound truly binds there. Also record that the assay construct is G12D while the downloaded model is the ordinary KRAS entry, and that the model has no defined nucleotide state; those affect interpretation of switch geometry.
