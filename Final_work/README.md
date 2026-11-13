# Why do Baltic men die earlier?

Final project for SZF_184 Data Visualisation and Storytelling (RSU, autumn 2026, Sergio Uribe). Jolanta Stutane.

**Finding:** at the same income, Baltic women live as long as other Europeans, but Baltic men live up to 4.6 years shorter (Latvia, 2024).

## Open and run

1. Open `final_project.Rproj` in RStudio or Positron. The project file sets the folder as the root, so `here()` finds every path.
2. Open `stutane_final_project.qmd` and click **Render** (or run `quarto render stutane_final_project.qmd`). It renders from a clean session. Missing packages, `pacman` included, are installed by the first chunks.

## What is in this folder

| Item | Content |
|---|---|
| `stutane_final_project.qmd` + `.html` | The report: question, data, cleaning, combining, exploration, every version of the final graph with a log, the answer and the limits. The `.qmd` holds the overview and puts the section files below together |
| `_1_setup.qmd` to `_7_limits.qmd` | The section files of the report (16 files). Each can also be run on its own |
| `data/raw/` + `SOURCE.md` | The six original downloads (Eurostat and WHO via Our World in Data), never edited, with source, link, licence and download date |
| `data/clean/` | The clean tables and the combined panel `gap_panel.csv`, created by the code |
| `figures/` | `graph_v1.png`, `graph_v2.png`, `final_graph.png` and `colour_blind_check.png` |
| `story.md` | One page: the question, the path to the finding, the answer, the final graph, evidence and limits |
| `prompt_log.md` | How I worked with AI, prompt examples, decisions, and the AI mistakes I caught |
| `talk.qmd` + `.html` | Video 1: the talk, as slides. Press `S` in the browser for the speaker notes |
| `demo.qmd` + `.html` | Video 2: a code walkthrough that runs the report's own code, from the raw download to the final graph. Press `S` for the notes |
| `notes/` | My decisions, a glossary of functions, the talk and demo scripts, and working notes |
| `Project_idea.md` | The project plan and progress |
