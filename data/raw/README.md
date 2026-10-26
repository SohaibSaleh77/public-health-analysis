# Raw data — what goes in this folder

The notebooks read a set of CSVs downloaded by hand from
[Our World in Data](https://ourworldindata.org). The files are **not committed
to the repo** (they're gitignored), so after cloning you need to fetch them
yourself — it takes about two minutes. Every file is a small, plain CSV
downloaded from a chart page (the "Download" button under any OWID chart gives
you the same file; the links below are the direct ones).

Grab each of these, keep the **exact filenames** below, and drop them into
this folder:

| File in this folder | OWID chart | Direct link | Primary source | Years |
|---|---|---|---|---|
| `life_expectancy.csv` | [Life expectancy](https://ourworldindata.org/grapher/life-expectancy) | [CSV](https://ourworldindata.org/grapher/life-expectancy.csv) | UN WPP (2024); Zijdeman et al. (2015); Riley (2005) | 1950–2023 |
| `child_mortality.csv` | [Child mortality](https://ourworldindata.org/grapher/child-mortality) | [CSV](https://ourworldindata.org/grapher/child-mortality.csv) | UN IGME (2024) | 1950–2024 |
| `gdp_per_capita.csv` | [GDP per capita, Maddison](https://ourworldindata.org/grapher/gdp-per-capita-maddison-project-database) | [CSV](https://ourworldindata.org/grapher/gdp-per-capita-maddison-project-database.csv) | Maddison Project Database 2023 | 1950–2022 |
| `schooling.csv` | [Mean years of schooling, long run](https://ourworldindata.org/grapher/mean-years-of-schooling-long-run) | [CSV](https://ourworldindata.org/grapher/mean-years-of-schooling-long-run.csv) | Lee & Lee (2016); Barro & Lee (2015) | 1950–2020 |
| `health_spending.csv` | [Healthcare expenditure per capita](https://ourworldindata.org/grapher/annual-healthcare-expenditure-per-capita) | [CSV](https://ourworldindata.org/grapher/annual-healthcare-expenditure-per-capita.csv) | World Bank / WHO GHED | 2000–2024 |
| `dtp3_immunization.csv` | [Child mortality vs DTP3 immunization](https://ourworldindata.org/grapher/child-mortality-vs-share-of-children-immunized-against-diphtheria-pertussis-and-tetanus) | [CSV](https://ourworldindata.org/grapher/child-mortality-vs-share-of-children-immunized-against-diphtheria-pertussis-and-tetanus.csv) | WHO/UNICEF WUENIC | 1980–2024 |
| `smoking.csv` | [Share of adults who smoke](https://ourworldindata.org/grapher/share-of-adults-who-smoke) | [CSV](https://ourworldindata.org/grapher/share-of-adults-who-smoke.csv) | WHO GHO | 2000–2024 |
| `obesity.csv` | [Share of adults defined as obese](https://ourworldindata.org/grapher/share-of-adults-defined-as-obese) | [CSV](https://ourworldindata.org/grapher/share-of-adults-defined-as-obese.csv) | WHO GHO | 1980–2024 |
| `air_pollution_deaths.csv` | [Death rate from ambient air pollution](https://ourworldindata.org/grapher/death-rate-ambient-air-pollution) | [CSV](https://ourworldindata.org/grapher/death-rate-ambient-air-pollution.csv) | IHME GBD (2019) | 2019 only |
| `water_access.csv` | [Improved water vs GDP](https://ourworldindata.org/grapher/improved-water-sources-vs-gdp-per-capita) | [CSV](https://ourworldindata.org/grapher/improved-water-sources-vs-gdp-per-capita.csv) | WHO/UNICEF JMP | 1990–2025 |
| `sanitation_access.csv` | [Stunting vs improved sanitation](https://ourworldindata.org/grapher/prevalence-of-stunting-vs-improved-sanitation-facilities) | [CSV](https://ourworldindata.org/grapher/prevalence-of-stunting-vs-improved-sanitation-facilities.csv) | WHO/UNICEF JMP | 1983–2024 |
| `population.csv` | [Population](https://ourworldindata.org/grapher/population) | [CSV](https://ourworldindata.org/grapher/population.csv) | UN WPP (2024); HYDE | 1950–2023 |
| `continents.csv` | [Continents according to OWID](https://ourworldindata.org/grapher/continents-according-to-our-world-in-data) | [CSV](https://ourworldindata.org/grapher/continents-according-to-our-world-in-data.csv) | Our World in Data | — |

A few practical notes:

- I downloaded my copies in **October 2026**. OWID updates these charts
  regularly, so a fresh download will differ in the last decimal places —
  that's fine, but the exact numbers in the notebooks may shift a little.
- Two of the files (`water_access.csv`, `sanitation_access.csv`) are scatter
  charts that happen to bundle a GDP / stunting column alongside the indicator
  I wanted. The wrangling notebook just picks the columns it needs and ignores
  the rest.
- `continents.csv` is the country-to-continent mapping; the first notebook
  also uses it as the "is this a real country" filter, because OWID's other
  files mix in regional aggregates ("World", "High-income countries", …).
- All data is licensed **CC BY 4.0** — free to use with attribution. Every
  figure in this repo credits OWID and the primary sources in its footer.
