# Decisions

Every choice and why, in my words.

## Step 1. Ask

- **Alcohol (29 Oct).** Alcohol is probably what sits behind many heart disease and accident deaths, so I want it in the data. I take it in two stages. First, alcohol-related causes of death (alcohol abuse, chronic liver disease) from the deaths table I already plan to download, `hlth_cd_asdr2`, so no new source. Second, only if time allows after the five tables are joined in Step 4: alcohol consumption per person from the WHO Global Health Observatory as a sixth table. Eurostat's own drinking survey (EHIS) only covers a few years, so I don't use it. Alcohol overlaps with heart disease and accidents, so in Exploration I check which factor still matters.

## Setup

- **All packages at the start (31 Oct).** Every package the project will need is loaded in the setup chunk, so I never have to come back and add one in the middle. I added `patchwork` now, before I need it, to put charts side by side, for example a weak and an improved version of the same chart in the talk.

## Step 2. Gathering data

- **Raw means raw (29 Oct).** The only selection is in the download link (sex, age, years), and anyone can read it there. After the download I change nothing in Section 2. Even `clean_names()` waits until Section 3, so the raw tables keep Eurostat's own column names, and every change to the data happens where I can explain it.

## Step 3. Cleaning

- **Flagged values are kept (30 Oct).** Some life expectancy values carry a letter, for example `e` (estimated), `p` (provisional) or `b` (break in the series). The letter says how the number was made, not that it is wrong, so I keep every flagged value. In the 27 EU countries only 52 of 540 values have a flag, all in Poland, Romania, Germany, France, Luxembourg, Czechia and Italy. Estonia, Latvia and Lithuania have none.
- **Technical columns are removed (30 Oct).** Six columns describe the download, not the data: `dataflow`, `last_update`, `freq`, `unit`, `age` and `conf_status`. The checks showed that `freq`, `unit` and `age` hold one value each (the choices I made in the link) and that `conf_status` is empty, so they tell me nothing new. Keeping them would also give the joined table in Section 4 many repeated columns with clashing names. The one useful fact, the `last_update` date, goes into the source line once instead of sitting in every row. I keep five columns: `country_code`, `sex`, `year`, `life_expectancy` and `flag`.
- **The flag column stays, for now (30 Oct).** If a value looks odd in Exploration, the flag tells me straight away whether it was estimated, provisional or a break in the series, without going back to the raw file. It's one small column, so it costs almost nothing now. I decide in Section 4 whether it goes into `gap_panel`, because that's where it starts to cost something: `pivot_wider()` would split it into one flag for women and one for men. Dropping it later is one line, `select(-flag)`. Getting it back after dropping it would mean redoing 3.1.
- **Luxembourg stays, with two years missing (31 Oct).** Luxembourg has no healthy life years for 2023 and 2024. I keep its 16 real values and leave the two years missing, nothing filled in. Dropping Luxembourg would throw away 8 good years to hide 2 missing ones, and cutting everyone to 2015 to 2022 would cost me the latest Baltic figures because of one small country. In charts for 2023 and 2024 Luxembourg simply has no healthy life years dot, and I name the gap in Limits.
- **Germany 2022 marked low reliability (31 Oct).** Germany's healthy life years for 2022 carry the flag `bu`: a break in the series and low reliability. I keep the values like all other flagged ones, but if Germany jumps in 2022 in Exploration, I'll check this flag first.
- **Charts and neat tables in the checks (30 Oct).** I show the check results as charts and tables instead of console text, so the report is easier to read. A `tbl_summary()` table for missing values, histograms split by sex for the range, and a bar chart of rows per country. Small tables go through `knitr::kable()`, which only changes how they look. The check charts use colour from the viridis family, the course's default palette, which colour-blind readers can also tell apart. Each chart has its own pair of colours, so one colour never means two things: blue and green for women and men, purple and orange for other units and EU countries. No red, because red is kept for the Baltic states in the whole project.
- **EU filter in cleaning, not in combining (30 Oct).** I remove the non-EU countries and the EU totals in Section 3, table by table, right after the checks. That way each clean table holds only what the analysis uses, and Section 4 can focus on reshaping and joining. The years don't need a filter at all, because the download links already ask for 2015 to 2024.
