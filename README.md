# — Enhanced Humanized Blog Generator (Python + Agents + Notebooks)

This repository is a **clean, GitHub-ready port** of your n8n workflow to Python. It includes:
- Modular source code under `src/`
- CLI scripts under `scripts/`
- Two step-by-step **Jupyter notebooks** under `notebooks/` (“LLM planning” then “Agents & pipeline”)
- A `.env.example` with required environment variables
- `requirements.txt` to install dependencies

> The logic is converted from your original n8n JSON (Schedule → Sheets filter → context injection → planning → research → write → multi-layer humanization → internal linking → HTML → Drive doc → Sheet update).

## Quickstart

```bash
python -m venv .venv && source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env   # then fill the keys and IDs
python scripts/run_once.py
```

To run on the same cron as n8n (Tuesdays 01:02 Asia/Kolkata), use:
```bash
python scripts/schedule.py
```

## Structure

```
indmoney-bloggen/
├── .env.example
├── requirements.txt
├── README.md
├── src/
│   ├── __init__.py
│   ├── config.py
│   ├── google_io.py
│   ├── llm.py
│   ├── humanize.py
│   └── pipeline.py
├── scripts/
│   ├── run_once.py
│   └── schedule.py
└── notebooks/
    ├── 01_llm_planning.ipynb
    └── 02_agents_pipeline.ipynb
```

## Notebooks
- **01_llm_planning.ipynb** — walks through: context injection → preliminary plan → research (mocked for offline) → detailed plan prompts.
- **02_agents_pipeline.ipynb** — stitches the agents end-to-end and shows where Sheets/Drive write happen.

> Tip: The notebooks are safe to run without external calls by toggling `DRY_RUN=True` in the first cell.

## Notes
- Service Account must have access to both Sheets and the Drive folder.
- You can switch LLM providers via `LLM_PROVIDER` env var.
- Humanization layers and internal-linking prompts match the n8n code faithfully.


## n8n → Python mapping
See **N8N_MAPPING.md** for a node-by-node map of the entire workflow.
