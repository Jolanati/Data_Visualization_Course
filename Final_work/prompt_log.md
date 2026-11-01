# Prompt log

**Tool:** Claude Code (Anthropic), inside VS Code, from 29 October 2026.
**Used for:** the whole project, as a guide that writes R code in small steps and explains it. I run every chunk myself, check the output and make every analysis decision.

The full, word-for-word history of every session is saved by Claude Code on my laptop and can be shared on request. This log describes how I worked, shows typical prompts, lists the decisions and the AI mistakes I caught.

## 1. How I work with AI

1. **First, my project idea.** I wrote my project idea in a Markdown file (`Project_idea.md`): my question, my expected message, the data I wanted and the rules for how the AI has to work with me. Then I asked Claude to read it and get acquainted with it, so it knows my project before writing any code. Claude reads it again at the start of every session and continues from the Progress section.
2. **I say what it needs to do, Claude writes it.** The code is built one small chunk at a time. For each chunk I first say in my own words what it needs to do, for example: "Now we need to keep only the EU countries." Claude writes the chunk.
3. **Claude explains the technical side: which functions it uses.** What each function does, what its arguments mean, and why this approach and not another. If I don't understand, Claude explains it again in simpler words or with an everyday example (for example, `read_csv()` is like typing a printed table into Excel while the printout stays in the drawer).
4. **Claude checks that I understood, with a question.** Before I run the chunk, Claude asks me one question about it, for example: "If Eurostat had written Greece as `GR` instead of `EL`, what would the row count print?" Some answers I got wrong, and Claude corrected me and explained why, which helped me understand the code better. Then I run the chunk in Positron, and we only talk about the output when something is unexpected.
5. **I decide, the AI suggests.** For every analysis decision (which countries, what to do with missing years, which columns to keep) Claude gives the options with a recommendation, and I choose. Claude is not allowed to decide silently.
6. **Every decision is written down twice.** In the report (`final_project.qmd`), in academic voice, and in `notes/decisions.md`, in my own words, for the talk.
7. **New functions go into a glossary.** In `notes/glossary.md`, each one with a one-line meaning and a small example, so I can revise before the defence.
8. **Comments for my future self.** Every line of code has a comment that says what it does and why, so I can still read it in 10 years (Sergio's rule, which I added to the plan).
9. **Fixed rules for the AI.** Never edit the raw data, never replace missing values with 0, never add itself as co-author in my commits.

## 2. What I ask: prompt examples

Copied word for word, typing mistakes included.

**Understanding the project and setting the structure**

> Please get aquinted with the content of folder ''Final work'', there is my final R project idea

> I would like the structure of the Analysis qmd to be similar to one I did in ML - https://github.com/Jolanati/wine-dine/blob/main/wine-dine/wine-dine.ipynb Where all ise logical, clear and easy to understand

**Asking for the next step**

> Ok, that is not so bing issue, so let;s keep working with code, what is the next one small logical step according to notbook structure, we need to do?

**Asking for better output**

> ok, now in sections 2.1 and 3.1 maybe there is visually better way tgo show result, maybe graphs, more neater tables?

> 3.1.2 can we have it colors not grey?

**Asking to understand**

> what is technical columns?

> life_expectancy_clean and life_expectancy_raw are objects?

> So, practcally we turn the CVS into R readable format?

**Asking for a recommendation, then deciding**

> would you suggest dropping flag or not?

> Qhat would be jo suggestion for LUx?

**Challenging the AI on data bias**

> Maybe we take 10? But if we take 10, how we decide? which 10, and should not we state that in our decisions etc, that we did not use alll 93, as it is a lot of data?

> But if I premptively cohoose datat that will support my hypothesis, arent I hetting biased by design?

> Maybe, we goo with all 93 causes, we look on the 17 chapters first, and than, in the later data analysis, we look deeper in that?

> I would not download additional data, as we dont need them

**Correcting how the AI works with me**

> I believe we are moving too slow, maybe, for each code chunk, you explain to me what and why we do, before I run it? and ask me do I understand it?

> Don't rush, what is next question>

> One reminder, my professour tought us to write comments in the code, so that if after a 10 years you open your code, you can understand what ypu were doing

## 3. What we decided

Who suggested what, and what I chose. The full reasons are in `notes/decisions.md`.

| Date | Decision | AI suggested | I decided |
|---|---|---|---|
| 29 Oct | Topic and question | *[to fill in: what Claude suggested in the first session]* | The Baltic men question (the AI topic was dropped), my expected message written before the data |
| 30 Oct | Report structure | Follow my wine-dine notebook | Yes, plus a "found, decided" summary after every cleaning step |
| 30 Oct | Where to remove non-EU countries | Section 3 (cleaning) or Section 4 (combining), recommended 3 | Section 3 |
| 30 Oct | Keep the `flag` column | Keep it, it is cheap to drop later | Keep it, decide again before the join |
| 30 Oct | Charts in the quality checks | Gray charts, red kept for the Baltics | Colour (viridis), still no red outside the Baltics |
| 30 Oct | Tables | `tbl_summary()` from the course | Yes, plus `knitr::kable()` for small tables |
| 31 Oct | Luxembourg has no healthy life years for 2023 and 2024 | Three options, recommended keeping it with the gap | Keep it with the gap, nothing filled in |
| 31 Oct | Packages | Add `patchwork` later, when needed | Add it now, all packages load at the start |
| 1 Nov | Folder names | Rename to Sergio's template | Yes |
| 1 Nov | Section order in the report | Keep mine, add a map to the template | Yes |
| 1 Nov | The web app | Optional, as it is not required | Keep it as my presentation tool |
| 1 Nov | Which causes of death to download (the table has 93) | 6 causes named by my hypotheses, or 10 with smoking, suicide and accidents added, chosen by hypothesis, not by data | I asked whether choosing causes that fit my hypotheses makes me biased by design. Decided: download all 93, look first at the non-overlapping chapters of causes without choosing anything, then deeper at my hypothesis causes. Causes that don't matter go into the dead ends |
| 1 Nov | Which countries in the deaths table (it also has about 460 regions, 55 MB) | Put the 27 EU countries in the link (3.5 MB), as an exception to the 30 Oct rule | Yes. I don't download data I don't need |
| 1 Nov | Two codes for "all causes" in the deaths table (`TOTAL` and `A-R_V-Y` in different years) | Merge them into `TOTAL`, the safest of three options | Yes |
| 1 Nov | How to mark the chapters of causes | A column `cause_level` (all causes, chapter, detail), keeping all rows | Yes, after Claude checked that the chapters add up to all causes |
| 1 Nov | Luxembourg and Ireland far above all others in GDP | Keep both as real values, check every GDP chart with and without them | Yes |
| 1 Nov | The `flag` column before reshaping | Drop it, the flags are already described in Section 3 | Yes |
| 1 Nov | Deaths: 92 causes in one panel, or two tables | Two tables: `gap_panel` with all causes, `deaths_by_cause` for the detail | Two tables |
| 1 Nov | The gap in deaths: difference or ratio | Difference, men minus women, because a ratio breaks when women have 0 deaths | Difference |
| 1 Nov | Which join | `left_join()` onto life expectancy, so no country-year is lost | `left_join()` |
| 1 Nov | Country names | Add English names, written once in a small table | Yes |

## 4. AI mistakes I caught

**1. A wrong row count.** Before I loaded the healthy life years file, Claude said to expect "about 617 rows". My `glimpse()` output said `Rows: 618`. I trusted the output, not the AI. Claude then explained its mistake: the file has 619 lines, and one of them is the header, so 618 rows.

**2. A decision written in the notes but not in the report.** Claude logged the removal of the technical columns in `notes/decisions.md`, but the summary table in the report only said "the technical columns are removed", without naming them. I asked: "Should not that also be in 3.1.3?" Now the report names all six columns and says why each one goes.

**3. Code before the decision.** Claude was ready to write the code that removes columns before the reason was recorded. I asked: "removing technical columns also that is in decisions first?" Since then the order is always: decide, write it down, then code.

**4. A selection that could bias the analysis.** For the deaths table, Claude suggested downloading only 6 (or 10) of the 93 causes of death, chosen because my hypotheses name them. I asked: "But if I premptively cohoose datat that will support my hypothesis, arent I hetting biased by design?" Claude agreed that a hand-picked list has a blind spot: if the real driver were a cause I hadn't thought of, I would never see it. We changed the design: all 93 causes are downloaded, the analysis first looks at the chapters of causes without choosing anything, and only then at my hypothesis causes.

**5. A misleading quiz question about `case_when()`.** In 3.3.4, Claude asked me what would happen if `.default = "detail"` came first in `case_when()`. The question was wrong: `.default` is always used last, for rows no rule matched, wherever it is written. What matters is the order of the rules with `~`, because the first true rule wins. This time Claude noticed the mistake itself, in the next step, and corrected it before I answered. It reminded me that the AI's questions can be wrong too, not only its answers.

**6. Too many decisions at once.** When Sergio's template arrived, Claude listed four decisions in one message and pushed for answers. I said "Don't rush" and we now go through them one at a time.
