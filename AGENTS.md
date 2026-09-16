# Operating instructions

See `spec.md` for the authoritative problem specification, scope, residue regions, confidence requirements, and definition of done.

## Environment

Use `uv` for Python environments: create the environment with `uv venv`, and run all Python with `uv run`. This is intended to work the same way on every OS.

## Data and structure loading

Input data lives in `data/`:

- `data/my_construct.fasta` is the assay sequence: KRAS G12D, residues 1–169.
- `data/KRAS_alphafold_model.cif` is a predicted, single-chain AlphaFold KRAS mmCIF model. It contains atom coordinates and residue records; its local pLDDT values are in the mmCIF local quality records and are represented per residue (also reflected in the atom B-factor column). Read the mmCIF headers and map residues by their explicit identifiers, not array position.
- `data/KRAS_alphafold_pae.json` is the confidence file. It contains `predicted_aligned_error`, a square pairwise matrix, and `max_predicted_aligned_error`; values are in Ångström and lower is better. Confirm dimensions and position mapping from the file before using it.

When loading a structure, use the pLDDT in the mmCIF B-factor column for per-residue confidence, and use PAE from the JSON. Exclude residues beyond 169, missing residues, and non-protein records from the G-domain comparison. Write outputs to `results/`.

## Fold service

To fold a sequence that is not in the AlphaFold DB, read https://ddls-structure-api-8a7d6803.svc.hypha.aicell.io/skill.md and follow it. Load the fold key from `DDLS_FOLD_KEY` in `.env` with `set -a; source .env; set +a`, then send it as the Bearer token. Never write the key itself into `AGENTS.md` or any other committed file.

## Version control

This folder is a git repository. Commit the current state *before* any big change—such as installing packages, rewriting a working file, or making a large refactor—and commit again whenever something starts working. Use short, clear messages.

## Reporting rule

Never report an answer about a structure without first reporting the confidence that matches the claim and confirming that the model is actually this protein.
