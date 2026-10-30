# Staying Attached: Do Combine Movement Traits Predict Coverage Tightness for Defensive Backs?

My entry for the [NFL Big Data Bowl 2027](https://www.kaggle.com/competitions/nfl-big-data-bowl-2027), an analytics competition run by the NFL and hosted on Kaggle.

## Goal

When a defensive back covers a receiver, his job is to stay close enough to contest the catch. The NFL Scouting Combine is known for its 40-yard dash, but straight-line speed says little about *how* a player moves: how hard he can brake, how quickly he flips direction, and how much speed he keeps through a turn.

This project asks:

> **Do defensive backs who change direction more efficiently at the Combine stay tighter on receivers in NFL games, and does that edge depend on how far downfield the throw is?**

My hypothesis is that change-of-direction traits matter most on short and intermediate throws, where routes are sharp and reactive, while top speed matters more on deep balls. If movement traits explain coverage tightness better than straight-line speed alone, that's useful information for evaluating defensive backs before the draft.

## Data

All data comes from the [NFL Big Data Bowl 2027 competition](https://www.kaggle.com/competitions/nfl-big-data-bowl-2027/data):

- **Combine tracking** from the 2023, 2024, and 2025 NFL Scouting Combines, recorded by sensors in players' shoulder pads every 0.1 seconds (position, speed, acceleration, deceleration, distance)
- **In-game tracking** for the same players during their NFL snaps
- Five position groups, of which this project focuses on **defensive backs** (cornerbacks and safeties)

Competition data isn't redistributable, so it isn't included in this repository.

## Approach

1. **Combine movement profile.** From change-of-direction drills, compute each defensive back's peak deceleration, peak re-acceleration, and speed retained through turns, alongside straight-line top speed.
2. **Coverage tightness.** From game tracking, measure the distance between each defender and the nearest receiver at the moment the pass arrives, grouped into short, intermediate, and deep throws.
3. **Comparison.** Test whether movement traits explain coverage tightness beyond what top speed explains, using cross-validation and weighting players by number of targeted snaps to account for small samples.
4. **Interpretation.** Illustrate the findings with player examples and play animations, and translate them into something a scout could use.

## Repository structure

```
.
├── notebooks/
│   └── 01_explore_and_baseline.ipynb   # data inventory, feature pipelines, baseline model
├── data/                               # competition files (local only)
├── requirements.txt
└── README.md
```

## Running locally

```bash
pip install -r requirements.txt
kaggle competitions download -c nfl-big-data-bowl-2027 -p data/
unzip data/nfl-big-data-bowl-2027.zip -d data/
jupyter lab
```

The notebook also runs directly on Kaggle, where it reads from the competition's input folder.

## Progress

- [x] Research question and approach
- [ ] Data inventory
- [ ] Combine movement features
- [ ] Coverage-tightness measure
- [ ] Model comparison
- [ ] Visuals and final write-up

## Glossary

- **Defensive back:** a cornerback or safety; the defenders primarily responsible for covering receivers on passing plays.
- **Separation:** the distance between a receiver and the nearest defender. Less separation means tighter coverage.
- **Scouting Combine:** an annual pre-draft event where prospects perform standardized drills and measurements.
