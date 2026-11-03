# Project: Why do Baltic men die earlier?

Course: Data visualization and storytelling (SZF_184), RSU, autumn 2026. Lecturer: Sergio Uribe.
Student: Jolanta Stutane. Language of the project: English.
Tools: Positron, R, Quarto, Claude Code.

Start every Claude Code session with: "Read Project_idea.md first, then continue from the Progress section."

## What Sergio asks for, and how my plan delivers it

Sergio's final project template (published 31 Oct): https://github.com/sergiouribe/Data_Visualization_Course_RSU/tree/main/capstone/project_template. The rubric is in the same repo, `capstone/rubric.md`.

| Sergio asks for | My plan |
|---|---|
| One question that names a group, a measure and a period | Life expectancy gap, women minus men, EU member states, 2015 to 2024, Baltic states highlighted (section 2) |
| `final_project.Rproj`, `final_project.qmd` that renders from a clean session | Done. The `.qmd` keeps my own section order, with a table in its overview that maps Sergio's 7 template sections to mine (section 4) |
| `data/raw/` with the original files and `SOURCE.md` (link, date, licence) | Five Eurostat tables downloaded by code, never edited. `SOURCE.md` grows with each table |
| `data/clean/` (optional) | `gap_panel.csv`, the joined table |
| Each function commented with why | Comments for my future self on every line (section 1) |
| At least 4 graphs, each matched to a question type | Exploration (Step 5): question above each graph, finding with a number below |
| One refined final graph, saved to `figures/` | The story graph, built in frames, saved as `figures/final_graph.png` (Step 6) |
| `prompt_log.md` with prompts and one AI mistake fixed | My AI workflow, prompt examples, decisions table, and the mistakes I caught |
| `story.md`, one page | Question, data, answer with a number, final graph, evidence, limits (Step 7) |
| Hand-in: zip of the folder without `.Rproj.user`, with the rendered HTML | Step 7 |
| 5-minute defence: explain any chunk line by line, make one change in 2 minutes, say why this graph type | Our chunk-by-chunk way of working, and the practice in Step 8 |
| Talk: one graph in 3 minutes, scored on Ask, Show, Cut, Focus, Tell | 2 minutes "the road" from the app, then the 3 graded minutes on the final graph in frames (section 7) |

**My additions** on top of the template: the five data quality checks reported for every table, `notes/decisions.md` and `notes/glossary.md` for the talk and the defence, and a Vercel data app as the presentation tool. The app is not graded. The zip is.

**Still to ask Sergio:**

- The template asks for every prompt in full. Is a workflow description with prompt examples, plus the saved session history on request, accepted?
- May I take 2 minutes to show the road before the 3-minute graph?
- What is the hand-in deadline, and is the talk on Fri 20 Nov?

## Progress (Claude Code updates this at the end of every session)

- [x] Step 1. Ask: question locked, expected message written, factor guess written
- [x] Step 2. Get the data: five files in `data/raw/`, filters recorded, `SOURCE.md` updated, each one imports (1 Nov)
- [x] Step 3. Clean: five checks done for each table, "found, decided" summary written (1 Nov)

  Steps 2 and 3 are done table by table (load, then clean, then the next table):

  - [x] Life expectancy (`demo_mlexpec`): 2.1 loaded, 3.1 cleaned. `life_expectancy_clean` has 540 rows and 5 columns (30 Oct)
  - [x] Healthy life years (`hlth_hlye`): 2.2 loaded, 3.2 cleaned. `healthy_life_years_clean` has 536 rows and 5 columns, Luxembourg kept with 2023 and 2024 missing (31 Oct)
  - [x] Deaths by cause (`hlth_cd_asdr2`): 2.3 loaded, 3.3 cleaned (1 Nov). `deaths_clean` has 43 902 rows and 7 columns, 27 EU countries, 92 causes (all causes merged into `TOTAL`), `cause_level` marks all causes, 20 chapter-level groups and 71 detailed causes, 2015 to 2023
  - [x] GDP per capita (`tec00114`): 2.4 loaded, 3.4 cleaned (1 Nov). `gdp_clean` has 270 rows and 4 columns (no sex), 2015 to 2024. Luxembourg and Ireland kept, check with and without them in Step 5
  - [x] Education by sex (`sdg_04_20`): 2.5 loaded, 3.5 cleaned (1 Nov). `education_clean` has 540 rows and 5 columns. 2021 break in series (new survey method) noted for Step 5
- [x] Step 4. Combine: `data/clean/gap_panel.csv` saved, row counts explained (1 Nov). `gap_panel`: 270 rows, 17 columns. `deaths_by_cause`: 21 951 rows. Only `NA`: deaths 2024 (27 rows), healthy life years Luxembourg 2023 and 2024 (2 rows)

  Open for Step 4: (1) decided 1 Nov: `flag` is dropped before `pivot_wider()`. (2) decided 1 Nov: two tables, `gap_panel` with deaths from all causes, and `deaths_by_cause` with one row per country, year and cause. (3) GDP has no sex, so it joins on country and year only. (4) Deaths end in 2023, healthy life years for Luxembourg end in 2022: these become `NA` in the panel, never 0 Also decided 1 Nov: the deaths gap is a difference, men minus women, per 100 000. Join (1 Nov): `left_join()` onto life expectancy, 270 rows after every join.
- [ ] Step 5. Explore: at least 4 graphs in the template format, correlations, dead ends noted. Done so far (2 Nov): Graphs 1 to 15 with findings, dead ends appendix (education, alcohol disorders F10), draft summary in 5.9

  Open for Step 5, from the scientific check (2 Nov):
  - [x] A. Hypothesis 7 (overlap of factors): the three death factors correlate with each other at 0.77 to 0.84, so their effects cannot be separated with 27 countries. Show the overlap in a small correlation table and say so
  - [x] B. Statistical uncertainty: add p-values or confidence intervals with `cor.test()` (new function). Checked: all five p < 0.01, education without the Baltic states p = 0.30
  - [x] C. Robustness across years: show 2019 next to 2023. Checked: 2019 correlations are almost the same (circulatory 0.85, external 0.91, alcohol 0.80, GDP -0.53, education 0.47)
  - A, B and C done 3 Nov as 5.9 "How robust are the results?". Summary is now 5.10
  - [ ] D, E, G into Section 7, Limits: the death factors are built into the gap; only deaths under 65 were analysed (older men's deaths also add to the gap); country averages, not individuals (ecological fallacy)
  - [ ] F (optional, only if time allows): direct alcohol consumption from WHO as a sixth table
  - [x] Update the summary (now 5.10) with Graphs 13 to 15 and 5.9 (the gap is about the men; Lithuania's fall and Latvia's stagnation; poor health in Latvia for both sexes)
  - [ ] Graph 15 title: maybe "In Latvia, women and men alike live far fewer years in good health"
  - [ ] Lithuania alcohol policy around 2017 to 2018: find a citable source or leave it out. The data only shows when deaths fell, not why
- [ ] Step 6. Final graph: refined, saved as `figures/final_graph.png`, story frames exported, report title changed to the finding
- [ ] Step 7. Hand-in: renders from a clean session, `story.md` written, `prompt_log.md` complete, zip uploaded. App live
- [ ] Step 8. Rehearse: the 5-minute talk and the defence

Template files:

- [x] `final_project.Rproj` and `.gitignore` from the template (1 Nov)
- [x] Folder names: `final_project.qmd`, `data/raw/`, `data/clean/`, `figures/` (1 Nov)
- [x] YAML header with `execute:` `echo: true`, `warning: false`, `message: false` (1 Nov)
- [x] Overview table that maps the template sections to mine (1 Nov)
- [x] `data/raw/SOURCE.md`: all 5 tables (1 Nov)
- [ ] `prompt_log.md`: draft written (1 Nov). Fill in the 29 Oct row. Add prompts, decisions and caught mistakes at the end of each session. **Before hand-in:** decide the wording of step 2 so it matches how we really work (Claude proposes the next step from the plan, I say its purpose in my own words, Claude writes it)
- [ ] `story.md`: skeleton with the question (1 Nov). The rest after Step 6
- [x] Render test from a clean session after the renaming (1 Nov), and again with Section 5 and its 15 graphs (3 Nov)

Decisions so far (full reasons in `notes/decisions.md`):

- **Question (locked):** "Why do men in the Baltic states die so much earlier than women, compared with the rest of the EU?", with three sub-questions in `final_project.qmd`. The AI topic was dropped (29 Oct).
- **Expected message (30 Oct):** "Baltic men die earlier, and this goes mainly with heart disease deaths before 65." Written after the life expectancy table was loaded, but before the other four tables.
- **Data:** five Eurostat tables (section 3), 2015 to 2024, one row per country and year after the join.
- **Alcohol (29 Oct):** alcohol-related causes of death from `hlth_cd_asdr2` now. WHO alcohol consumption per person as a sixth table only after Step 4, if time allows.
- **Report structure (30 Oct, 1 Nov):** `final_project.qmd` follows my wine-dine notebook and the order the work was done: overview with main findings, pipeline and section map, a sub-section table at the start of every section, a "found, decided" summary after every cleaning step, dead ends in an appendix. A table in the overview maps Sergio's template sections to mine.
- **Names (1 Nov):** Sergio's template names. `analysis.qmd` became `final_project.qmd`, `data-raw/` became `data/raw/`, `data/` became `data/clean/`. The raw files were moved, not changed (checksums identical).
- **EU filter (30 Oct):** non-EU countries and aggregates are removed in Step 3, table by table, using the shared `eu_codes` list. **Exception (1 Nov):** the deaths table also has about 460 regions (55 MB in total), so its link asks for the 27 EU countries directly (3.5 MB).
- **Causes of death (1 Nov):** all 93 causes are downloaded (women and men, under 65), to avoid choosing data that fits my hypotheses. Analysis looks first at the non-overlapping chapters of causes, then deeper at the causes my hypotheses name. I raised the bias question myself (see `prompt_log.md`). In cleaning (1 Nov): the two codes for all causes (`TOTAL`, `A-R_V-Y`) are merged into `TOTAL`, and a column `cause_level` marks "all causes", "chapter" (17 chapters + 3 COVID-19 codes, which add up to all causes) or "detail". Zeros are kept as real counts.
- **Columns (30 Oct, 1 Nov):** each clean table drops Eurostat's technical columns and keeps `flag`. In Step 4 the flags are dropped before `pivot_wider()`, because they are already described in Section 3.
- **Luxembourg (31 Oct):** kept in healthy life years with 2023 and 2024 missing, not filled in. Name it in Limits.
- **Luxembourg and Ireland in GDP (1 Nov):** kept as real values, far above the rest (cross-border workers, company profits). Every chart against GDP in Step 5 is checked with and without them.
- **Education (1 Nov):** no new decision needed. 72 of the 104 `b` flags are in 2021, when the EU changed its labour force survey method, so a jump between 2020 and 2021 may be the method, not a real change.
- **Germany 2022 (31 Oct):** healthy life years flagged `bu` (low reliability). Kept, check it first if Germany jumps in 2022.
- **Charts in the checks (30 Oct):** viridis colours, a different pair per chart. Red (firebrick) is kept for the Baltic states only. Small tables with `knitr::kable()`.
- **Packages (31 Oct):** all packages load in the setup chunk, `patchwork` added in advance.
- **Working with Claude (1 Nov):** I say what each chunk needs to do, Claude writes it and explains the functions and the approach, then asks me one question to check that I understood. Then I run it (section 1, the same as in `prompt_log.md`).
- **App and talk (1 Nov):** the Vercel app stays, visually polished, as the presentation tool. Talk about 5 minutes: 2 minutes the road, 3 graded minutes on one graph. The zip comes first, the app go or no-go is on Thu 12 Nov.
- **Story style:** Rosling-like. One chart built up in frames, each with one spoken sentence (Step 6).

---

## 1. How Claude Code works with me

Claude Code writes the code. My job is to understand it well enough to explain every line in the defence, and to choose every analysis decision myself. So Claude Code acts as a **guide**: it builds step by step and teaches as it goes.

The steps below are the same nine as in `prompt_log.md`, section 1, with the same headings. Keep the two in line.

1. **First, my project idea.** I wrote this plan. Claude reads it at the start of every session and continues from the Progress section (it first read it when I asked it to get acquainted with my project).
2. **I say what it needs to do, Claude writes it.** For each chunk I say its purpose in my own words, for example: "Now we need to keep only the EU countries." If I'm not sure what comes next, Claude names the next step from the plan, and I put its purpose in my words. Claude writes the chunk: small, a few lines, never a whole script at once.
3. **Claude explains the technical side: which functions it uses.** What each function does, what its arguments mean, why this one and not an alternative, the approach, which class topic it connects to, and what the output should look like. If I don't understand, Claude explains again in simpler words or with an everyday analogy.
4. **Claude checks that I understood, with a question.** One question per chunk, before I run it, for example: "If Eurostat had written Greece as `GR` instead of `EL`, what would the row count print?" If my answer is wrong, Claude corrects me kindly and explains why. Then I run the chunk. We only discuss the output when something is unexpected or I ask. **From Section 5 on (2 Nov):** after each chunk Claude also gives one interesting finding from the output and a suggested next step.
5. **I decide, the AI suggests.** Claude never makes an analysis decision silently (which year, which filter, what to do with an outlier, which factor wins). It gives the options with a recommendation, and I choose. One decision at a time, don't rush me.
6. **Every decision is written down twice.** In `final_project.qmd` (academic voice) and in `notes/decisions.md` (my voice, for the talk). Decide first, then write it down, then code.
7. **New functions go into a glossary.** `notes/glossary.md`: each new function with a one-line meaning and a tiny example.
8. **Comments for my future self.** Every chunk has a header comment and a comment on every line (see Code style below).
9. **Fixed rules for the AI.** Never edit files in `data/raw/`. Never replace `NA` with 0. Never add Claude as co-author in my commits.

More rules:

- If something errors, explain the error before fixing it.
- Prefer functions the course has taught: `glimpse()`, `summary()`, `filter()`, `select()`, `mutate()`, `arrange()`, `distinct()`, `rename()`, `case_when()`, `pivot_longer()`, `pivot_wider()`, the joins (`left_join()`, `inner_join()`, `full_join()`, `anti_join()` with `join_by()`), `group_by()` with `summarize()`, `count()`, `ggplot2` (`geom_point()`, `geom_histogram()`, `geom_smooth(method = "lm")`, `facet_wrap()`, `labs()`, `theme_minimal()`, `scale_color_manual()`, `annotate()`, `ggsave()`), `ggrepel::geom_text_repel()`, `janitor::clean_names()`, `gtsummary::tbl_summary()`. If something outside the list is better, name it, explain it in two sentences, and let me decide.
- **Known new functions** (not from the course, explain fully when we reach them): `cor()` in Step 5, `parse_number()` only if Eurostat flags turn a number column into text, `geom_segment()` with arrows in Step 6, `jsonlite` for the app export.
- After each code piece, draft the explaining paragraph for `final_project.qmd`, short and plain. I edit it until I'm happy with it.
- At the end of each workflow step, give me a "can I explain this?" summary in three bullet points.
- **Step gate:** never start the next workflow step until I confirm I can explain the current one.
- At the start of a session, read the Progress section and continue from the first unchecked item. At the end, tick what is done, update the decisions list, and add to `prompt_log.md`.
- **Data app code** (HTML, CSS, JavaScript) follows the same approach, but lighter: explain the structure and the key parts, not every line. It is not course content.

Code style:

- All packages load once, in the setup chunk at the top, with `pacman::p_load()`. Never add a package in the middle.
- Paths: always `here("data", "raw", "file.csv")`. No absolute paths.
- Native pipe `|>`, not `%>%`.
- Object names: the full dataset name, then what the object holds: `life_expectancy_url`, `life_expectancy_file`, `life_expectancy_raw`, `life_expectancy_clean`. Others: `healthy_life_years_`, `deaths_`, `gdp_`, `education_`. Chunk labels follow the same names, for example `show-life-expectancy-load`.
- **Comments for my future self (Sergio's rule, also in his checklist):** every chunk starts with a header comment (section number and purpose), and every line or small group of lines has an inline comment that says what it does and why, or what result to expect. Written so that I can open the code in 10 years and still understand it.

Writing style:

- **In `final_project.qmd`: academic voice.** No "I", "my" or "me". Impersonal or passive phrasing ("the data is filtered in Section 3"), no contractions. The only exception is the personal sentence in Background ("I live in Latvia...").
- **In `notes/decisions.md`, `prompt_log.md`, the app and the talk: my own voice.** Plain, direct sentences of mixed length. Contractions are fine.
- No long dashes anywhere. Use commas, colons or full stops.
- No filler openers, no buzzwords, no "in today's world" phrasing.
- Titles state a finding, not a topic.

---

## 2. The question

**Question (locked):** Why do men in the Baltic states die so much earlier than women, compared with the rest of the EU?

- **Measure:** life expectancy at birth, women minus men, in years ("the gap").
- **Group:** the 27 EU member states, with Estonia (EE), Latvia (LV) and Lithuania (LT) highlighted.
- **Period:** 2015 to 2024, or the latest year each table has.

**Sub-questions:**

1. Which country-level factors (causes of death, alcohol, income, education) are associated with the size of the gap?
2. How has the gap changed between 2015 and 2024?
3. Where do Estonia, Latvia and Lithuania sit compared with the other EU countries?

**Expected message (written 30 Oct, before the data was explored, kept unchanged):** "Baltic men die earlier, and this goes mainly with heart disease deaths before 65."

**Wording rule:** say "associated with" and "goes with". Never say "causes". With 27 countries the data cannot show cause.

**Angles to compare in Exploration** (the final graph uses the one with the clearest message):

1. The Rosling chart: life expectancy against GDP per capita, women and men side by side. Do Baltic women fit the EU pattern while Baltic men fall below it?
2. Causes of death: which cause (circulatory diseases, cancer, external causes such as accidents and suicide, alcohol-related causes) differs most between men and women under 65 in the Baltics?
3. Healthy life years: how many years of good health do men have compared with women?
4. Education: does the gap in tertiary education between women and men line up with the life expectancy gap?
5. The Baltics against the rest of the EU.
6. Change over time: did the gap narrow between 2015 and 2024, and which countries gained most?

**Starting hypotheses, not results:**

1. A bigger gap goes with higher male death rates under 65 from circulatory diseases.
2. External causes (accidents, suicide) also go with a bigger gap.
3. More alcohol-related deaths among men go with a bigger gap.
4. Poorer countries have lower life expectancy for both sexes, but men fall further below the pattern in the Baltics.
5. Countries where women are much more educated than men have bigger gaps.
6. The gap has narrowed in most countries since 2015, but not equally.
7. These factors overlap. Check which one still matters, and which are stand-ins for another (for example wealth or alcohol).

**Early hints from press coverage, to check against the data:** Latvia has the widest gap in the EU (9.8 years in 2024), then Lithuania (8.6) and Estonia (8.5). Baltic men have the lowest healthy life years in the EU (the data so far agrees: the lowest value in the healthy life years table is Latvian men, 2017). Lithuania had the highest male death rate from ischaemic heart disease in 2019. Women are much more educated than men in Latvia and Estonia. An older report listed the gaps as Lithuania 10.7, Latvia 9.8 and Estonia 8.9 years (year to check), so Lithuania's gap may have narrowed the most.

---

## 3. Data

Five Eurostat tables, joined on country code and year. Each one is downloaded from R through the Eurostat API (an SDMX-CSV link built with `paste0()`, saved with `download.file()` only if the file doesn't exist yet) into `data/raw/`. The filters live in the link, so the download is reproducible. Each table gets an entry in `data/raw/SOURCE.md`. Licence: CC BY 4.0, re-use allowed with the source named.

| Short name | Eurostat code | Variable | Status |
|---|---|---|---|
| Life expectancy | `demo_mlexpec` | Life expectancy at birth (`Y_LT1`), women and men, in years | Done. 2015 to 2024, all 27 EU countries complete |
| Healthy life years | `hlth_hlye` | Healthy life years at birth (`HLY_Y0`), women and men | Done. 2015 to 2024, Luxembourg missing 2023 and 2024 |
| Deaths by cause | `hlth_cd_asdr2` | Standardised death rate per 100 000 (`RT`), all 93 causes, women and men, under 65 (`Y_LT65`), the 27 EU countries selected in the link | Done. 2015 to 2023 (no 2024 yet). All causes merged into `TOTAL`, `cause_level` marks all causes, 20 chapter-level groups and 71 detailed causes |
| GDP per capita | `tec00114` | GDP per capita in PPS, index EU27 = 100, no sex dimension | Done. 2015 to 2024, complete. Luxembourg and Ireland far above the rest, kept |
| Education by sex | `sdg_04_20` | % of people aged 25 to 34 with tertiary education (`ED5-8`), women and men | Done. 2015 to 2024, complete. Break in series in 2021 (new survey method) |

Fallback for GDP if `tec00114` gives trouble: `nama_10_pc`, unit `CP_PPS_EU27_2020_HAB`, indicator `B1GQ`.

**GDP over time:** `tec00114` compares countries within a year. Over the years it shows catching up or falling behind the EU average, not real growth. Say so in the story. For real growth, switch to `nama_10_pc`, unit `CLV10_EUR_HAB`.

**Alcohol, stage 2 (optional, only after Step 4):** total alcohol consumption per person aged 15+ from the WHO Global Health Observatory. It uses three-letter country codes (`LVA`, not `LV`), so it needs a code lookup before the join, and its latest year may be older than 2024. Eurostat's drinking survey (EHIS) only has a few waves, so it is not used.

Other tables, if an angle needs them later: life expectancy by education level (`demo_mlexpecedu`, about 19 countries), median age (`demo_pjanind`), population (`demo_pjan`, for bubble size).

Rules:

- Keep 2015 to 2024 unless a table forces a shorter period. Write down each table's years.
- Write down the exact filters (indicator, sex, age class, cause, unit) in `notes/decisions.md` and `SOURCE.md`. A reader must be able to repeat the download.
- Life expectancy is a snapshot: it assumes this year's death rates last a whole life. Say so in the report and the app.
- Tables have different latest years. Write the year of each table in the final graph's source line.
- Name the source on every chart: "Source: Eurostat, [codes], accessed [date]".

What to expect in cleaning (learned so far, and still to watch):

- Different country coverage per table, including non-EU countries and aggregates such as `EU27_2020`. Removed with `eu_codes` in Step 3.
- Missing years show up as missing rows, not empty cells. Only the row counts (Check 5) reveal them.
- Flags (`e` estimated, `p` provisional, `b` break in series, `u` low reliability) are kept, and the `flag` column stays.
- 2020 and 2021 show a COVID dip in life expectancy. It is real, not an error.
- The deaths table has many causes and age groups. Filter to one at a time and check the row count each time.
- Country codes may not match across tables (Greece is `EL` in Eurostat). Check with `anti_join()` at the join.
- Read every file with `read_csv(na = c("", ":", "NA"), show_col_types = FALSE)`. Report missing values, never replace them.

---

## 4. Folder structure and the Quarto document

```
Final_work/                # Sergio's project_template structure
├── final_project.Rproj    # RStudio project file, sets the project root
├── final_project.qmd      # the whole analysis: text that explains, then code
├── story.md               # one page: question, answer, final graph (required)
├── prompt_log.md          # AI workflow, prompts, decisions, AI mistakes (required)
├── data/
│   ├── raw/               # untouched Eurostat downloads, plus SOURCE.md (required)
│   └── clean/             # gap_panel.csv
├── figures/               # final_graph.png and the story frames, 300 dpi
├── Project_idea.md        # this plan
├── .here                  # empty file, a second way for here() to find the root
├── notes/
│   ├── decisions.md       # every choice and why, in my voice
│   └── glossary.md        # each new function, one line each
├── R/
│   └── 99_export_for_app.R    # writes the JSON and images the app reads
└── app/                   # the Vercel data app (static files only)
    ├── index.html
    ├── style.css
    ├── app.js
    ├── data/              # JSON exported from R
    └── img/               # story frames from R
```

The hand-in zip is the whole folder without `.Rproj.user`, with `final_project.html` included.

### The Quarto document

Header of `final_project.qmd`:

```yaml
---
title: "Why do Baltic men die earlier?"   # change to the finding after Step 5
author: "Jolanta Stutane"
format:
  html:
    code-fold: true
    code-summary: "Show the code"
    toc: true
    embed-resources: true
execute:
  echo: true        # as in Sergio's template
  warning: false
  message: false
---
```

My sections, and where Sergio's template sections are in them (the same table is in the document's overview):

| My section | Content | Sergio's template section |
|---|---|---|
| Project overview | Background, aim, question, assumptions, main findings, pipeline, section map | 2. Question and data |
| 1. Environment setup | 1.1 packages, 1.2 project folder | 1. Packages |
| 2. Gathering data | 2.1 to 2.5, one per table: build the link, download once, load and look | 2. Question and data |
| 3. Cleaning | 3.1 to 3.5, one per table: tidy names, five checks, "found, decided" summary, apply the decisions | 3. Exploration (the checks) and 4. Wrangling |
| 4. Combining the tables | Reshape with `pivot_wider()`, compute the gap, join, check, save | 4. Wrangling |
| 5. Exploration | At least 4 graphs, question above, finding with a number below | 5. Graphs |
| 6. The story | The final graph in frames, saved as `figures/final_graph.png` | 6. Final graph |
| 7. Limits | What the data cannot tell | 7. Limits |
| Appendix | Dead ends | |

Pattern inside every section:

1. **Rationale:** two to five sentences, academic voice, plain words.
2. **Code:** one or a few short chunks, each with a header comment and inline comments.
3. **What it shows / Result:** one or a few sentences on the result.

Rules:

- Never put code without explaining text before it.
- Chunks the app should display get a label that starts with `show-`.
- Render with Session > Restart R, then Render (or `quarto render final_project.qmd`).

---

## 5. Workflow: eight steps

Each step has a task and a "done when", and points to the class that taught the tools.

### Step 1. Ask (done)
- Question, expected message and the factor I expect to matter most, written before the data was explored.

### Step 2. Get the data
- **Task:** download each CSV into `data/raw/` through the Eurostat API. Every table follows the same three steps in `final_project.qmd`: build the link, download once, load and look. Record the filters in `notes/decisions.md` and add the table to `data/raw/SOURCE.md` (link, download date, last Eurostat update, size). Import with `read_csv(na = c("", ":", "NA"), show_col_types = FALSE)`, look with `glimpse()` and `head()`.
- **Done when:** each file imports without errors. No `clean_names()` here: the raw tables keep Eurostat's own names.

### Step 3. Clean (class 3 data quality checklist, classes 4 and 6)
- **Task:** for each table, four stages: tidy the names (`clean_names()`, `rename()`), run the five checks, write the "found, decided" summary, apply the decisions (EU filter with `eu_codes`, keep five columns with `select()`).

| # | Check | How |
|---|---|---|
| 1 | Types | `glimpse()` |
| 2 | Missing values | `tbl_summary()`, `count(flag)` |
| 3 | Units and ranges | Histogram by sex with `facet_wrap()`, dashed lines for the expected range |
| 4 | Duplicates | `distinct()` and `nrow()`, compare |
| 5 | Row counts | Bar chart of rows per unit, EU highlighted, dashed line at the expected count |

- **Done when:** each table has its "found, decided" summary, and the clean table has the expected rows and five columns.

### Step 4. Combine (classes 5, 7, 8, 9)
- **Task:** the tables arrive already limited to the EU (Step 3) and to 2015 to 2024 (the links). Reshape with `pivot_wider()` so women and men sit side by side, compute the gaps with `mutate()`, and join the tables with `join_by(country_code, year)`. After each join, check the row count (27 countries times 10 years, 270 rows) and run `anti_join()` to list country-years that did not match. Save the result to `data/clean/gap_panel.csv`.
- **Core columns first:** `country_code`, `country`, `year`, `le_women`, `le_men`, `le_gap` (women minus men), `hly_women`, `hly_men`, `gdp_pps_index`, `baltic` (TRUE for EE, LV, LT). Add the rest for the angles in Step 5: `death_circ_men_lt65`, `death_circ_women_lt65`, `death_cancer_men_lt65`, `death_cancer_women_lt65`, `death_ext_men_lt65`, `death_ext_women_lt65`, `death_alc_men_lt65`, `death_alc_women_lt65`, `tertiary_women_pct`, `tertiary_men_pct`.
- **Decide here:** does `flag` go into `gap_panel`?
- **Done when:** one row per country and year, no duplicates, missing values documented, row counts explained.

**Possible Moodle checkpoint (course README, confirm with Sergio): Fri 13 Nov 2026, 23:59.** Five lines that load the clean data and run without errors:

```r
pacman::p_load(tidyverse, here)
here("data", "clean", "gap_panel.csv") |>
  read_csv(show_col_types = FALSE) |>
  glimpse() |>
  nrow()
```

### Step 5. Explore (class 3 chart by type, class 10 chart by question)
- **Template format:** at least 4 graphs. Each answers one question: the question as the heading above the chunk ("Graph 1: [question]") and one finding sentence with a number below ("Finding: ..."). Each graph type matches a question type from class 10 (distribution, counts, scatter, differences, time as a line). Be ready to say why this graph type.
- **Distributions:** one histogram per variable.
- **Factors:** one scatter plot of `le_gap` against each candidate explanation (male death rate from each cause, GDP, the tertiary education gap), same y axis, with `facet_wrap()` and `geom_smooth(method = "lm")`. Report the correlation for each pair with `cor()` (new function, explain first, including what the number means and how missing values are handled). Check each outlier: data error or real value? One sentence per outlier.
- **Overlap:** if every explanation also goes with GDP, say they may be stand-ins for wealth.
- **Time:** the gap over the years for every country as lines, with the Baltics in red. `group_by(country_code)` and `summarize()` for each country's change from 2015 to 2024. Remember the 2020 and 2021 dip.
- **Group comparison:** the Baltic average against the rest of the EU with `group_by(baltic)` and `summarize()`, shown as a box plot or bars.
- **The Rosling chart:** `pivot_longer()` to put women and men in one `sex` column, then life expectancy against GDP, one dot per country and sex. Do Baltic men sit far below the pattern while Baltic women do not? Try this angle first.
- **Dead ends** go in `notes/decisions.md` and the appendix of `final_project.qmd`.
- **Done when:** I can name the strongest explanation, the weakest, one surprise, which angle has the clearest message, and whether GDP explains most of the pattern.

### Step 6. The final graph (Show, Cut, Focus, Tell; classes 11, 12, 13)
- **Task:** build one refined graph from the clearest angle.
  - Highlight the Baltics with the `baltic` column: `scale_color_manual(values = c("TRUE" = "firebrick", "FALSE" = "gray70"))`, `theme_minimal(base_size = 14)`, `theme(legend.position = "none", panel.grid.minor = element_blank())`. Labels with `ggrepel::geom_text_repel()`, the note with `annotate("text", ...)`.
  - Title states the finding. Example shape only: "[Group of countries] have the widest gap between women and men in life expectancy, [by how much]".
  - One annotation pointing at the evidence.
  - Source line, axis labels with units, ordered categories, no legend or grid line without a job, no border. Honest scales: bars from zero, say if an axis does not start at zero.
- **Checklist before export (the rubric):**
  - [ ] Ask: the title answers the question.
  - [ ] Show: right chart for the data, honest scales, data quality applied.
  - [ ] Cut: no non-data ink.
  - [ ] Focus: gray plus one color.
  - [ ] Tell: finding title, one annotation, source.
- **Build it in frames (Rosling style).** One chart in five frames, after an opening question. Each frame adds one layer and gets one spoken sentence. Axes, scales and point size stay the same in every frame. Starting idea, decided after Exploration:

| Frame | What appears | What I say (one sentence) | Layer in R |
|---|---|---|---|
| 0 | A question to the audience, no chart (for example: which EU country has the biggest gap between how long women and men live?) | [write it] | none |
| 1 | Women's and men's life expectancy against GDP in 2024, two dots per country, all gray | [what the axes mean] | `geom_point()` in `gray70`, a different shape for men |
| 2 | Each country's two dots joined by a line, the gap | [what the line length means] | `geom_line(aes(group = country_code))` |
| 3 | The Baltics in red, with labels | [where the Baltics sit] | `scale_color_manual()`, `ggrepel::geom_text_repel()` |
| 4 | Arrows from the 2015 to the 2024 position for the Baltics | [how far each country moved] | `geom_segment()` with an arrow |
| 5 | One annotation, the finding as the title, the source | [the finding in one sentence] | `annotate("text", ...)`, `labs(title = ..., caption = ...)` |

  Rules for the frames: say what to look at before it appears. One idea per frame. The last frame states the finding. Build by saving the plot after each layer (`p1`, then `p2 <- p1 + ...`).
- **Export:** each frame with `ggsave(here("figures", "story_frame_1.png"), width = 8, height = 5.5, dpi = 300)`, up to frame 5. Frame 5 is also saved as `figures/final_graph.png`.
- **Report title:** change the `title:` in the YAML header from the question to the finding.

### Step 7. Hand-in and the app
- **Report:** Session > Restart R, then Render. `final_project.html` must render from top to bottom with no errors.
- **`story.md`:** one page: question, data (two sentences), the answer in one sentence with a number, the final graph, evidence (three to five sentences with numbers), limits.
- **`prompt_log.md`:** complete the workflow, prompt examples, decisions table and caught mistakes.
- **Zip:** the whole `Final_work/` folder without `.Rproj.user`, with `final_project.html`. Upload to Moodle.
- **App:** follow section 6. Presentation tool, not graded.
- **Done when:** the checklist in section 8 is all ticked, the zip is uploaded, and the app is live with numbers that match the report.

### Step 8. Polish and rehearse
- Run the checklist in section 8.
- Write the talk (section 7).
- **Practise the defence:** pick a random chunk and explain every line out loud. Practise one quick change (reorder a bar chart, add a `facet_wrap()`) in under 2 minutes. For every graph, say in one sentence why this graph type.
- Rehearse the talk twice with a timer. Prepare an offline backup.

---

## 6. The Vercel data app (presentation tool)

**What it is:** a small interactive web app in plain HTML, CSS and JavaScript, hosted on Vercel, visually polished. I present from it. It walks through seven sections that match `final_project.qmd`: Question, Data, Cleaning, Combining the tables, Exploration, The story, Limits. Each section shows the logic, the result, and an info button that reveals the R code that produced it. R does all the analysis. The app only displays what R exported. It is not part of the graded hand-in.

**Why this split:**

- The analysis stays in R, with course tools, and runs top to bottom.
- The code shown in the app is extracted from `final_project.qmd`, so it cannot drift from what actually ran.
- No framework, no build step, no npm packages. Fewer things to break before the deadline.
- The story frames are ggplot2 images exported from R, so Cut, Focus and Tell are applied with the course's own tool.

### Data flow

```
final_project.qmd  ->  R/99_export_for_app.R  ->  app/data/*.json + app/img/story_frame_*.png
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
| `app/data/correlations.json` | Correlation of the gap with each candidate explanation, plus n | Exploration |
| `app/data/code.json` | R code per step, extracted from `final_project.qmd` | All |
| `app/data/steps.json` | Two to four sentences per step on the logic, shortened from `final_project.qmd` | All |
| `app/img/story_frame_1.png` to `story_frame_5.png` | The story frames from R. The last one is `final_graph.png` | The story |

**Code extraction.** Chunks in `final_project.qmd` whose label starts with `show-` are the ones the app displays. `99_export_for_app.R` reads `final_project.qmd`, collects those chunks by label, and writes `code.json`. It uses `jsonlite`, which the course has not covered, so Claude Code explains that script line by line.

### Page layout

One page that scrolls, with a sticky navigation for the seven sections. Each section has four parts:

1. A heading that says what happened, not only the topic.
2. The logic: two to four sentences on why I did it.
3. The result: a table or a chart.
4. An info button ("Show the R code") that opens the code block, with a copy button.

### Interactions (keep to this list)

- **Cleaning:** a table of the five checks per dataset. Click a dataset to see my notes.
- **Combining the tables:** row counts before and after each join, and the list of unmatched countries.
- **Exploration:** a dropdown to switch the explanation on the x axis. Scatter plot with a hover tooltip (country, both values). A toggle to highlight the Baltics. The correlation and n for the selected explanation.
- **The story:** the five frames with next and back buttons, and one caption per frame (my spoken sentence). The opening question comes first. The last frame is the final graph.

### Design rules

- Same colors as the ggplot: `gray70` for all countries, `firebrick` for the Baltics. Use the same two colors in the CSS.
- Large text and high contrast, readable on a projector. Laptop screen first, mobile only if time remains.
- Titles state a finding. A source line under every chart.
- Every number in the app comes from the JSON files. Never type a result by hand.
- One charting library from a CDN (Plotly.js or Observable Plot). Pick one on day one and stay with it.

### Technical notes

- `fetch()` does not work on a page opened as a `file://` link. Test locally with a small server inside `app/`: `python -m http.server 8000`, or `npx serve app`.
- Commit the JSON files and the images. Vercel does not run R.
- Deploy with the CLI (`npm i -g vercel`, then `vercel deploy app --prod`), or import the GitHub repo in Vercel with Root Directory `Final_work/app`, framework preset "Other", no build command.
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

If the app does not work end to end by **Thu 12 Nov**, I present from the rendered HTML report and the PNG frames instead. The zip is complete either way.

---

## 7. The presentation and the defence

**The talk, about 5 minutes, from the app:**

| Time | Part | In the app |
|---|---|---|
| 0:00 to 2:00 | The road: the question, the five Eurostat tables, one real problem from cleaning (for example Luxembourg's missing years), the join, one dead end | Question, Data, Cleaning, Combining, Exploration |
| 2:00 to 5:00 | The graded part: one graph. Open with the question, build it in frames, end on the finding (Ask, Show, Cut, Focus, Tell). Beginning, middle and end | The story |

If Sergio enforces exactly 3 minutes, the road shrinks to about 30 seconds.

**The defence, 5 minutes:** Sergio points to one chunk and I explain each line. He asks for one change (a new order, a facet) and I have 2 minutes. I answer: why this graph type for this question?

Rules for the talk:

- Hints from classmates (29 Oct, secondhand): make a "wow" moment, use everything learned in the classes, show the code and tell how I did it, as a story. The opening question and the frames are my "wow" moment.
- Open the info button at most once, at the moment where the logic matters most.
- Keep `final_project.qmd` open in Positron next to the app, for the defence.
- Say the finding in one sentence at the start of the graph part and again at the end.
- Optional: one light joke, friendly and never at anyone's expense. Draft: "Estonia added seven years of life expectancy in twenty years. For a country that runs its government like software, that is just a good update cycle." Use it only if it feels natural.
- Offline backup: the app running from a local server on the laptop, the rendered HTML report, the PNG frames. Wi-Fi fails at the worst time.

---

## 8. Checklist

**Sergio's template checklist:**

- [ ] The question names a group, a measure and a period, and the data can answer it.
- [ ] The original data sit in `data/raw/` and `SOURCE.md` has the link and the licence.
- [ ] `final_project.qmd` renders from a clean session (Session > Restart R, then Render).
- [ ] Each function in the code has a comment that says why I use it.
- [ ] At least 4 graphs, each matched to a question type from the course.
- [ ] One graph is refined: title that states the finding, labels, order, one highlight color.
- [ ] `prompt_log.md` lists my prompts, and one entry describes an AI mistake I fixed.
- [ ] `story.md` fits on one page.
- [ ] I can explain every line of my code in the 5-minute defence.

**Sergio's rubric** (100 points, the capstone is 60% of the course grade; each principle scored 0 missing, 1 partial, 2 good, 3 complete):

| Principle | What the audience checks | Points |
|---|---|---|
| Ask | The question is clear and the data can answer it. The message fits in one sentence | 20 |
| Show | Chart type fits the data type. Honest scales (bars from zero, labelled log axes). Tidy data, the data quality checklist applied and reported, the script runs | 25 |
| Cut | No non-data ink. No legend, border or grid line without a job | 15 |
| Focus | One element stands out (gray plus one color). The eye goes to the message | 15 |
| Tell | The title states the finding. One annotation points to the evidence. The talk has a beginning, middle and end | 25 |

**My own final checks:**

- [ ] `data/raw/` is untouched.
- [ ] Row counts are explained at every join.
- [ ] The limits are stated plainly.
- [ ] The source is named on the final graph.
- [ ] The zip has the whole folder without `.Rproj.user`, and includes `final_project.html`.
- [ ] The deployed app works on another device.

**Course coverage**, so the project shows what the classes taught. Every row is used at least once, on purpose, with a sentence in `final_project.qmd` saying why.

| Class | Tools and ideas | Where they show up in the project |
|---|---|---|
| 01 Why storytelling | Bad graphs, the five principles, baseline and axis tricks with `coord_cartesian()`, why pie charts mislead | Step 6 checklist. Optionally a "bad" version next to the good one with `patchwork` |
| 02 First graph | `pacman::p_load()`, `read_csv()`, `glimpse()`, `ggplot()`, `geom_point()`, `labs()`, `ggsave()` | Setup chunk, Step 2 import, scatter plots, exporting the frames |
| 03 Know your data | `summary()`, `count()`, `distinct()`, `arrange()`, chart by data type, outlier check, the data quality checklist | Step 3: the five checks for every table. Step 5: one sentence per outlier |
| 04 Row and column verbs | `filter()`, `select()`, `mutate()`, `arrange()`, `rename()`, `case_when()`, `is.na()`, `gtsummary::tbl_summary()`, `janitor::clean_names()` | Step 3: names, `tbl_summary()`, EU filter, columns. Step 4: `case_when()` for the `baltic` column |
| 05 Tidy data | Tidy data rules, `pivot_longer()` | Step 4: every Eurostat table is tidy. Step 5: `pivot_longer()` for the Rosling chart |
| 06 dplyr recap | The verbs together in one pipe, gapminder, `geom_line()` | Step 4: the pipeline. Step 5: the gap over time |
| 07 Pivot | `pivot_wider()`, `pivot_longer()` | Step 4: women and men side by side to compute the gap |
| 08 Joins | `left_join()`, `inner_join()`, `full_join()`, `anti_join()`, `join_by()` | Step 4: join on `join_by(country_code, year)`, `anti_join()`, row counts |
| 09 Split, apply, combine | `group_by()`, `summarize()`, `count()`, `arrange()` | Step 5: Baltic average against the rest, change from 2015 to 2024 |
| 10_1 Distribution | `geom_histogram()`, `geom_density()` | Step 3: range checks. Step 5: one histogram per variable |
| 10_2 Counts | `geom_bar()`, `geom_col()`, `fct_reorder()`, `coord_flip()`, `slice_max()` | Step 3: rows per country (done). Step 5: ranked bar chart of the gap |
| 10_3 Scatter | `geom_point()`, `geom_smooth(method = "lm")`, `facet_wrap()` | Step 5: the gap against each explanation. The Rosling chart |
| 10_4 Differences | `geom_boxplot()`, `geom_violin()`, `geom_jitter()`, `stat_summary()` | Step 5: Baltics against the rest of the EU |
| Later classes | Themes, highlight color, `ggrepel`, `annotate()` | Step 6: the story frames. Update when those classes happen |

---

## 9. Timeline

Course dates come from the course README and may change. My own deadline: 16 Nov.

| Date | Do | Course event |
|---|---|---|
| 29 Oct to 1 Nov | Steps 1, 2 and 3 done: all five tables loaded and cleaned. Template structure adopted | |
| 2 to 6 Nov | Step 4: reshape and join into `gap_panel`. Start Step 5 | |
| Sat 7 Nov | Clean table from a script | Class 16 |
| 8 to 12 Nov | Step 5: explore. First draft of Step 6. Dummy app deployed by Mon 9 Nov. **Thu 12 Nov: go or no-go on the app** | |
| Fri 13 Nov, 23:59 | Moodle checkpoint script | Moodle deadline |
| 14 to 16 Nov | Final graph, `story.md`, `prompt_log.md`, render test, zip. App live with real data. Rehearsal | My deadline: 16 Nov |
| Thu 19 Nov | Draft and refine | Classes 17 and 18 |
| Fri 20 Nov | Presentation and defence | Classes 19 and 20 |

If time gets short: get an ugly working version of the whole pipeline first (Steps 1 to 6), then the hand-in files, then the app, then polish. The app can lose features. The zip cannot lose the clean data, the final graph, the data quality notes, `story.md` or `prompt_log.md`.

---

## 10. Risks

- Data years differ by table. Death rates are often a year or two behind. Say so in the source line.
- Join problems (country codes, aggregate rows, missing years like Luxembourg's) are likely. Budget time for them.
- With 27 countries, one outlier can move the trend line. Show the line with and without it if it matters. Known from the checks: Luxembourg (GDP index 244 to 279, cross-border workers) and Ireland (178 to 237, profits of international companies) sit far to the right in any chart against GDP.
- Cause-of-death data is coded and reported differently across countries. Mention it as a caveat.
- The GDP index shows position relative to the EU average, not real growth. Say so.
- App scope: JavaScript is not part of the course. Keep the app to the interactions in section 6. Every extra is a risk to the deadline.
- Optional extras, only after the 12 Nov go or no-go: bubble size by population (`demo_pjan`), a year slider on the Exploration scatter plot.

---

## 11. Course reference

Five principles: **Ask** (question and one-sentence message before code), **Show** (tidy data, right chart, honest scales), **Cut** (remove every mark that does not carry data), **Focus** (gray plus one color), **Tell** (title states the finding, add an annotation).

Chart by data type: 1 number, histogram. 2 numbers, scatter. 1 category, bar of counts. Number by category, box plot. Time, line. More variables: color, shape, size, facet. No 3D.

Bars start at zero. Lines may leave zero if the range is stated.

Palette: viridis by default. Gray plus one color to highlight one group.

Course repo: https://github.com/sergiouribe/Data_Visualization_Course_RSU
