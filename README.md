# CNCO: a career in data

A hand-compiled dataset covering CNCO, built the same way as the Why Don't We dataset. It is the data behind the interactive dashboard page `cnco-career.html`.

## Files

| File | Rows | What it holds |
|---|---|---|
| `releases.csv` | 66 | Singles, promotional singles, EPs and albums, with dates, certifications and chart notes |
| `events.csv` | 20 | Tours, TV, milestones and band-status events |
| `awards.csv` | 14 | Award entries and results where stated |
| `members.csv` | 5 | Members, origins and notes |
| `timeline.csv` | 9 | One row per year: era, members, releases and videos |
| `build.py` |  | Generates all CSVs, this README and the dashboard page. Edit the data modules, not the CSVs |

## Column notes

**`releases.csv`**
- `type`: `single`, `promo` (promotional single), `EP` or `album`. Singles where the group is a featured artist are `single` rows with the lead artist in `featured_artist` and "featured" in `notes`.
- `release_date` is ISO format, as precise as the sources allow. `date_precision` says `day`, `month` or `year`, so filter on it before doing date arithmetic.
- `project`: the album, EP or other release a track belongs to, or `Non-album`. This follows the grouping in the source table.
- `lineup`: who was in the group at release. `lineup` is `Quintet` before Joel Pimentel's exit on May 9, 2021 and `Quartet` from then on, by release date.
- `label`: Sony Latin, as listed in the sources.
- `certifications` uses the source's codes (RIAA, AMPROFON, MC, ARIA, BPI, RMNZ, FIMI and others). `chart_notes` holds only peaks the source states in its tables.

**`events.csv`**: `verification` flags weaker or conflicting items.

**`awards.csv`**: `result` is the outcome where the source states one.

**`timeline.csv`**: `members_during_year` counts anyone in the group at any point in the year; `members_at_year_end` counts who was in it on December 31 (hiatus years still count the members).

## Quick start

```python
import pandas as pd
r = pd.read_csv("releases.csv")
print(r.groupby(["year", "type"]).size().unstack(fill_value=0))
print(r[r.date_precision == "day"].sort_values("release_date").head())
```

## Known limitations

- Birth dates: the Wikipedia article gives ages only, so `born` is blank.
- Music videos: no director data was found, so there is no `videos.csv` for CNCO.
- Songs named in the article text but missing from the discography tables (Miami with Beele, Borrachita with Symon Dice) are not in `releases.csv`. Non-single guest spots (for example Todo Cambió, Súbeme la Radio, Vivir Sin Ti) appear only as collaborators.
- Tan Fácil and Quisiera: the table's US Latin peaks (#5, #26) differ from the text's Hot Latin Songs figures (#23, #29). The table is used and the text figure is noted.
- Que Quiénes Somos: the discography text says August 11, 2019; its table and the main article say October 11. October 11 is used.
- The data ends at the November 17, 2023 disbandment; later solo activity was not researched.
- **No streaming data**: none could be verified, so none is invented.

## Sources and licence

Compiled from Wikipedia and the press reports named in the dashboard footnote. Wikipedia text is licensed CC BY-SA, so if you redistribute this dataset, credit those sources and share it under a compatible licence. Verify anything important against the original sources before relying on it.
