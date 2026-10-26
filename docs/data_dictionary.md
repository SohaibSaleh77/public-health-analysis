# Data dictionary

## The analysis panel — `data/processed/panel.csv`

One row per **country-year** (1950–2024), 248 countries and territories,
~18,300 rows. Missing values are `NaN`, never zero-filled. Built by
[data_wrangling](../notebooks/data_wrangling.ipynb).

| Column | Type | Description |
|---|---|---|
| `country` | str | Country name as OWID spells it |
| `iso3` | str | ISO 3166-1 alpha-3 code (a few territories use OWID codes, e.g. Kosovo = `OWID_KOS`; historical entities like `USSR` age out of the modern data) |
| `region` | str | Continent per OWID's mapping (Africa, Asia, Europe, North America, Oceania, South America) |
| `year` | int | Calendar year |
| `life_expectancy` | float | Life expectancy at birth, both sexes (years) |
| `child_mortality` | float | Under-five mortality (deaths per 1,000 live births) — *converted from OWID's percent, ×10* |
| `stunting` | float | Share of children under 5 stunted (%) |
| `dtp3` | float | DTP3 immunization coverage (% of one-year-olds) |
| `smoking` | float | Adults who smoke or use tobacco, age-standardised (%) |
| `obesity` | float | Adults with BMI ≥ 30, crude (%) |
| `air_pollution_deaths` | float | Ambient air pollution mortality, age-standardised (per 100,000) — **2019 only** |
| `gdp_pc` | float | GDP per capita, 2011 int'l $ (PPP) — Maddison |
| `health_spending_pc` | float | Current health expenditure per capita, PPP int'l $ |
| `schooling` | float | Mean years of schooling, adults 15+ |
| `water_access` | float | Population using improved drinking water sources (%) |
| `sanitation_access` | float | Population with improved sanitation facilities (%) |
| `population` | float | Total population (mid-year) |

`data/processed/world.csv` has the same indicator columns for OWID's own
"World" aggregate rows, where the source publishes one. I use theirs rather
than averaging countries — the UN's world life expectancy is a properly
constructed aggregate, not a mean of country values.

## Raw files — `data/raw/`

Thirteen CSVs, downloaded by hand from Our World in Data (not committed —
**[`data/raw/README.md`](../data/raw/README.md) lists the exact download
links**). Coverage below is after filtering to countries, 1950–2024:

| File | OWID chart (slug) | Indicator(s) taken | Coverage |
|---|---|---|---|
| `life_expectancy.csv` | `life-expectancy` | Life expectancy | 237 countries, 1950–2023 |
| `child_mortality.csv` | `child-mortality` | Under-five mortality | 201 countries, 1950–2024 |
| `gdp_per_capita.csv` | `gdp-per-capita-maddison-project-database` | GDP per capita | 169 countries, 1950–2022 |
| `schooling.csv` | `mean-years-of-schooling-long-run` | Mean years of schooling | 146 countries, 1950–2020 |
| `health_spending.csv` | `annual-healthcare-expenditure-per-capita` | Health expenditure pc | 193 countries, 2000–2024 |
| `dtp3_immunization.csv` | `child-mortality-vs-share-of-children-immunized-…` | DTP3 coverage | 194 countries, 1980–2024 |
| `smoking.csv` | `share-of-adults-who-smoke` | Smoking prevalence | 167 countries, 2000–2024 |
| `obesity.csv` | `share-of-adults-defined-as-obese` | Obesity prevalence | 199 countries, 1980–2024 |
| `air_pollution_deaths.csv` | `death-rate-ambient-air-pollution` | Air pollution mortality | 183 countries, **2019 only** |
| `water_access.csv` | `improved-water-sources-vs-gdp-per-capita` | Improved water access | 231 countries, 2000–2024 |
| `sanitation_access.csv` | `prevalence-of-stunting-vs-improved-sanitation-facilities` | Sanitation access, stunting | 232 / 163 countries, 1983–2024 |
| `population.csv` | `population` | Population | 247 countries, 1950–2023 |
| `continents.csv` | `continents-according-to-our-world-in-data` | Country → continent mapping | 272 entities |

Country counts are after filtering to the 1950+ country panel — the raw files
contain additional aggregate rows ("World", income groups, …) which data_wrangling.ipynb
removes by keeping only entities present in the continents mapping.

## Notes and quirks (things you'd otherwise discover the hard way)

- **Child mortality units.** OWID's column is a probability in percent; the
  panel converts to per-1,000 live births. If you compare raw vs processed
  numbers, processed = raw × 10. Sanity check in data_wrangling.ipynb: world 1990 =
  93.5 per 1,000, matching UN IGME's published ~93.
- **"soures".** The water-access column header really is spelled
  `Using improved drinking water soures`. Preserved verbatim in the raw file;
  if OWID ever fixes it upstream, the indicator registry at the top of
  data_wrangling.ipynb is the single place to update.
- **Air pollution is a single cross-section** (IHME GBD 2019 estimates). It
  appears in maps and cross-sectional correlations only — there is no time
  series to trend.
- **Maddison GDP ends in 2022** and uses 2011 international dollars; health
  spending uses *current* PPP dollars. The two income measures are not
  level-comparable — fine for separate fits, wrong for joint arithmetic.
- **Schooling ends in 2020** and covers 146 countries — the sparsest indicator
  in the set, and the missing countries are disproportionately poor.
- **Population ends in 2023**, which is why population-weighted series (and
  the world child-mortality trend) end in 2023 even where the underlying
  indicator runs to 2024.
- **Sanitation and stunting come from the same file** — OWID publishes them as
  a single scatter chart; the wrangling notebook extracts both columns.
- **Rows without a `Code` are aggregates.** The country filter in data_wrangling.ipynb
  (membership in `continents.csv`) exists to keep "High-income countries" out
  of the country panel.

## Derived files — `data/processed/`

Everything here is produced by the notebooks and gitignored; run them to
regenerate.

| File | Written by | Contents |
|---|---|---|
| `panel.csv` | data_wrangling.ipynb | The country-year panel described above |
| `world.csv` | data_wrangling.ipynb | OWID's own World aggregate rows |
| `data_coverage.csv` | data_wrangling.ipynb | Per indicator: countries reporting, year range, observation count |
| `le_rankings.csv` | country_comparison.ipynb | Countries ranked by latest life expectancy, with 1990 baseline and change since (≥500k population) |
| `spearman_correlations.csv` | socioeconomic_links.ipynb | Rank-correlation matrix across 11 indicators (latest snapshot) |
| `spearman_sample_sizes.csv` | socioeconomic_links.ipynb | Pairwise sample sizes for that matrix |
| `relationship_fits.csv` | socioeconomic_links.ipynb | Semilog fit results in per-doubling units (slope, R², n) for six indicator pairs |
