# Project: What goes with AI use in EU enterprises?

Course: Data visualization and storytelling (SZF_184), RSU, autumn 2026. Lecturer: Sergio Uribe.
Student: Jolanta. Language of the project: English.
Tools: Positron, R, Quarto, Claude Code. Deliverables: an R analysis with a Quarto report, plus an interactive data app in plain HTML, CSS and JavaScript hosted on Vercel.

Put this file in the project root as `CLAUDE.md` so Claude Code reads it at the start of every session.

---

## 1. How Claude Code should work with me

I do the analysis myself. This is graded work, and I must be able to explain every line on the day.

Default mode is **coach, not ghostwriter**:

- Explain the next step in plain words before any code. Say which class topic it comes from.
- Give hints and small code skeletons with blanks (`___`), like the course worksheets. Do not hand me a finished pipeline unless I ask for it by name.
- Ask me to predict what a line will print before I run it.
- After I write code, review it. Say what works, what could break, and why.
- Use only what the course has taught: `glimpse()`, `summary()`, `filter()`, `select()`, `mutate()`, `case_when()`, `pivot_longer()`, `pivot_wider()`, the joins (`left_join()`, `inner_join()`, `full_join()`, `anti_join()` with `join_by()`), `group_by()` with `summarize()`, `count()`, `ggplot2` (`geom_point()`, `geom_histogram()`, `geom_smooth(method = "lm")`, `facet_wrap()`), `janitor::clean_names()`. If a new function would help, name it, explain it in two sentences, and let me decide.
- Never edit files in `data-raw/`. Never replace `NA` with 0.
- If I say "write it for me", do it, then explain each line afterwards.
- **Exception for the data app:** HTML, CSS and JavaScript for the Vercel app are not course content and not graded as analysis. Claude Code may write that code, with short comments, and explain each part when it is done. All R analysis code stays in coaching mode.

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

> [One sentence. Write it myself on 30 Oct. Examples of the shape: "Countries with more ICT specialists have more AI-using firms, but Latvia is an exception." or "No single indicator explains AI use."]

Starting hypotheses, not results:

1. More ICT specialists in the workforce goes with more AI-using firms.
2. A more digitally skilled population goes with more AI-using firms.
3. Higher R&D spending goes with more AI-using firms, but this may only reflect wealth.
4. Early hint to check: Latvia may have a high share of ICT specialists but low AI use.

---

## 3. Data

Four Eurostat tables, joined on country code. Download each as CSV from the Eurostat data browser into `data-raw/`.

| Short name | Eurostat code | Variable I want | Year target | Status |
|---|---|---|---|---|
| AI use | `isoc_eb_ai` | % of enterprises using at least one AI technology, all sizes, all sectors | 2025 (2024 for context) | to confirm on download |
| ICT specialists | `isoc_sks_itspt` | ICT specialists, % of total employment | 2025 | to confirm on download |
| Digital skills | `isoc_sk_dskl_i21` | Individuals with at least basic digital skills (`I_DSK2_BAB`), % of individuals | 2025 | to confirm on download |
| R&D | `rd_e_gerdtot` | R&D expenditure, % of GDP, all sectors | latest year, probably 2024 | to confirm on download |

Optional fifth and sixth tables, only if time allows: GDP per capita (confirm the table code first), SME digital intensity (`isoc_e_dii`).

Rules:

- Write down the exact filter labels I choose on the Eurostat site (indicator, size class, sector, unit) in `notes/decisions.md`. A reader must be able to reproduce the download.
- The AI table covers enterprises with 10 or more employees. State this in the app.
- If R&D is a year older than the others, say so in the final graph's source line.
- Name the source on every chart: "Source: Eurostat, [codes], accessed [date]".

Things to expect in cleaning:

- Different country coverage per table (some include Norway and candidate countries).
- Aggregate rows such as an EU total. Remove them before joining.
- Country codes may not match across tables (Eurostat uses its own codes, for example for Greece). Check with `anti_join()`.
- Eurostat marks missing values with symbols or flags. Report how many, do not replace them.

---

## 4. Folder structure

```
ai-adoption-story/
├── CLAUDE.md
├── .here                  # empty file so here() finds the project root
├── _quarto.yml
├── index.qmd              # the message and the final graph
├── 01-question.qmd
├── 02-data.qmd
├── 03-cleaning.qmd
├── 04-manipulation.qmd
├── 05-exploration.qmd
├── 06-story.qmd
├── 07-limits.qmd
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
    └── decisions.md       # every choice and why
```

Code folding in `_quarto.yml` (this is the info button in the Quarto report; the Vercel app has its own, see section 6):

```yaml
format:
  html:
    code-fold: true
    code-summary: "Show the code"
    embed-resources: false
```

For the single-file submission, render once with `embed-resources: true` into a separate output.

---

## 5. Workflow: eight steps

Each step has a course link, a task, and a "done when".

### Step 1. Ask (course: Ask principle, class 15)
- **Task:** write the question and the one-sentence expected message in `01-question.qmd`.
- **Done when:** both are written, and the question is registered in Moodle (due Fri 30 Oct).

### Step 2. Get the data (class 15 data import, readings: Ellis and Leek, R4DS ch. 7)
- **Task:** download the four CSVs into `data-raw/`. Record filters in `notes/decisions.md`.
- **Done when:** each file imports with `read_csv()`, `clean_names()` and `glimpse()` without errors.

### Step 3. Clean (class 3 data quality checklist, classes 4 and 6)
- **Task:** for each table run the five checks and write one comment line per check.

| # | Check | How |
|---|---|---|
| 1 | Types | `glimpse()` |
| 2 | Missing values | `summary()` |
| 3 | Units and ranges | `summary()`, histogram |
| 4 | Duplicates | `distinct()` and `count()`, compare |
| 5 | Row counts | `count()` before and after every filter |

- **Done when:** each table has a short "what I found, what I decided" note, and one table is shown in the app as a quality report.

### Step 4. Manipulate (classes 5, 7, 8, 9)
- **Task:** keep EU member states and the target year, reshape if needed (`pivot_longer()`), rename to short column names, join the four tables on country code. After each join, check row counts and run `anti_join()` to list countries that did not match. Save the result to `data/ai_panel.csv`.
- **Target columns:** `country_code`, `country`, `ai_use_pct`, `ict_specialists_pct`, `digital_skills_pct`, `rd_pct_gdp`, `baltic` (TRUE for EE, LV, LT).
- **Done when:** one row per country, no duplicates, missing values documented, row counts explained.

**Moodle checkpoint, Fri 13 Nov 2026, 23:59.** Submit about five lines that load the clean data and run without errors:

```r
pacman::p_load(tidyverse, here)
here("data", "ai_panel.csv") |>
  read_csv(show_col_types = FALSE) |>
  glimpse() |>
  nrow()
```

### Step 5. Explore (class 3 chart by type, class 10 chart by question)
- **Task:** one histogram per variable. One scatter plot of `ai_use_pct` against each indicator, same y axis, using `facet_wrap()`. Add `geom_smooth(method = "lm")`. Report the correlation for each pair. Check each outlier: data error or real value? Write one sentence per outlier.
- **Keep a "dead ends" list** in `notes/decisions.md`: relationships I expected that were weak, and what I did about it.
- **Done when:** I can name the strongest indicator, the weakest, and one surprise.

### Step 6. Tell the story (Show, Cut, Focus, Tell; classes 11, 12, 13)
- **Task:** build one scatter plot.
  - Gray points for all countries, one color for EE, LV, LT. Viridis if more colors are needed.
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
- **Report (safe submission):** the Quarto pages from Steps 1 to 6, rendered with code folding. Also render one self-contained HTML file (`embed-resources: true`). This is the version that must run from top to bottom.
- **Data app:** follow section 6. R exports the data, the app displays it.
- **Done when:** the report renders with no errors, the app is live on a Vercel link, and the numbers in the app match the report.
- **Ask Sergio** whether a web app is accepted as the deliverable, and in which form he wants the script.

### Step 8. Polish and rehearse
- Run the rubric checklist (section 8).
- Write the 60-second script and the 10-minute talk (section 7).
- Rehearse twice with a timer. Prepare an offline backup (see section 6).

---

## 6. The Vercel data app

**What it is:** a small interactive web app in plain HTML, CSS and JavaScript, hosted on Vercel. It walks the reader through the eight steps. Each step shows the logic, the result, and an info button that reveals the R code that produced it. R does all the analysis. The app only displays what R exported.

**Why this split:**

- The graded analysis stays in R, with course tools, and runs top to bottom.
- The code shown in the app is extracted from my real `.qmd` files, so it cannot drift from what actually ran.
- No framework, no build step, no npm packages. Fewer things to break before the deadline.
- The final story graph is the ggplot2 image exported from R, so Cut, Focus and Tell are judged on the course's own tool.

### Data flow

```
analysis in .qmd files  ->  R/99_export_for_app.R  ->  app/data/*.json + app/img/story.png
                                                              |
                                          app/index.html reads the JSON with fetch()
                                                              |
                                                           Vercel
```

| File written by R | Content | Used in step |
|---|---|---|
| `app/data/ai_panel.json` | Clean table, one object per country | Explore, Story |
| `app/data/quality.json` | Per table: rows, columns, missing values, duplicates, range of the key variable, my decision | Clean |
| `app/data/joins.json` | Row counts before and after each join, countries listed by `anti_join()` | Manipulate |
| `app/data/correlations.json` | Correlation of AI use with each indicator, plus n | Explore |
| `app/data/code.json` | R code per step, extracted from the `.qmd` files | All |
| `app/data/steps.json` | Two to four sentences per step on the logic, written by hand | All |
| `app/img/story.png` | The final ggplot, 300 dpi | Story |

**Code extraction.** Mark the code to show with comments inside the chunks:

```r
# >>> show: join
ai_panel <- ai |> left_join(ict, by = join_by(country_code))
# <<< show
```

`99_export_for_app.R` reads the `.qmd` files, collects the blocks by name, and writes `code.json`. It uses `jsonlite`, which the course has not covered, so Claude Code should explain that script line by line.

### Page layout

One page that scrolls, with a sticky navigation for the eight steps. Each step has four parts:

1. A heading that says what happened, not only the topic.
2. The logic: two to four sentences on why I did it.
3. The result: a table or a chart.
4. An info button ("Show the R code") that opens the code block from `code.json`, with a copy button.

### Interactions (keep to this list)

- **Clean:** a table of the five checks per dataset. Click a dataset to see my notes.
- **Manipulate:** row counts before and after each join, and the list of unmatched countries.
- **Explore:** a dropdown to switch the indicator on the x axis. Scatter plot with a hover tooltip (country, both values). A toggle to highlight the Baltics. The correlation and n for the selected indicator.
- **Story:** the final image only. No controls, no extras. This is the part the rubric scores for Cut and Focus.

### Design rules

- Same colors as the ggplot: gray for all countries, one color for the Baltics. Pick one viridis color and use the same hex code in R and in the CSS.
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

1. Skeleton page with eight empty sections and navigation. Deploy it.
2. Load `ai_panel.json` (dummy data first) and draw the scatter plot.
3. Add the info button with code from `code.json`.
4. Add the indicator dropdown and the Baltic highlight.
5. Add the quality and joins tables.
6. Add the story image and polish.

### Fallback

If the app does not work end to end by **Thu 12 Nov**, the Quarto report becomes the main deliverable, and the app is shown only if it is ready. The R analysis is the graded core. The app is the way it is delivered.

---

## 7. The 10-minute presentation

I show three things live: the **data**, the **code**, and the **app**.

| Time | Part | What I show | Where |
|---|---|---|---|
| 0:00 to 0:45 | Ask | The question, the expected message, why it matters | App, question page |
| 0:45 to 1:45 | Data | The four sources, one raw CSV, what each variable means | Positron data viewer, then app |
| 1:45 to 3:15 | Clean | The five checks, one real problem I found, my decision | App, quality table. Open the code fold once |
| 3:15 to 4:30 | Manipulate | The joins, the `anti_join()` result, row counts before and after | App, then the pipeline code |
| 4:30 to 6:00 | Explore | Switch the indicator live with the dropdown, one dead end, why I picked the winner | App, exploration page |
| 6:00 to 8:00 | Story | The final graph: read the title, point at the annotation, say what the Baltics show | App, home page |
| 8:00 to 9:00 | Limits | Association not cause, about 27 countries, company size mix | App, limits page |
| 9:00 to 10:00 | Wrap | What the method shows, what I would do next, questions | App |

Rules for the talk:

- Open the info button at most twice, on the two moments where the logic matters most (joins and the final graph).
- Live interaction only in the Explore part. The Story part shows one still graph.
- Say the finding in one sentence at the start of the Story part and again at the end.
- Offline backup: the app running from a local server on the laptop, the self-contained HTML report, the PNG of the final graph, and screenshots of each step. Wi-Fi fails at the worst time.

**3-minute version** (the course capstone format, in case Sergio requires it):

- 0:00 to 0:20: question and message.
- 0:20 to 1:00: data and the quality checks, one sentence each.
- 1:00 to 2:40: the story graph, title, annotation, Baltics.
- 2:40 to 3:00: limits and close.

Confirm with Sergio which length applies.

---

## 8. Rubric checklist (course capstone rubric, 100 points)

Source: `capstone/rubric.md` in the course repo. Update this when the official criteria for my final work arrive.

| Principle | Points | What I show |
|---|---|---|
| Ask | 20 | Clear question, data can answer it, one-sentence message |
| Show | 25 | Right chart type, honest scales, tidy data, data quality checklist reported, script runs |
| Cut | 15 | No non-data ink, no legend, border or grid line without a job |
| Focus | 15 | Gray plus one color, the eye goes to the message |
| Tell | 25 | Finding in the title, one annotation, beginning, middle and end |

Final check before submitting:

- [ ] The project renders from top to bottom with no errors.
- [ ] `data-raw/` is untouched.
- [ ] Row counts are explained at every join.
- [ ] The limits are stated plainly.
- [ ] The source is named on the final graph.
- [ ] The deployed link works on another device.

---

## 9. Timeline

Dates follow the course README (classes may move, so recheck the schedule). Today is Thu 29 Oct.

| Date | Do | Course event |
|---|---|---|
| Thu 29 Oct | Step 1: question and expected message. Start Step 2: download data | |
| Fri 30 Oct | Step 2 done: all four files import | Class 15: question in Moodle, import script and tidy check |
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

- Data years differ by table. R&D may lag one year. Say so in the source line.
- Join problems (country codes, aggregate rows) are likely. Budget time for them.
- With about 27 countries, one outlier can move the trend line. Show the line with and without it if it matters.
- Company size mix may explain part of the country differences: in the EU in 2025, large enterprises used AI far more than small ones. Mention it as a caveat, no modelling needed.
- App scope: JavaScript is not part of the course. Keep the app to the interactions listed in section 6. Every extra feature is a risk to the deadline.
- Open question: is a Vercel app accepted as the deliverable? Ask Sergio.
- Open question: what exactly is due on 16 Nov, and how long is the talk?

---

## 11. Course reference

Five principles: **Ask** (question and one-sentence message before code), **Show** (tidy data, right chart, honest scales), **Cut** (remove every mark that does not carry data), **Focus** (gray plus one color), **Tell** (title states the finding, add an annotation).

Chart by data type: 1 number, histogram. 2 numbers, scatter. 1 category, bar of counts. Number by category, box plot. Time, line. More variables: color, shape, size, facet. No 3D.

Bars start at zero. Lines may leave zero if the range is stated.

Palette: viridis by default. Gray plus one color to highlight one group.

Course repo: https://github.com/sergiouribe/Data_Visualization_Course_RSU
