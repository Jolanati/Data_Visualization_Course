# Project: Why do Baltic men die earlier?

Course: Data visualization and storytelling (SZF_184), RSU, autumn 2026. Lecturer: Sergio Uribe.
Student: Jolanta. Language of the project: English.
Tools: Positron, R, Quarto, Claude Code. Deliverables: the whole analysis written as one Quarto document (text that explains, then code), plus an interactive data app in plain HTML, CSS and JavaScript hosted on Vercel.

Put this file in the project root. Start every Claude Code session with: "Read Project_idea.md first, then continue from the Progress section."

**Status:** Sergio has not sent the requirements for the final work yet. Until he does, we work like a data analyst, step by step: find the data, clean it, explore it, manipulate it, then tell the story. The course repo's capstone files (template, rubric, dates) are not the spec. Use them only as loose hints about what the course values. When the requirements arrive, we adjust.

**Unofficial hints (classmates' group chat, 29 Oct, secondhand, confirm with Sergio):** use my own data. Make a "wow" moment in the presentation. Use everything learned in the classes. Show the code in the presentation and tell how I did it, as a story. Sergio said he will come back to this in a later class.

**First session, in this order:**

1. Create the folders from section 4 and an empty `.here` file.
2. Create `analysis.qmd` with the header and setup chunk from section 4.
3. I write the question and the expected message in section 1 of `analysis.qmd`, before any code runs on the data.
4. Download the five Eurostat tables into `data-raw/`, record the filters in `notes/decisions.md`, and import each one with `glimpse()`.

## Progress (Claude Code updates this at the end of every session)

- [x] Step 1. Ask: question locked, expected message written, country-trait guess written
- [ ] Step 2. Get the data: five files in `data-raw/`, filters recorded, each one imports
- [ ] Step 3. Clean: five checks done for each table, notes written
- [ ] Step 4. Combine: `data/gap_panel.csv` saved, row counts explained
- [ ] Step 5. Explore: histograms, scatter plots, correlations, dead ends noted
- [ ] Step 6. Story graph: final plot exported
- [ ] Step 7. Report and app: `analysis.qmd` renders, app is live
- [ ] Step 8. Rehearse the 10-minute talk

Decisions so far:

- Work mode: guided build, one step at a time, with a step gate (see section 1).
- Time is included: 2015 to 2024, one row per country and year.
- Story style: Rosling-like. One chart built up in frames, each with one spoken sentence (see Step 6).
- Question (locked): "Why do men in the Baltic states die so much earlier than women, compared with the rest of the EU?", with three sub-questions in `analysis.qmd`.
- Expected message (30 Oct): "Baltic men die earlier, and this goes mainly with heart disease deaths before 65." Written after the life expectancy table was loaded, but before the other four tables.
- Data direction: why Baltic men die earlier. Life expectancy by sex, healthy life years, deaths by cause, GDP per capita, education by sex (all Eurostat). The AI topic was dropped.
- Tables: five, codes in section 3, filters still to confirm on download.
- Alcohol (29 Oct): option C. Alcohol-related causes of death from `hlth_cd_asdr2` now. WHO alcohol consumption per person as a sixth table only after Step 4 is done, if time allows.

---

## 1. How Claude Code should work with me

Claude Code writes the code. My job is to understand it well enough to explain every line on presentation day, and to choose every analysis decision myself. So Claude Code acts as a **guide**: it builds step by step and teaches as it goes.

Mode: **guided build, one small step at a time.** For every step:

1. **Before the code.** Say in plain words what we are about to do, why, and which class topic it connects to.
2. **Write the code in small pieces.** A few lines at a time, never a whole script at once, with a short comment on each line or group of lines.
3. **After the code.** Explain each function used: what it does, why this one and not an alternative, and what its arguments mean. Describe what the output should look like.
4. **Check my understanding.** Ask me one question (for example: what will this print? why `left_join()` and not `inner_join()`?). Wait for my answer before moving on. Correct me kindly.
5. **Record the reason.** Add one or two sentences to `notes/decisions.md`, in my voice, so I can reuse them in the talk.

Rules:

- Run and test each piece with me before the next one. If something errors, explain the error before fixing it.
- Prefer functions the course has taught: `glimpse()`, `summary()`, `filter()`, `select()`, `mutate()`, `arrange()`, `distinct()`, `rename()`, `case_when()`, `pivot_longer()`, `pivot_wider()`, the joins (`left_join()`, `inner_join()`, `full_join()`, `anti_join()` with `join_by()`), `group_by()` with `summarize()`, `count()`, `ggplot2` (`geom_point()`, `geom_histogram()`, `geom_smooth(method = "lm")`, `facet_wrap()`, `labs()`, `theme_minimal()`, `scale_color_manual()`, `annotate()`, `ggsave()`), `ggrepel::geom_text_repel()`, `janitor::clean_names()`, `gtsummary::tbl_summary()`. If something outside the list is better, name it, explain it in two sentences, and let me decide.
- **Known new functions** (not taught in the course, so explain each one fully when we reach it): `cor()` for correlations in Step 5, and `parse_number()` only if Eurostat flags turn a number column into text in Step 3.
- Never make an analysis decision silently (which year, which filter, what to do with an outlier, which trait wins). Give options with a recommendation, and I choose.
- Never edit files in `data-raw/`. Never replace `NA` with 0.
- After each code piece, draft the explaining paragraph for `analysis.qmd`, short and plain. I edit it until I'm happy with it.
- **Academic voice in `analysis.qmd`:** no "I", "my" or "me". Use impersonal or passive phrasing ("the data is filtered in Section 3", "the analysis checks...") and no contractions. The only exception is the personal sentence in Background ("I live in Latvia..."). `notes/decisions.md` stays in my own voice, because it is for the talk.
- Keep a running glossary in `notes/glossary.md`: each new function with a one-line meaning and a tiny example, so I can revise before the talk.
- At the end of each workflow step, give me a "can I explain this?" summary in three bullet points.
- **Step gate:** never start the next workflow step until I confirm I can explain the current one. One step at a time, even if going faster would be possible.
- At the start of a session, read the Progress section and continue from the first unchecked item. At the end, tick what is done and update the decisions list.
- If I say "explain it simpler", use plain words or an everyday analogy. If I say "skip the question", skip it.
- **Data app code** (HTML, CSS, JavaScript) follows the same approach, but lighter: explain the structure and the key parts, not every line. It is not course content.

Code style:

- Packages: `pacman::p_load(tidyverse, here, janitor)` at the top of every script.
- Paths: always `here("data-raw", "file.csv")`. No absolute paths.
- Native pipe `|>`, not `%>%`.
- Object names: the full dataset name, then what the object holds, no short prefixes: `life_expectancy_url`, `life_expectancy_file`, `life_expectancy_raw`, `life_expectancy_clean`. Others: `healthy_life_years_`, `deaths_`, `gdp_`, `education_`. Chunk labels follow the same names, for example `show-life-expectancy-load`.
- One comment per data quality check, written as a sentence.

Writing style for any text Claude drafts for me (app text, titles, captions, notes):

- No long dashes. Use commas, colons or full stops.
- Plain, direct sentences of mixed length. Contractions are fine.
- No filler openers, no buzzwords, no "in today's world" phrasing.
- Titles state a finding, not a topic.

---

## 2. The question

**Main question:** Which country-level factors (causes of death, income, education) go with the gap in life expectancy between women and men in EU countries, how has that gap changed between 2015 and 2024, and where do the Baltic states sit?

- Number: life expectancy at birth, women minus men, in years (the "gap").
- Group: EU member states (about 27 countries).
- Period: 2015 to 2024, or the latest year each table has. Time is part of the project from the start.
- Highlighted group: Estonia (EE), Latvia (LV), Lithuania (LT).

**Wording rule:** say "associated with" and "goes with". Never say "causes". With about 27 countries the data cannot show cause.

**Angles to compare in Exploration** (the story uses the one with the clearest message):

1. The Rosling chart: life expectancy against GDP per capita, women and men side by side. Do Baltic women fit the EU pattern while Baltic men fall below it?
2. Causes of death: which cause (circulatory diseases, cancer, external causes such as accidents and suicide) differs most between men and women under 65 in the Baltics?
3. Healthy life years: how many years of good health do men have compared with women?
4. Education: does the gap in tertiary education between women and men line up with the life expectancy gap?
5. The Baltics against the rest of the EU.
6. Change over time: did the gap narrow between 2015 and 2024, and which countries gained most?

**Expected message (fill in before any code, then keep it unchanged):**

> [One sentence. Write it myself before any code runs on the data. Examples of the shape: "The life expectancy gap is biggest where men die young from heart disease." or "Income does not explain the Baltic gap, men's deaths do."]

Starting hypotheses, not results:

1. A bigger gap goes with higher male death rates under 65 from circulatory diseases.
2. External causes (accidents, suicide) also go with a bigger gap.
3. Poorer countries have lower life expectancy for both sexes, but men fall further below the pattern in the Baltics.
4. Countries where women are much more educated than men have bigger gaps.
5. The gap has narrowed in most countries since 2015, but not equally. The Baltic states may be catching up at different speeds.
6. These factors overlap. Check which one still matters, and which are stand-ins for another (for example wealth).
7. Early hints from press coverage of Eurostat data, to check: Latvia has the widest gap in the EU (9.8 years in 2024), then Lithuania (8.6) and Estonia (8.5). Baltic men have the lowest healthy life years in the EU. Lithuania had the highest male death rate from ischaemic heart disease in 2019. Women are much more educated than men in Latvia and Estonia. An older report listed the gaps as Lithuania 10.7, Latvia 9.8 and Estonia 8.9 years (year to check), so Lithuania's gap may have narrowed the most.

---

## 3. Data

Five Eurostat tables, joined on country code and year. Download each one from R through the Eurostat API (an SDMX-CSV link built with `paste0()`, saved with `download.file()` only if the file doesn't exist yet) into `data-raw/`. The filters live in the link, so the download is reproducible.

| Short name | Eurostat code | Variable I want | Year target | Status |
|---|---|---|---|---|
| Life expectancy | `demo_mlexpec` | Life expectancy at birth by sex (F and M), in years. The table has many age classes, so choose the one for birth (code to confirm, probably `Y_LT1`) | 2015 to 2024 | to confirm on download |
| Healthy life years | `hlth_hlye` | Healthy life years at birth, women and men | 2015 to 2023 or 2024 | to confirm on download |
| Deaths by cause | `hlth_cd_asdr2` | Standardised death rate per 100 000 by cause (circulatory diseases, cancer, external causes, and the alcohol-related causes: alcohol abuse and chronic liver disease, labels to confirm) and sex, for people under 65 | 2015 to the latest year, maybe 2022 or 2023 | to confirm on download |
| GDP per capita | `tec00114` | GDP per capita in PPS, index with EU27 = 100 | 2015 to 2024 | to confirm on download |
| Education by sex | `sdg_04_20` | % of people aged 25 to 34 with tertiary education, women and men | 2015 to 2024 | to confirm on download |

Fallback for GDP if `tec00114` gives trouble: `nama_10_pc`, unit `CP_PPS_EU27_2020_HAB`, indicator `B1GQ` (GDP at market prices per capita, in PPS).

**GDP over time:** `tec00114` is built for comparing countries within a year. Over the years it shows catching up or falling behind relative to the EU average, not real growth. Say so in the story. If I want real growth, switch to `nama_10_pc`, unit `CLV10_EUR_HAB` (chain linked volumes, euro per capita).

**Alcohol, stage 2 (optional, only after Step 4):** total alcohol consumption per person aged 15+, litres of pure alcohol per year, from the WHO Global Health Observatory. It uses three-letter country codes (`LVA`, not `LV`), so it needs a code lookup before the join, and its latest year may be older than 2024. Check on download. Eurostat's drinking survey (EHIS) only has a few waves, so it is not used.

Tables I know exist, if an angle needs them later: life expectancy by education level (`demo_mlexpecedu`, covers only about 19 countries), median age (`demo_pjanind`). Smoking data come from the same separate health survey (EHIS), and I have not checked those tables.

Rules:

- Keep 2015 to 2024 as the window unless a table forces a shorter one. Write down each table's years in `notes/decisions.md`.
- Write down the exact filter labels I choose on the Eurostat site (indicator, sex, age class, cause of death, unit) in `notes/decisions.md`. A reader must be able to reproduce the download.
- Life expectancy is a snapshot: it assumes today's death rates last for a whole life. State this in the app.
- The tables may have different latest years (death rates are often a year or two behind). Write the year of each table in the final graph's source line.
- Name the source on every chart: "Source: Eurostat, [codes], accessed [date]".

Things to expect in cleaning:

- Different country coverage per table (some include Norway and candidate countries).
- 2020 and 2021 show a COVID dip in life expectancy. It is real data, not an error. Say so in the story.
- The life expectancy table has many age classes, and the death rate table has many causes and age classes. Filter to one at a time and check the row count each time.
- Aggregate rows such as an EU total. Remove them before joining.
- Country codes may not match across tables (Eurostat uses its own codes, for example for Greece). Check with `anti_join()`.
- Eurostat marks missing values with `:`. Declare it when reading the file, as class 4 does: `read_csv(na = c("", ":", "NA"), show_col_types = FALSE)`. Report how many are missing, do not replace them.
- Eurostat can add flags such as `p` (provisional) or `e` (estimated) to some numbers. That can turn a number column into text. Check every column type with `glimpse()`.

---

## 4. Folder structure

```
baltic-men-story/
├── Project_idea.md
├── .here                  # empty file so here() finds the project root
├── analysis.qmd           # the whole project: text that explains, then code
├── R/
│   └── 99_export_for_app.R    # writes the JSON and image the app reads
├── data-raw/              # untouched Eurostat downloads
├── data/                  # clean tables I create
├── figures/               # exported PNGs, 300 dpi
├── app/                   # the Vercel data app (static files only)
│   ├── index.html
│   ├── style.css
│   ├── app.js
│   ├── data/              # JSON exported from R
│   └── img/               # story frames from R
└── notes/
    ├── decisions.md       # every choice and why
    └── glossary.md        # each new function, one line each
```

Two choices here are not taught in the course, so be ready to explain them: `data-raw/` keeps the downloads untouched while `data/` holds my clean tables, and the empty `.here` file lets `here()` find the project root. I save the final graph to `figures/` and copy it to `app/img/`.

### The Quarto document

All the main work lives in one file, `analysis.qmd`. For each step I write text that explains what I do and why, then the code, then one sentence on what it shows. The same file renders to one self-contained HTML report, which is my safe submission.

Header of `analysis.qmd`:

```yaml
---
title: "Why do Baltic men die earlier?"
author: "Jolanta"
format:
  html:
    code-fold: true
    code-summary: "Show the code"
    toc: true
    embed-resources: true
---
```

One heading per step:

```
# Project overview      (no number: background, aim, research question, assumptions)
# 1. Environment setup  (1.1 packages, 1.2 project folder)
# 2. Gathering data     (opening text, then 2.1 to 2.5, one per dataset, each with x.1 build the link, x.2 download once, x.3 load and look)
# 3. Cleaning
# 4. Combining the tables
# 5. Exploration
# 6. The story
# 7. Limits
```

Pattern inside every section:

1. **Rationale:** two to five sentences, academic impersonal voice, plain words.
2. **Code:** one or a few short chunks (about 15 lines or less each).
3. **What it shows:** one sentence on the result.

Rules:

- A setup chunk at the top loads packages with `pacman::p_load(tidyverse, here, janitor)`.
- Never put code without explaining text before it.
- Claude Code drafts the explaining text after each code piece. I edit it until it sounds like me.
- Chunks the app should display get a label that starts with `show-`:

````markdown
```{r}
#| label: show-join
gap_panel <- life_exp |> left_join(gdp, by = join_by(country_code))
```
````

Render with `quarto render analysis.qmd`.

When Sergio's requirements arrive, the headings can be renamed or regrouped without changing the code.

---

## 5. Workflow: eight steps

Each step has a task and a "done when", and points to the class that taught the tools.

### Step 1. Ask
- **Task:** write the question and the one-sentence expected message in section 1 of `analysis.qmd`. Also write which factor I expect to matter most for the gap (heart disease deaths, accidents and suicide, income, or education), and why. No code in this step.
- **Done when:** both are written, before any code runs on the data. If the course asks for the question to be registered somewhere, do that too (check with Sergio).

### Step 2. Get the data
- **Task:** download the five CSVs into `data-raw/` through the Eurostat API from R. Each dataset in `analysis.qmd` follows the same three steps: build the link, download once, load and look. Structure of each section follows my wine-dine notebook: section intro with a table, then sub-sections with description first, then small step-by-step code chunks. Record filters in `notes/decisions.md`. Import each file with `read_csv(na = c("", ":", "NA"), show_col_types = FALSE)` and check the column types with `glimpse()`.
- **Done when:** each file imports with `read_csv()` and checks with `glimpse()` and `head()` without errors. No `clean_names()` here: it is the first step of Step 3, so the raw tables keep Eurostat's own names.

### Step 3. Clean (class 3 data quality checklist, classes 4 and 6)
- **Task:** for each table run the five checks and write one comment line per check. `gtsummary::tbl_summary()` suits the quality table, the course uses it a lot. If Eurostat flags turned a number column into text, fix it (this is where `parse_number()` would come in, explained first).

| # | Check | How |
|---|---|---|
| 1 | Types | `glimpse()` |
| 2 | Missing values | `summary()` |
| 3 | Units and ranges | `summary()`, histogram |
| 4 | Duplicates | `distinct()` and `count()`, compare |
| 5 | Row counts | `count()` before and after every filter |

- **Done when:** each table has a short "what I found, what I decided" note, and one table is shown in the app as a quality report.

### Step 4. Manipulate (classes 5, 7, 8, 9)
- **Task:** keep EU member states and the years 2015 to 2024, reshape (`pivot_wider()` puts women and men side by side, `pivot_longer()` does the opposite), rename to short column names, compute the gaps with `mutate()`, and join the five tables on country code and year with `join_by(country_code, year)`. After each join, check row counts (about 27 countries times 10 years, so about 270 rows) and run `anti_join()` to list country-years that did not match. Save the result to `data/gap_panel.csv`.
- **Core columns first:** `country_code`, `country`, `year`, `le_women`, `le_men`, `le_gap` (women minus men), `hly_women`, `hly_men`, `gdp_pps_index`, `baltic` (TRUE for EE, LV, LT). Add the rest when we reach the angles in Step 5: `death_circ_men_lt65`, `death_circ_women_lt65`, `death_cancer_men_lt65`, `death_cancer_women_lt65`, `death_ext_men_lt65`, `death_ext_women_lt65`, `death_alc_men_lt65`, `death_alc_women_lt65`, `tertiary_women_pct`, `tertiary_men_pct`.
- **Done when:** one row per country and year, no duplicates, missing values documented, row counts explained.

**Possible course checkpoint (from the course README, confirm that it applies): Fri 13 Nov 2026, 23:59.** Five lines that load the clean data and run without errors:

```r
pacman::p_load(tidyverse, here)
here("data", "gap_panel.csv") |>
  read_csv(show_col_types = FALSE) |>
  glimpse() |>
  nrow()
```

### Step 5. Explore (class 3 chart by type, class 10 chart by question)
- **Task:** one histogram per variable. One scatter plot of `le_gap` against each candidate explanation (male death rate from each cause, GDP, the tertiary education gap between women and men), same y axis, using `facet_wrap()`. Add `geom_smooth(method = "lm")`. Report the correlation for each pair with `cor()` (new function, explain it first, including what the number means and how missing values are handled). If I decide against `cor()`, describe the pattern in words and drop `correlations.json`. Check each outlier: data error or real value? Write one sentence per outlier.
- **Keep a "dead ends" list** in `notes/decisions.md`: relationships I expected that were weak, and what I did about it.
- **Look at the overlap on purpose.** If every explanation also goes with GDP, say that they may be stand-ins for wealth.
- **Time.** Draw the gap over the years for every country as lines (class 3: time means a line chart), with the Baltics in red. Use `group_by(country_code)` and `summarize()` to get each country's change from 2015 to 2024. Which countries narrowed the gap most? Which did not? Remember the 2020 and 2021 dip.
- **Group comparison.** Compare the Baltic average with the rest of the EU using `group_by(baltic)` and `summarize()`, and show it with a box plot or bars (classes 9 and 10).
- **The Rosling chart.** Use `pivot_longer()` to put women and men in one column called `sex`, then plot life expectancy against GDP with one dot per country and sex. Do Baltic men sit far below the pattern while Baltic women do not? Try this angle first.
- **Done when:** I can name the strongest explanation, the weakest, one surprise, which angle has the clearest message, and whether GDP explains most of the pattern.

### Step 6. Tell the story (Show, Cut, Focus, Tell; classes 11, 12, 13)
- **Task:** build one scatter plot.
  - Highlight the Baltics the way class 11 does, using the `baltic` column: `scale_color_manual(values = c("TRUE" = "firebrick", "FALSE" = "gray70"))`, `theme_minimal(base_size = 14)`, and `theme(legend.position = "none", panel.grid.minor = element_blank())`. Labels with `ggrepel::geom_text_repel()`, the note with `annotate("text", ...)` (class 12). Viridis stays for charts with several groups.
  - Title states the finding. Example shape (a shape only, not a result): "[Group of countries] have the widest gap between women and men in life expectancy, [by how much]".
  - One annotation pointing at the evidence (label the three Baltic points, or one outlier).
  - Source line, axis labels with units, no legend or grid line without a job, no border.
  - Honest scales: label the axes clearly, say if an axis does not start at zero.
- **Checklist before export:**
  - [ ] Ask: the title answers the question.
  - [ ] Show: right chart for two numbers, honest scales, data quality applied.
  - [ ] Cut: no non-data ink.
  - [ ] Focus: gray plus one color.
  - [ ] Tell: finding title, one annotation, source.
- **Build the story in frames (Rosling style).** One chart, shown in five frames, preceded by an opening question. Each frame adds one layer and gets one spoken sentence. Axes, scales and point size are identical in every frame, so nothing jumps. Plan the frames before coding:

| Frame | What appears | What I say (my words, one sentence) | Layer in R |
|---|---|---|---|
| 0 | A question to the audience, no chart yet (for example: which EU country has the biggest gap between how long women and men live?) | [write it] | none |
| 1 | Women's and men's life expectancy against GDP in 2024, two dots per country, all in gray | [what the axes mean] | `geom_point()` in `gray70`, a different shape for men |
| 2 | Each country's two dots joined by a line, the gap | [what the line length means] | `geom_line(aes(group = country_code))` |
| 3 | The Baltics in red, with labels | [where the Baltics sit] | `scale_color_manual()`, `ggrepel::geom_text_repel()` |
| 4 | Time: arrows from the 2015 position to the 2024 position for the Baltics | [how far each country moved] | `geom_segment()` with an arrow |
| 5 | One annotation, the finding as the title, plus the source | [the finding in one sentence] | `annotate("text", ...)`, `labs(title = ..., caption = ...)` |

  This is a starting idea for the frames. Decide the final frames in Step 6, after Exploration shows which angle is clearest.

  Rules for the frames: say what to look at before it appears. One idea per frame. Plain words. The last frame states the finding. Build the frames by saving the plot after each added layer (for example `p1`, then `p2 <- p1 + ...`), so frame 5 is the final graph.
- **Export:** each frame with `ggsave(here("figures", "story_frame_1.png"), width = 8, height = 5.5, dpi = 300)`, and so on up to frame 5. Frame 5 is also copied as `story.png`.

### Step 7. Build the report and the data app
- **Report (safe submission):** `analysis.qmd` rendered to one self-contained HTML file with `quarto render analysis.qmd`. This is the version that must run from top to bottom.
- **Data app:** follow section 6. R exports the data, the app displays it.
- **Done when:** the report renders with no errors, the app is live on a Vercel link, and the numbers in the app match the report.
- **Ask Sergio** whether a web app is accepted as the deliverable, and in which form he wants the script.

### Step 8. Polish and rehearse
- Run the quality checklist (section 8).
- Write the 10-minute talk (section 7).
- Rehearse twice with a timer. Prepare an offline backup (see the rules in section 7).

---

## 6. The Vercel data app

**What it is:** a small interactive web app in plain HTML, CSS and JavaScript, hosted on Vercel. It walks the reader through seven sections that match `analysis.qmd`: Question, Data, Cleaning, Combining the tables, Exploration, The story, Limits. Each section shows the logic, the result, and an info button that reveals the R code that produced it. R does all the analysis. The app only displays what R exported.

**Why this split:**

- The analysis stays in R, with course tools, and runs top to bottom.
- The code shown in the app is extracted from my real `analysis.qmd` file, so it cannot drift from what actually ran.
- No framework, no build step, no npm packages. Fewer things to break before the deadline.
- The story frames are ggplot2 images exported from R, so the course's Cut, Focus and Tell principles are applied with the course's own tool, and the app needs no JavaScript charting for the story.

### Data flow

```
analysis.qmd  ->  R/99_export_for_app.R  ->  app/data/*.json + app/img/story_frame_*.png
                                                              |
                                          app/index.html reads the JSON with fetch()
                                                              |
                                                           Vercel
```

| File written by R | Content | Used in section |
|---|---|---|
| `app/data/gap_panel.json` | Clean table, one object per country and year | Exploration, The story |
| `app/data/quality.json` | Per table: rows, columns, missing values, duplicates, range of the key variable, my decision | Cleaning |
| `app/data/joins.json` | Row counts before and after each join, countries listed by `anti_join()` | Combining the tables |
| `app/data/correlations.json` | Correlation of the life expectancy gap with each candidate explanation, plus n | Exploration |
| `app/data/code.json` | R code per step, extracted from `analysis.qmd` | All |
| `app/data/steps.json` | Two to four sentences per step on the logic, shortened from the "What I do and why" text in `analysis.qmd` | All |
| `app/img/story_frame_1.png` to `story_frame_5.png`, and `story.png` | The story frames from R, 300 dpi. `story.png` is the last frame | The story |

**Code extraction.** Chunks in `analysis.qmd` whose label starts with `show-` are the ones the app displays (see the example in section 4). `99_export_for_app.R` reads `analysis.qmd`, collects those chunks by label, and writes `code.json`. It uses `jsonlite`, which the course has not covered, so Claude Code should explain that script line by line.

### Page layout

One page that scrolls, with a sticky navigation for the seven sections. Each section has four parts:

1. A heading that says what happened, not only the topic.
2. The logic: two to four sentences on why I did it.
3. The result: a table or a chart.
4. An info button ("Show the R code") that opens the code block from `code.json`, with a copy button.

### Interactions (keep to this list)

- **Cleaning:** a table of the five checks per dataset. Click a dataset to see my notes.
- **Combining the tables:** row counts before and after each join, and the list of unmatched countries.
- **Exploration:** a dropdown to switch the explanation on the x axis. Scatter plot with a hover tooltip (country, both values). A toggle to highlight the Baltics. The correlation and n for the selected explanation.
- **The story:** the five frames of one chart with next and back buttons, and one caption per frame (my spoken sentence). The opening question comes first. No other controls. The last frame is the final graph. This is the part where the course's Cut and Focus principles show.

### Design rules

- Same colors as the ggplot: `gray70` for all countries, `firebrick` for the Baltics (the course's highlight color). Use the same two colors in the CSS.
- Large text and high contrast, readable on a projector. Laptop screen first, mobile only if time remains.
- Titles state a finding. A source line under every chart.
- Every number in the app comes from the JSON files. Never type a result by hand.
- One charting library from a CDN (Plotly.js or Observable Plot). Pick one on day one and stay with it.

### Technical notes

- `fetch()` does not work on a page opened as a `file://` link. Test locally with a small server inside `app/`: `python -m http.server 8000`, or `npx serve app`.
- Commit the JSON files and the image. Vercel does not run R.
- Deploy in one of two ways: install the CLI (`npm i -g vercel`) and run `vercel deploy app --prod`, or push to GitHub, import the repo in Vercel, set Root Directory to `app`, framework preset "Other", and leave the build command empty.
- Deploy a dummy version with fake data by Mon 9 Nov. Then swap in real data and redeploy.
- Check the live link on another device and on a phone hotspot.

### Build order

Each piece works before the next starts.

1. Skeleton page with seven empty sections and navigation. Deploy it.
2. Load `gap_panel.json` (dummy data first) and draw the scatter plot.
3. Add the info button with code from `code.json`.
4. Add the explanation dropdown and the Baltic highlight.
5. Add the quality and joins tables.
6. Add the story frames with next and back buttons and captions, then polish.

### Fallback

If the app does not work end to end by **Thu 12 Nov**, the Quarto report becomes the main deliverable, and the app is shown only if it is ready. The R analysis is the core. The app is the way it is delivered.

---

## 7. The 10-minute presentation

I show three things live: the **data**, the **code**, and the **app**.

| Time | Part | What I show | Where |
|---|---|---|---|
| 0:00 to 0:45 | Ask | The question, the expected message, why it matters | App, Question section |
| 0:45 to 1:45 | Data | The five sources, one raw CSV, what each variable means | Positron data viewer, then app |
| 1:45 to 3:15 | Clean | The five checks, one real problem I found, my decision | App, Cleaning section, quality table. Open the info button once |
| 3:15 to 4:30 | Manipulate | The joins, the `anti_join()` result, row counts before and after | App, then the pipeline code |
| 4:30 to 6:00 | Explore | Switch the explanation live with the dropdown, the gap over time, one dead end, why I picked the winner | App, Exploration section |
| 6:00 to 8:00 | Story | Open with the guess question, then step through the five frames, one sentence each. End on the finding | App, The story section |
| 8:00 to 9:00 | Limits | Association not cause, about 27 countries, life expectancy is a snapshot of today's death rates, countries differ in age structure | App, Limits section |
| 9:00 to 10:00 | Wrap | What the method shows, what I would do next, questions | App |

Rules for the talk:

- Open the info button at most twice, on the two moments where the logic matters most (joins and the final graph).
- Keep `analysis.qmd` open in Positron next to the app, to show the text-then-code structure.
- Live interaction only in the Explore part. In the Story part I step through the frames of one chart, Rosling style.
- Say the finding in one sentence at the start of the Story part and again at the end.
- Optional: one light joke to break the ice, friendly and never at anyone's expense. Draft: "Estonia added seven years of life expectancy in twenty years. For a country that runs its government like software, that is just a good update cycle." Use it only if it feels natural.
- Offline backup: the app running from a local server on the laptop, the self-contained HTML report, the PNG of the final graph, and screenshots of each step. Wi-Fi fails at the worst time.

Confirm the required talk length with Sergio when the requirements arrive.

---

## 8. Quality checklist (provisional)

Built from the course's Ask, Show, Cut, Focus, Tell principles. This is not official grading. Replace it when Sergio's requirements arrive.

| Principle | What I show |
|---|---|
| Ask | Clear question, data can answer it, one-sentence message |
| Show | Right chart type, honest scales, tidy data, data quality checks reported, the project runs |
| Cut | No non-data ink, no legend, border or grid line without a job |
| Focus | Gray plus one color, the eye goes to the message |
| Tell | Finding in the title, one annotation, a clear beginning, middle and end |

Course coverage check, so the project shows what the classes taught. Built from the course repo's `classes/` folders and my own lecture folders (checked 29 Oct). Every row must be used at least once, on purpose, with a sentence in `analysis.qmd` saying why.

| Class | Tools and ideas | Where they show up in the project |
|---|---|---|
| 01 Why storytelling | Bad graphs, the five principles (Ask, Show, Cut, Focus, Tell), baseline and axis tricks with `coord_cartesian()`, why pie charts mislead | Step 6 checklist. In the talk: one "bad" version of my chart next to the good one |
| 02 First graph | `pacman::p_load()`, `read_csv()`, `glimpse()`, `ggplot()`, `geom_point()`, `labs()`, `ggsave()` | Setup chunk, Step 2 import, every scatter plot, exporting the story frames |
| 03 Know your data | `summary()`, `count()`, `distinct()`, `arrange()`, chart by data type, outlier check, the data quality checklist (types, missing, ranges, duplicates, row counts) | Step 3: the five checks for every table. Step 5: one sentence per outlier |
| 04 Row and column verbs | `filter()`, `select()`, `mutate()`, `arrange()`, `rename()`, `case_when()`, `is.na()`, `gtsummary::tbl_summary()`, `janitor::clean_names()` | Step 3: quality table with `tbl_summary()`. Step 4: EU countries, years, short names, `case_when()` for the `baltic` column |
| 05 Tidy data | Tidy data rules, `pivot_longer()` (wide to long) | Step 4: check every Eurostat table is tidy. Step 5: `pivot_longer()` to one `sex` column for the Rosling chart |
| 06 dplyr recap | The verbs together in one pipe, gapminder, `geom_line()` | Step 4: the cleaning pipeline. Step 5: the gap over time, one line per country |
| 07 Pivot | `pivot_wider()`, `pivot_longer()` | Step 4: `pivot_wider()` puts women and men side by side so I can compute the gap |
| 08 Joins | `left_join()`, `inner_join()`, `full_join()`, `anti_join()`, `join_by()` | Step 4: join the tables on `join_by(country_code, year)`, `anti_join()` to list what didn't match, row counts after each join |
| 09 Split, apply, combine | `group_by()`, `summarize()`, `count()`, `arrange()` | Step 5: Baltic average against the rest, change from 2015 to 2024 per country, ranking countries by gap |
| 10_1 Distribution | `geom_histogram()`, `geom_density()` | Step 3: range check. Step 5: one histogram per variable, density of the gap for women and men |
| 10_2 Counts | `geom_bar()`, `geom_col()`, `fct_infreq()`, `fct_reorder()`, `fct_rev()`, `fct_other()`, `coord_flip()`, `slice_max()`, treemap, pie | Step 3: rows per country with `geom_bar()`, to spot missing countries. Step 5: ranked bar chart of the gap with `geom_col()`, `fct_reorder()` and `coord_flip()`. `slice_max()` for the top countries |
| 10_3 Scatter | `geom_point()`, `geom_smooth(method = "lm")`, `facet_wrap()` | Step 5: the gap against each explanation, one panel per explanation, with a trend line. The Rosling chart |
| 10_4 Differences | `geom_boxplot()`, `geom_violin()`, `geom_jitter()`, `stat_summary()` with `mean_se` error bars | Step 5: Baltics against the rest of the EU, box plot with jittered points, means with error bars |
| Later classes (not built yet) | Themes, highlight color, `ggrepel`, `annotate()` | Step 6: the story frames. Update this row when those classes happen |

Not from the lectures, explained in full when used: `cor()` (Step 5), `parse_number()` (Step 3, only if needed), `geom_segment()` with arrows (Step 6, frame 4), `jsonlite` (app export).

Final check before submitting:

- [ ] The project renders from top to bottom with no errors.
- [ ] `data-raw/` is untouched.
- [ ] Row counts are explained at every join.
- [ ] The limits are stated plainly.
- [ ] The source is named on the final graph.
- [ ] The deployed link works on another device.

---

## 9. Timeline

Course dates come from the repo README and may not apply to this work. Treat them as soft markers until Sergio confirms. Today is Thu 29 Oct. The only deadline I know for sure is my own: 16 Nov.

| Date | Do | Course event (provisional) |
|---|---|---|
| Thu 29 Oct | Step 1: question and expected message. Start Step 2: download data | |
| Fri 30 Oct | Step 2 done: all five files import | Class 15: question in Moodle, import script and tidy check |
| 31 Oct to 6 Nov | Steps 3 and 4: clean and join | |
| Sat 7 Nov | Clean table from a script | Class 16 |
| 8 to 12 Nov | Step 5: explore. First draft of Step 6. Dummy app deployed by Mon 9 Nov. **Thu 12 Nov: go or no-go on the app** | |
| Fri 13 Nov, 23:59 | Moodle checkpoint script | Moodle deadline |
| 14 to 16 Nov | Final graph, report rendered, app live with real data, rehearsal | My deadline: 16 Nov (confirm what this covers) |
| Thu 19 Nov | Draft and refine, per the README | Classes 17 and 18 |
| Fri 20 Nov | Presentation, per the README | Classes 19 and 20 |

Build order if time gets short: get an ugly working version of the whole pipeline first (Steps 1 to 6), then deploy the app, then polish. The app can lose features. It cannot lose the clean data, the single honest graph, or the data quality notes.

---

## 10. Risks and open questions

- Data years differ by table. Death rates are often a year or two behind, and education may be a different year. Say so in the source line.
- Join problems (country codes, aggregate rows) are likely. Budget time for them.
- With about 27 countries, one outlier can move the trend line. Show the line with and without it if it matters.
- Cause-of-death data is coded and reported differently across countries, and the latest year may lag. Mention it as a caveat, no modelling needed.
- Optional extras for the story, only if time allows and only after the Thu 12 Nov go or no-go: bubble size by population (needs a population table, code to confirm, probably `demo_pjan`), and a year slider on the Exploration scatter plot, Rosling style (the data already has the year, so this is only app work). The five frames are the plan. Extras are not.
- Time makes the panel bigger (about 270 rows) and the join needs two keys. Check row counts for each year.
- The GDP index shows position relative to the EU average, not real growth. Say so.
- App scope: JavaScript is not part of the course. Keep the app to the interactions listed in section 6. Every extra feature is a risk to the deadline.
- Open question: what are the real requirements (deliverable form, talk length, deadlines)? Waiting for Sergio.
- Open question: is a Vercel app accepted as the deliverable? Ask Sergio.
- Open question: is AI-assisted code acceptable for this assignment, and how should I state it? I found no AI policy in the course repo files, so ask Sergio before submitting.
- Open question: what exactly is due on 16 Nov?

---

## 11. Course reference

Five principles: **Ask** (question and one-sentence message before code), **Show** (tidy data, right chart, honest scales), **Cut** (remove every mark that does not carry data), **Focus** (gray plus one color), **Tell** (title states the finding, add an annotation).

Chart by data type: 1 number, histogram. 2 numbers, scatter. 1 category, bar of counts. Number by category, box plot. Time, line. More variables: color, shape, size, facet. No 3D.

Bars start at zero. Lines may leave zero if the range is stated.

Palette: viridis by default. Gray plus one color to highlight one group.

Course repo: https://github.com/sergiouribe/Data_Visualization_Course_RSU
