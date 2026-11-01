# Data sources

All files in this folder were downloaded from **Eurostat**, the statistical office of the European Union, through its API, by the code in `final_project.qmd` (Section 2). The filters are part of each download link, so the download can be repeated. The files are kept exactly as downloaded and are never edited.

**Licence:** Eurostat data may be re-used free of charge, provided the source is acknowledged. The European Commission's reuse policy (Commission Decision 2011/833/EU) applies the Creative Commons Attribution 4.0 licence (CC BY 4.0). Copyright notice: https://ec.europa.eu/eurostat/about-us/policies/copyright

**How to cite:** "Source: Eurostat, [table code], accessed [download date]."

---

## demo_mlexpec.csv

| | |
|---|---|
| **Table** | `demo_mlexpec`, Life expectancy by age and sex |
| **Table page** | https://ec.europa.eu/eurostat/databrowser/view/demo_mlexpec/default/table |
| **Download link** | https://ec.europa.eu/eurostat/api/dissemination/sdmx/2.1/data/demo_mlexpec/A.YR.F+M.Y_LT1.?startPeriod=2015&endPeriod=2024&format=SDMX-CSV |
| **Filters in the link** | Annual, in years, women and men, at birth (`Y_LT1`), all countries, 2015 to 2024 |
| **Downloaded** | 29 October 2026 |
| **Last updated by Eurostat** | 25 September 2026 (column `LAST UPDATE` in the file) |
| **Size** | 906 rows, 11 columns |

## hlth_hlye.csv

| | |
|---|---|
| **Table** | `hlth_hlye`, Healthy life years and life expectancy at birth, by sex |
| **Table page** | https://ec.europa.eu/eurostat/databrowser/view/hlth_hlye/default/table |
| **Download link** | https://ec.europa.eu/eurostat/api/dissemination/sdmx/2.1/data/hlth_hlye/A.YR.F+M.HLY_Y0.?startPeriod=2015&endPeriod=2024&format=SDMX-CSV |
| **Filters in the link** | Annual, in years, women and men, healthy life years at birth (`HLY_Y0`), all countries, 2015 to 2024 |
| **Downloaded** | 30 October 2026 |
| **Last updated by Eurostat** | 17 July 2026 (column `LAST UPDATE` in the file) |
| **Size** | 618 rows, 11 columns |

## hlth_cd_asdr2.csv

| | |
|---|---|
| **Table** | `hlth_cd_asdr2`, Causes of death, standardised death rate by NUTS 2 region of residence |
| **Table page** | https://ec.europa.eu/eurostat/databrowser/view/hlth_cd_asdr2/default/table |
| **Download link** | https://ec.europa.eu/eurostat/api/dissemination/sdmx/2.1/data/hlth_cd_asdr2/A.RT.F+M.Y_LT65..AT+BE+BG+CY+CZ+DE+DK+EE+EL+ES+FI+FR+HR+HU+IE+IT+LT+LU+LV+MT+NL+PL+PT+RO+SE+SI+SK?startPeriod=2015&endPeriod=2024&format=SDMX-CSV |
| **Filters in the link** | Annual, rate per 100 000 (`RT`), women and men, under 65 (`Y_LT65`), all 93 causes, the 27 EU member states, 2015 to 2024 (data ends in 2023) |
| **Downloaded** | 1 November 2026 |
| **Last updated by Eurostat** | 8 June 2026 (column `LAST UPDATE` in the file) |
| **Size** | 43 902 rows, 12 columns |

## Still to download

| File | Table | Content |
|---|---|---|
| `tec00114.csv` | `tec00114` | GDP per capita in PPS, EU27 = 100 |
| `sdg_04_20.csv` | `sdg_04_20` | Tertiary education, age 25 to 34, by sex |

Each one gets its own section above when it is downloaded.
