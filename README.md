# KRAS confidence viewer

A small FastAPI/Tailwind/3Dmol.js viewer for the supplied KRAS structure-confidence result. It reads `results/results.json`, the PDB from `results/`, and the PAE JSON from `data/`; no database is used.

## Run

```bash
uv run uvicorn app:app --host 127.0.0.1 --port 8000
```

Open http://127.0.0.1:8000/.

The Python dependencies are listed in `requirements.txt`. Install them with `uv pip install -r requirements.txt` if needed.
