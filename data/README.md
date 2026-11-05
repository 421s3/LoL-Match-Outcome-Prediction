# Data

The dataset is **not included** in this repository. Download it yourself and place the three files listed below in this folder.

## Source

Nathan Smallcalder. "League of Legends (LoL) Matches Patch 25.19+". Kaggle, 2026.
https://www.kaggle.com/datasets/nathansmallcalder/lol-match-history-and-summoner-data-80k-matches

The data was collected from the Riot Games API (patch 25.19) and covers roughly 279,000 matches. The Kaggle page lists the license as "Other (specified in description)" and the description does not grant redistribution rights, so the files are not republished here. Check the dataset page and Riot's API terms before reusing the data. The dataset page states that no summoner names, IDs or PUUIDs are included.

## Files used

Place these in `data/`:

| File | Columns used |
|---|---|
| `MatchTbl.csv` | `MatchId`, `QueueType`, `RankFk`, `GameDuration` (seconds) |
| `TeamMatchTbl.csv` | `MatchFk`, `BlueWin`, and the Blue/Red `Kills`, `TowerKills`, `DragonKills`, `BaronKills`, `RiftHeraldKills` |
| `RankTbl.csv` | `RankId`, `RankName` |

The other tables in the dataset (summoner, champion and item tables) are not used.

## Filtering applied in the analysis

Matches are kept if `QueueType` is `CLASSIC` (ranked Summoner's Rift) and the rank is not "Unranked", then games under 15 minutes are removed. That leaves 110,521 matches.
