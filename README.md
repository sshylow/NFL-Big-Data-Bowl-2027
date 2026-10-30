# NFL Big Data Bowl 2027: Does Combine Movement Carry Over for Defensive Backs?

My entry for the [NFL Big Data Bowl 2027](https://www.kaggle.com/competitions/nfl-big-data-bowl-2027), an analytics competition run by the NFL and hosted on Kaggle. This year's challenge: find which pre-draft measurements from the NFL Scouting Combine are most closely tied to success in the NFL.

## Goal

Covering a receiver is mostly about stopping and changing direction: back-pedal, plant, and drive toward the ball. The Combine's best-known test, the 40-yard dash, measures none of that. But the Combine also records every drill with 10 Hz sensor tracking, including a defensive-back drill built around exactly that back-pedal-and-transition move.

This project asks:

> **Does the way a defensive back moves in the Combine's transition drill (how hard he brakes, how much speed he keeps, how fast he drives back out) carry over to how he moves in NFL games, and does it predict who earns a starting role?**

If tracking-based movement traits explain NFL outcomes beyond standard Combine times, they're a better tool for evaluating defensive backs before the draft.

## Data

All data comes from the [competition dataset](https://www.kaggle.com/competitions/nfl-big-data-bowl-2027/data): 510 prospects drafted or signed from 2023 to 2025, across nine files (~2.2 GiB).

| File | Used for |
|---|---|
| `players.csv` | Position, college, draft round and pick |
| `combine_results.csv` | Standard Combine tests (40, splits, 3-cone, shuttle, jumps) and measurements |
| `combine_tracking.csv` | 10 Hz tracking of 6,310 Combine drill attempts |
| `player_play.csv` | Play-level context for every cohort snap: coverage scheme, assignment, pass result |
| `game_tracking_{2023,2024,2025}.csv` | 10 Hz in-game tracking of cohort players |
| `player_career_successes.csv` | Snaps, games active, games started, honors |
| `games.csv` | Schedule metadata |

Competition data isn't redistributable, so it isn't included in this repository.

## Approach

1. **Exploratory analysis** ([`01_eda.ipynb`](notebooks/01_eda.ipynb)). Who the prospects are, how Combine results vary by position, how deep passes travel, how long quarterbacks hold the ball, and which defensive backs see the field most.
2. **Combine movement profile.** From each back-pedal-and-transition attempt, locate the back-pedal peak, the plant, and the drive-out, then measure braking force, speed retained through the plant, and re-acceleration time.
3. **In-game reaction.** For every pass play a defensive back covers, measure his acceleration and distance covered in the first second after the throw.
4. **Comparison** ([`02_db_movement_transfer.ipynb`](notebooks/02_db_movement_transfer.ipynb)). Test whether movement traits predict in-game reaction and career outcomes beyond what the 40, 10-yard split, 3-cone, and shuttle explain, using repeated cross-validation to account for small Combine classes.

**Design note:** game tracking covers only the 510 cohort players, so most opposing receivers aren't tracked. Rather than defender-to-receiver separation, the in-game measure is the defender's own reaction once the ball is in the air.

## Repository structure

```
.
├── notebooks/
│   ├── 01_eda.ipynb                      # exploratory analysis
│   └── 02_db_movement_transfer.ipynb     # movement features, in-game reaction, model comparison
├── data/                                 # competition files (local only)
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

The notebooks also run directly on Kaggle, where they read from the competition's input folder.

## Progress

- [x] Research question and approach
- [x] Analysis pipeline
- [ ] Exploratory findings
- [ ] Movement-feature refinement
- [ ] Model comparison results
- [ ] Visuals and final write-up

## Glossary

- **Defensive back:** a cornerback or safety; the defenders primarily responsible for covering receivers on passing plays.
- **Back-pedal and transition:** running backward while facing the quarterback, then planting and driving forward toward the ball.
- **Scouting Combine:** an annual pre-draft event where prospects perform standardized drills and measurements.
- **Air yards:** how far a pass travels past the line of scrimmage before it's caught or falls incomplete.
