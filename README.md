# NBA Finals 2026 Series Predictor — OKC vs SAS

Monte Carlo series simulator built with Python. Predicts game-by-game outcomes
using a Poisson scoring model calibrated on 2025-26 season and playoff data.

## What this demonstrates

- End-to-end sports analytics pipeline: data collection → EDA → modelling → simulation
- Poisson regression applied to discrete scoring events
- Sample-size weighted combination of regular season and playoff data
- Monte Carlo simulation (10,000 runs) with structured home court scheduling

## Methodology

Data pulled via `nba_api` for both teams across the full 2025-26 season and
current playoff run. For each game, expected points (λ) are computed as the
average of the home team's location-split offensive rating and the away team's
location-split defensive rating. Games are simulated by drawing independently
from two Poisson distributions, repeated across the 2-2-1-1-1 home court format
until one team reaches 4 wins.

Series current state at time of modelling: 1-1 (2 games played).

## Results

| | Win Probability |
|---|---|
| OKC Thunder | 54.9% |
| SAS Spurs | 45.1% |

Series goes 6 or 7 games in 75.3% of simulations. Most likely single outcome:
OKC wins in 7 (20.7%).

## Repo structure
├── data/raw/          # Game logs pulled via nba_api
├── notebooks/
│   ├── 01_data_collection.ipynb
│   ├── 02_eda.ipynb
│   └── 03_model.ipynb
└── outputs/           # Simulation plots
## Setup

```bash
pip install nba_api pandas numpy scipy matplotlib seaborn
```

Run notebooks in order. Random seed fixed at 42 for reproducibility.