# WorldCupBirthRateAnalysis ⚽ 🏆

> Does winning the FIFA World Cup trigger a baby boom 9 months later?

## Overview

This project analyzes whether FIFA World Cup-winning countries experience a statistically
meaningful uptick in birth rates in the year following their victory — a so-called
**"World Cup Baby Boom"** — and whether that effect correlates with national happiness scores.

The 9-month window corresponds to the biological lag between conception (peaking around
the July World Cup final) and birth (March–April of the following year).

## Data Sources

| Source | What | Coverage |
|--------|------|----------|
| World Bank API | Crude birth rate (SP.DYN.CBRT.IN) | ~200 countries, 1960–2024 |
| Our World in Data | Cantril Ladder happiness score | ~150 countries, 2011–2025 |
| Hardcoded | FIFA World Cup winners & hosts | 1930–2022 (22 tournaments) |

## Key Analyses

- Birth rate delta (post-win year vs. 5-year pre-win baseline) for all 22 winners
- Host country effect (separate from winners)
- Happiness index correlation (post-2012 wins)
- Statistical significance testing (one-sample t-test)

## Project Structure

```
WorldCupBirthRateAnalysis/
├── CLAUDE.md                                   # Collaborator notes for Claude
├── README.md                                   # This file
├── requirements.txt                            # Python dependencies
├── .gitignore                                  # Python/Jupyter standard ignores
├── data/
│   └── raw/                                    # API response caches (gitignored)
├── notebooks/
│   └── worldcup_birthrate_analysis.ipynb       # Main analysis notebook
└── assets/                                     # Saved chart images
```

## Setup

```bash
pip install -r requirements.txt
jupyter notebook notebooks/worldcup_birthrate_analysis.ipynb
```

The notebook is self-contained — Cell 1 auto-installs any missing packages.

## PDF Export

Run the export cell at the bottom of the notebook, or:

```bash
jupyter nbconvert --to html notebooks/worldcup_birthrate_analysis.ipynb
python -c "import weasyprint; weasyprint.HTML('notebooks/worldcup_birthrate_analysis.html').write_pdf('worldcup_analysis.pdf')"
```

## Methodology

For each World Cup win in year Y by country i:

$$\Delta BR_i = BR_{i,Y+1} - \overline{BR}_{i,Y-5:Y-1}$$

where $\overline{BR}_{i,Y-5:Y-1}$ is the 5-year pre-win rolling average birth rate.

## License

For research and educational use.
