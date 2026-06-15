# CLAUDE.md

## Project Purpose

Analyze whether FIFA World Cup-winning countries experience a measurable birth rate bump
approximately 9 months after winning — the so-called "World Cup Baby Boom" — and whether
that effect correlates with national happiness scores.

**Final deliverable:** a styled Jupyter notebook (`.ipynb`) that exports cleanly to PDF.

---

## Collaborator Role

All design decisions, analytical framing, and direction are made by the human collaborator.
Claude implements, refactors, and debugs code. When in doubt about assumptions — ask before
changing analytical methodology. This is a collaborative project.

---

## Branch

`claude/worldcup-birthrate-analysis-7dqpa9`

---

## Data Sources

### 1. World Cup Results
Hardcoded DataFrame in the notebook. 22 tournaments (1930–2022).
- West Germany → `DEU` (World Bank uses unified Germany ISO for all German data)
- England → `GBR` (United Kingdom ISO)
- 2002: dual hosts Japan (`JPN`) and South Korea (`KOR`)

### 2. Birth Rates — World Bank REST API
- Indicator: `SP.DYN.CBRT.IN` (crude birth rate per 1,000 population)
- Single bulk API call for all 18+ ISOs: `date=1960:2024&per_page=2000`
- Cached to `data/raw/wb_birth_rates.json`

### 3. Happiness Index — Our World in Data
- Cantril Ladder self-reported life satisfaction (0–10 scale)
- Cached to `data/raw/owid_happiness.csv`
- Available from 2011 onward → covers post-2012 WC wins only

---

## Core Analytical Assumptions

- **9-month proxy:** WC finals are in late June/July. Births occurring ~9 months later
  (March–April Y+1) appear in the annual birth rate for year Y+1.
- **Baseline:** 5-year rolling average of birth rate in years Y-5 to Y-1 (min. 3 years)
- **Delta:** `birth_rate_delta = BR[Y+1] - mean(BR[Y-5:Y-1])`
- **Coverage:** WB data starts 1960; wins before 1966 have partial/no baselines

---

## Data Directory

`data/raw/` — gitignored CSV/JSON caches from API calls. The directory is tracked via
`.gitkeep` but the actual data files are not committed.

---

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook notebooks/worldcup_birthrate_analysis.ipynb
```

Or run cells top-to-bottom — Cell 1 auto-installs missing packages.

---

## PDF Export

Run the export cell at the bottom of the notebook, or manually:

```bash
jupyter nbconvert --to html notebooks/worldcup_birthrate_analysis.ipynb
# Then use weasyprint to convert HTML to PDF
python -c "import weasyprint; weasyprint.HTML('notebooks/worldcup_birthrate_analysis.html').write_pdf('worldcup_analysis.pdf')"
```

LaTeX alternative (requires `apt install texlive-xetex texlive-fonts-recommended`):
```bash
jupyter nbconvert --to pdf notebooks/worldcup_birthrate_analysis.ipynb
```

---

## Visual Design

- **Background:** `#0d4a28` (dark forest green)
- **Accent:** `#FFD700` (gold)
- **Sage:** `#e8f5e0` (table even rows, fills)
- **Border radius:** 8–15px on HTML section headers
- Soft edges, no spines, dashed grid `#2d8c55`

---

## Conventions

- Commit messages: imperative mood, present tense, ≤72 chars
- One logical change per commit
- Never commit `data/raw/` files (gitignored)
- Clear notebook outputs before committing:
  ```bash
  jupyter nbconvert --ClearOutputPreprocessor.enabled=True --to notebook \
    --inplace notebooks/worldcup_birthrate_analysis.ipynb
  ```
