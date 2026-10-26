# Limitations

No analysis is stronger than its data and its design. This document lists
everything I know to be wrong with, or missing from, this one — in rough
order of how likely it is to bite you.

## 1. Correlation is not causation (the big one)

**Every relationship in this project is descriptive.** Nothing here
identifies a causal effect, and several of the headline correlations are
outright confounded:

- Smoking correlates *positively* with life expectancy across countries
  (ρ = +0.32) and obesity *positively* as well (ρ = +0.47). Nobody believes
  cigarettes extend life; income does, and rich societies smoke and eat more.
  That is confounding on full display.
- GDP per capita correlates ρ = +0.88 with life expectancy, but income also
  buys schooling, nutrition, sanitation, health systems, and safer
  everything-else. The "+3.6 years per doubling of GDP" fit (socioeconomic_links.ipynb)
  describes how the world *looks*, not what a policy that raised GDP by X
  would do to health, nor vice versa — the reverse channel (healthier
  populations earn more) is well documented.
- The health-spending residuals invite "efficiency" readings they cannot
  support: a country below the spending trend may have an inefficient system,
  an HIV/AIDS or conflict burden, or simply a different epidemiological
  profile.

**What would be needed for causal claims:** designs that isolate variation —
difference-in-differences around policy changes, instrumental variables,
regression discontinuity, or randomised trials — plus controls for the
confounders visible here. That is a different project; this one deliberately
stops at description.

## 2. Vintage mismatch across indicators

The cross-sections mix data vintages: GDP ends in 2022, life expectancy and
health spending run to 2023–24, air pollution stops at 2019, schooling at
2020. The snapshot method (latest value per country in a trailing window,
anchored at each indicator's own frontier — see the `latest_snapshot` helper
in country_comparison.ipynb and socioeconomic_links.ipynb) keeps samples comparable but cannot make a 2019
pollution estimate contemporaneous with a 2023 outcome. Correlations involving
air pollution or schooling are the most affected.

## 3. Comparability of income and spending measures

GDP per capita comes from the Maddison Project in **2011 international
dollars**; health expenditure is in **current PPP international dollars**.
The two are not level-comparable (fine for ranks and separate regressions,
wrong for "spending as share of GDP"-style arithmetic). PPP conversions
themselves are estimates with revision cycles, and price levels for
health-specific goods differ from general price levels — a known distortion
in health-spending comparisons.

## 4. Missingness is not random

Countries that report late or poorly are disproportionately poor,
conflict-affected, or small — the same countries with the worst outcomes.
Consequences:

- Pre-1950 "global" statistics would really be European statistics; the panel
  deliberately starts in 1950 for this reason.
- The schooling series (146 countries) and smoking series (167) under-sample
  exactly the region whose relationship to outcomes we most want to
  understand.
- Life expectancy is *modelled* by UN WPP for countries with partial vital
  registration — an estimate with uncertainty intervals, not a measurement.
  The same goes for the modelled UN IGME child-mortality series.

## 5. Modelled estimates and their uncertainty

Air pollution mortality (IHME GBD) is a **modelled** attribution, not a count
of death certificates — it combines exposure models, concentration-response
functions from epidemiological studies, and cause-of-death models. The GBD
publishes uncertainty intervals that can be wide in low-data countries; none
of the figures here display them (single-value choropleths and scatters).
Treat national air-pollution rankings as indicative, not exact.

## 6. Ecological fallacy

All analysis here is at the country level. Country-level correlations say
nothing about individuals: "countries with higher GDP live longer" does not
mean the rich *within* those countries live longer (within-country
inequality in both income and mortality is large, especially in the US). Any
within-country question needs microdata.

## 7. Indicator limitations

- **Life expectancy at birth ≠ healthy life expectancy.** Gains in longevity
  can coincide with stagnant or falling healthy-life expectancy (more years
  with disease). HALE would tell a different, less rosy story in several rich
  countries.
- **DTP3 coverage** is a schedule-completion proxy with known recording gaps
  (denominator problems, campaign vs routine double-counting in some
  countries).
- **Smoking is age-standardised; obesity is crude** in the raw sources —
  comparing the two trend panels (socioeconomic_links.ipynb) mixes adjustment conventions.
- **Improved water/sanitation** (JMP "improved" categories) is a
  service-type indicator, not a water-quality measurement; "safely managed"
  would be stricter and lower everywhere.

## 8. Snapshot windows and rankings

The 5-year trailing window means a country's "latest" value can be up to four
years older than another's (GDP 2022 vs life expectancy 2023). The 500k
population threshold removes microstates from rankings — their volatile
statistics would otherwise dominate top/bottom lists — but it also removes
real populations: about 20 territories, collectively small. geographic_analysis.ipynb's
maps deliberately *don't* apply the filter, which is why Monaco tops the map
while Hong Kong tops the ranking table.

## 9. COVID-era distortion

The 2020–21 mortality shock distorts endpoint comparisons: "change since
1990" for countries whose latest value is a COVID year partly measures the
pandemic, not the trend. The world series (global_trends.ipynb) shows the dip and
rebound explicitly; the movers (country_comparison.ipynb) use the latest available value
whatever it contains.

## 10. Aggregates and historical entities

The panel excludes income groups and regional aggregates (by construction),
and includes historical entities (East Germany, Czechoslovakia, the USSR)
that simply stop having data. Continent classifications follow OWID's
mapping — Russia in Europe, Turkey in Asia — one defensible convention among
several. Population weighting (used for all regional averages) embeds its own
choice: India dominates Asia's line the way Tuvalu never dominates Oceania's.

---

### Reading list for the causal machinery this project lacks

For anyone who wants the next step: Angrist & Pischke, *Mostly Harmless
Econometrics* (identification strategies); the J-PAL / 3ie repositories for
randomised evaluations in health; and Deaton, *The Analysis of Household
Surveys*, for why cross-country regressions "explain" so little of what
policymakers need to know.
