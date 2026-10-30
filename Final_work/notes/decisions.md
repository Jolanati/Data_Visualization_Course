# Decisions

Every choice and why, in my words.

## Step 1. Ask

- **Alcohol (29 Oct).** Alcohol is probably what sits behind many heart disease and accident deaths, so I want it in the data. I take it in two stages. First, alcohol-related causes of death (alcohol abuse, chronic liver disease) from the deaths table I already plan to download, `hlth_cd_asdr2`, so no new source. Second, only if time allows after the five tables are joined in Step 4: alcohol consumption per person from the WHO Global Health Observatory as a sixth table. Eurostat's own drinking survey (EHIS) only covers a few years, so I don't use it. Alcohol overlaps with heart disease and accidents, so in Exploration I check which factor still matters.

## Step 2. Gathering data

- **Raw means raw (29 Oct).** The only selection is in the download link (sex, age, years), and anyone can read it there. After the download I change nothing in Section 2. Even `clean_names()` waits until Section 3, so the raw tables keep Eurostat's own column names, and every change to the data happens where I can explain it.

## Step 3. Cleaning

- **Flagged values are kept (30 Oct).** Some life expectancy values carry a letter, for example `e` (estimated), `p` (provisional) or `b` (break in the series). The letter says how the number was made, not that it is wrong, so I keep every flagged value. In the 27 EU countries only 52 of 540 values have a flag, all in Poland, Romania, Germany, France, Luxembourg, Czechia and Italy. Estonia, Latvia and Lithuania have none.
