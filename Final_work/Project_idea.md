# Project: What goes with AI use in EU enterprises?

Course: Data visualization and storytelling (SZF_184), RSU, autumn 2026. Lecturer: Sergio Uribe.
Student: Jolanta. Language of the project: English.
Tools: Positron, R, Quarto, Claude Code. Deliverables: the whole analysis written as one Quarto document (text that explains, then code), plus an interactive data app in plain HTML, CSS and JavaScript hosted on Vercel.

Put this file in the project root. Start every Claude Code session with: "Read Project_idea.md first, then continue from the Progress section."

**Status:** Sergio has not sent the requirements for the final work yet. Until he does, we work like a data analyst, step by step: find the data, clean it, explore it, manipulate it, then tell the story. The course repo's capstone files (template, rubric, dates) are not the spec. Use them only as loose hints about what the course values. When the requirements arrive, we adjust.

**First session, in this order:**

1. Create the folders from section 4 and an empty `.here` file.
2. Create `analysis.qmd` with the header and setup chunk from section 4.
3. I write the question and the expected message in section 1 of `analysis.qmd`, before any code runs on the data.
4. Download the five Eurostat tables into `data-raw/`, record the filters in `notes/decisions.md`, and import each one with `glimpse()`.

## Progress (Claude Code updates this at the end of every session)

- [ ] Step 1. Ask: question locked, expected message written, indicator guess written
- [ ] Step 2. Get the data: five files in `data-raw/`, filters recorded, each one imports
- [ ] Step 3. Clean: five checks done for each table, notes written
- [ ] Step 4. Combine: `data/ai_panel.csv` saved, row counts explained
- [ ] Step 5. Explore: histograms, scatter plots, correlations, dead ends noted
- [ ] Step 6. Story graph: final plot exported
- [ ] Step 7. Report and app: `analysis.qmd` renders, app is live
- [ ] Step 8. Rehearse the 10-minute talk

Decisions so far:

- Work mode: guided build, one step at a time, with a step gate (see section 1).
- Question: draft wording in section 2, not locked yet.
- Expected message: not written yet.
- Tables: five, codes in section 3, filters still to confirm on download.

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
- Never make an analysis decision silently (which year, which filter, what to do with an outlier, which indicator wins). Give options with a recommendation, and I choose.
- Never edit files in `data-raw/`. Never replace `NA` with 0.
- After each code piece, draft the explaining paragraph for `analysis.qmd` in my voice, short and plain. I edit it until it sounds like me.
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
- One comment per data quality check, written as a sentence.

Writing style for any text Claude drafts for me (app text, titles, captions, notes):

- No long dashes. Use commas, colons or full stops.
- Plain, direct sentences of mixed length. Contractions are fine.
- No filler openers, no buzzwords, no "in today's world" phrasing.
- Titles state a finding, not a topic.

---

## 2. The question

**Main question:** Which country-level indicators are most strongly associated with enterprise AI use in the EU in 2025, and where do the Baltic states sit?

- Number: share of enterprises using at least one AI technology.
- Group: EU member states (about 27 countries).
- Period: 2025.
- Highlighted group: Estonia (EE), Latvia (LV), Lithuania (LT).

**Wording rule:** say "associated with" and "goes with". Never say "causes" or "fosters". With about 27 countries the data cannot show cause.

**Expected message (fill in before any code, then keep it unchanged):**

> [One sentence. Write it myself before any code runs on the data. Examples of the shape: "Countries with more ICT specialists have more AI-using firms, but Latvia is an exception." or "No single indicator explains AI use."]

Starting hypotheses, not results:

1. More ICT specialists in the workforce goes with more AI-using firms.
2. A more digitally skilled population goes with more AI-using firms.
3. Higher R&D spending goes with more AI-using firms, but this may only reflect wealth.
4. Richer countries (higher GDP per capita) have more AI-using firms. Check whether the other indicators still line up with AI use, or whether they are mostly stand-ins for wealth.
5. Early hint to check: Latvia may have a high share of ICT specialists but low AI use.

---

## 3. Data

Five Eurostat tables, joined on country code. Download each as CSV from the Eurostat data browser into `data-raw/`.

| Short name | Eurostat code | Variable I want | Year target | Status |
|---|---|---|---|---|
| AI use | `isoc_eb_ai` | % of enterprises using at least one AI technology, all sizes, all sectors | 2025 (2024 for context) | to confirm on download |
| ICT specialists | `isoc_sks_itspt` | ICT specialists, % of total employment | 2025 | to confirm on download |
| Digital skills | `isoc_sk_dskl_i21` | Individuals with at least basic digital skills (`I_DSK2_BAB`), % of individuals | 2025 | to confirm on download |
| R&D | `rd_e_gerdtot` | R&D expenditure, % of GDP, all sectors | latest year, probably 2024 | to confirm on download |
| GDP per capita | `tec00114` | GDP per capita in PPS, index with EU27 = 100 (built for comparing countries) | 2024, latest I found | to confirm on download |

Optional extra table, only if time allows: SME digital intensity (`isoc_e_dii`).

Fallback for GDP if `tec00114` gives trouble: `nama_10_pc`, unit `CP_PPS_EU27_2020_HAB`, indicator `B1GQ` (GDP at market prices per capita, in PPS).

Rules:

- Write down the exact filter labels I choose on the Eurostat site (indicator, size class, sector, unit) in `notes/decisions.md`. A reader must be able to reproduce the download.
- The AI table covers enterprises with 10 or more employees. State this in the app.
- R&D and GDP are probably 2024, one year older than the AI data. Say so in the final graph's source line.
- Name the source on every chart: "Source: Eurostat, [codes], accessed [date]".

Things to expect in cleaning:

- Different country coverage per table (some include Norway and candidate countries).
- Aggregate rows such as an EU total. Remove them before joining.
- Country codes may not match across tables (Eurostat uses its own codes, for example for Greece). Check with `anti_join()`.
- Eurostat marks missing values with `:`. Declare it when reading the file, as class 4 does: `read_csv(na = c("", ":", "NA"), show_col_types = FALSE)`. Report how many are missing, do not replace them.
- Eurostat can add flags such as `p` (provisional) or `e` (estimated) to some numbers. That can turn a number column into text. Check every column type with `glimpse()`.

---

## 4. Folder structure

```
ai-adoption-story/
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
│   └── img/story.png
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
title: "What goes with AI use in EU enterprises?"
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
# 1. Question
# 2. Data
# 3. Cleaning
# 4. Combining the tables
# 5. Exploration
# 6. The story
# 7. Limits
```

Pattern inside every section:

1. **What I do and why:** two to five sentences, first person, plain words.
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
ai_panel <- ai |> left_join(ict, by = join_by(country_code))
```
````

Render with `quarto render analysis.qmd`.

When Sergio's requirements arrive, the headings can be renamed or regrouped without changing the code.

---

## 5. Workflow: eight steps

Each step has a task and a "done when", and points to the class that taught the tools.

### Step 1. Ask
- **Task:** write the question and the one-sentence expected message in section 1 of `analysis.qmd`. Also write which indicator I expect to line up most strongly with AI use, and why. No code in this step.
- **Done when:** both are written, before any code runs on the data. If the course asks for the question to be registered somewhere, do that too (check with Sergio).

### Step 2. Get the data
- **Task:** download the five CSVs into `data-raw/`. Record filters in `notes/decisions.md`. Import each file with `read_csv(na = c("", ":", "NA"), show_col_types = FALSE)` and check the column types with `glimpse()`.
- **Done when:** each file imports with `read_csv()`, `clean_names()` and `glimpse()` without errors.

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
- **Task:** keep EU member states and the target year, reshape if needed (`pivot_longer()`), rename to short column names, join the five tables on country code. After each join, check row counts and run `anti_join()` to list countries that did not match. Save the result to `data/ai_panel.csv`.
- **Target columns:** `country_code`, `country`, `ai_use_pct`, `ict_specialists_pct`, `digital_skills_pct`, `rd_pct_gdp`, `gdp_pps_index`, `baltic` (TRUE for EE, LV, LT).
- **Done when:** one row per country, no duplicates, missing values documented, row counts explained.

**Possible course checkpoint (from the course README, confirm that it applies): Fri 13 Nov 2026, 23:59.** Five lines that load the clean data and run without errors:

```r
pacman::p_load(tidyverse, here)
here("data", "ai_panel.csv") |>
  read_csv(show_col_types = FALSE) |>
  glimpse() |>
  nrow()
```

### Step 5. Explore (class 3 chart by type, class 10 chart by question)
- **Task:** one histogram per variable. One scatter plot of `ai_use_pct` against each indicator, same y axis, using `facet_wrap()`. Add `geom_smooth(method = "lm")`. Report the correlation for each pair with `cor()` (new function, explain it first, including what the number means and how missing values are handled). If I decide against `cor()`, describe the pattern in words and drop `correlations.json`. Check each outlier: data error or real value? Write one sentence per outlier.
- **Keep a "dead ends" list** in `notes/decisions.md`: relationships I expected that were weak, and what I did about it.
- **Look at GDP on purpose.** If every indicator goes with GDP as well as with AI use, say that they may be stand-ins for wealth.
- **Done when:** I can name the strongest indicator, the weakest, one surprise, and whether GDP explains most of the pattern.

### Step 6. Tell the story (Show, Cut, Focus, Tell; classes 11, 12, 13)
- **Task:** build one scatter plot.
  - Highlight the Baltics the way class 11 does, using the `baltic` column: `scale_color_manual(values = c("TRUE" = "firebrick", "FALSE" = "gray70"))`, `theme_minimal(base_size = 14)`, and `theme(legend.position = "none", panel.grid.minor = element_blank())`. Labels with `ggrepel::geom_text_repel()`, the note with `annotate("text", ...)` (class 12). Viridis stays for charts with several groups.
  - Title states the finding. Example shape: "Latvia has [more/fewer] AI-using firms than its ICT workforce predicts".
  - One annotation pointing at the evidence (label the three Baltic points, or one outlier).
  - Source line, axis labels with units, no legend or grid line without a job, no border.
  - Honest scales: label the axes clearly, say if an axis does not start at zero.
- **Checklist before export:**
  - [ ] Ask: the title answers the question.
  - [ ] Show: right chart for two numbers, honest scales, data quality applied.
  - [ ] Cut: no non-data ink.
  - [ ] Focus: gray plus one color.
  - [ ] Tell: finding title, one annotation, source.
- **Export:** `ggsave(here("figures", "story.png"), width = 8, height = 5.5, dpi = 300)`.

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
- The final story graph is the ggplot2 image exported from R, so the course's Cut, Focus and Tell principles are applied with the course's own tool.

### Data flow

```
analysis.qmd  ->  R/99_export_for_app.R  ->  app/data/*.json + app/img/story.png
                                                              |
                                          app/index.html reads the JSON with fetch()
                                                              |
                                                           Vercel
```

| File written by R | Content | Used in section |
|---|---|---|
| `app/data/ai_panel.json` | Clean table, one object per country | Exploration, The story |
| `app/data/quality.json` | Per table: rows, columns, missing values, duplicates, range of the key variable, my decision | Cleaning |
| `app/data/joins.json` | Row counts before and after each join, countries listed by `anti_join()` | Combining the tables |
| `app/data/correlations.json` | Correlation of AI use with each indicator, plus n | Exploration |
| `app/data/code.json` | R code per step, extracted from `analysis.qmd` | All |
| `app/data/steps.json` | Two to four sentences per step on the logic, shortened from the "What I do and why" text in `analysis.qmd` | All |
| `app/img/story.png` | The final ggplot, 300 dpi | The story |

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
- **Exploration:** a dropdown to switch the indicator on the x axis. Scatter plot with a hover tooltip (country, both values). A toggle to highlight the Baltics. The correlation and n for the selected indicator.
- **The story:** the final image only. No controls, no extras. This is the part where the course's Cut and Focus principles show.

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
2. Load `ai_panel.json` (dummy data first) and draw the scatter plot.
3. Add the info button with code from `code.json`.
4. Add the indicator dropdown and the Baltic highlight.
5. Add the quality and joins tables.
6. Add the story image and polish.

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
| 4:30 to 6:00 | Explore | Switch the indicator live with the dropdown, one dead end, why I picked the winner | App, Exploration section |
| 6:00 to 8:00 | Story | The final graph: read the title, point at the annotation, say what the Baltics show | App, The story section |
| 8:00 to 9:00 | Limits | Association not cause, about 27 countries, company size mix | App, Limits section |
| 9:00 to 10:00 | Wrap | What the method shows, what I would do next, questions | App |

Rules for the talk:

- Open the info button at most twice, on the two moments where the logic matters most (joins and the final graph).
- Keep `analysis.qmd` open in Positron next to the app, to show the text-then-code structure.
- Live interaction only in the Explore part. The Story part shows one still graph.
- Say the finding in one sentence at the start of the Story part and again at the end.
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

- Data years differ by table. R&D and GDP are probably one year older than the AI data (2024 against 2025). Say so in the source line.
- Join problems (country codes, aggregate rows) are likely. Budget time for them.
- With about 27 countries, one outlier can move the trend line. Show the line with and without it if it matters.
- Company size mix may explain part of the country differences: in the EU in 2025, large enterprises used AI far more than small ones. Mention it as a caveat, no modelling needed.
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
