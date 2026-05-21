# NBA Playoffs 2026 Series Predictor

Monte Carlo series simulator built with Python. Predicts game-by-game outcomes
using a Poisson scoring model calibrated on 2025-26 season and playoff data.

Started this to predict the OKC vs SAS Western Conference Finals, but ended up
extending it to simulate the NBA Finals matchup as well once the brackets locked in.

## What this demonstrates

- End-to-end sports analytics pipeline: data collection → EDA → modelling → simulation
- Poisson regression applied to discrete scoring events
- Sample-size weighted combination of regular season and playoff data
- Monte Carlo simulation (10,000 runs) with structured home court scheduling
- Updating priors as series state changes (live series adjustment)

## Methodology

Data pulled via `nba_api` for both teams across the full 2025-26 season and
current playoff run. For each game, expected points (λ) are computed as the
average of the home team's location-split offensive rating and the away team's
location-split defensive rating. Games are simulated by drawing independently
from two Poisson distributions, repeated across the 2-2-1-1-1 home court format
until one team reaches 4 wins.

## Results

### Western Conference Finals — OKC Thunder vs SAS Spurs

Series state at time of modelling: 1-1 (2 games played).

| | Win Probability |
|---|---|
| OKC Thunder | 54.9% |
| SAS Spurs | 45.1% |

Series goes 6 or 7 games in 75.3% of simulations. Most likely single outcome:
OKC wins in 7 (20.7%).

---

### NBA Finals — OKC Thunder vs NYK Knicks

Pretty much every major outlet (ESPN, The Athletic, Bleacher Report) had OKC
as heavy favourites going into this matchup — makes sense on paper given their
regular season dominance and the way they dismantled SAS.

The model disagrees.

| | Win Probability |
|---|---|
| **NYK Knicks** | **50.6%** |
| OKC Thunder | 49.4% |

Essentially a coin flip, but the simulation consistently edges NYK across all
10,000 runs. The key driver is NYK's defensive rating at home (MSG) — it's
significantly better than what OKC faced in the West, and the 2-2-1-1-1
format gives the higher seed (NYK, East 1) home court, which the model weights
heavily through the location-split λ calculation.

Media narrative leans OKC because of SGA and their offensive ceiling. The model
sees NYK's defensive floor as the equaliser. Series probably goes 6-7 games either
way — most likely outcome in the simulation is NYK in 7 (18.4%).

## Repo structure

```text
├── data/raw/          # Game logs pulled via nba_api
├── notebooks/
│   ├── 01_data_collection.ipynb
│   ├── 02_eda.ipynb
│   └── 03_prediction_model.ipynb
└── outputs/           # Simulation plots + championship probability chart
```

## Setup

```bash
pip install nba_api pandas numpy scipy matplotlib seaborn
```

Run notebooks in order. Random seed fixed at 42 for reproducibility.
