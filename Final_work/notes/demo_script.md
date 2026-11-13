> **Update 12 Nov:** video 2 is now the slide deck `demo.qmd` / `demo.html`: the whole pipeline (gather, clean, combine, explore, final graph) with the real code from the report and its output. The script is in the speaker notes: open `demo.html` and press S. The text below is the earlier version, kept for reference.

# Video 2 script: code walkthrough of the final graph

About 6 to 7 minutes, around 900 spoken words. Open `demo.qmd` in Positron, camera on, screen shared. Before recording, run the first chunk once in a fresh session so the packages are already loaded.

For each part: **Show** is what to do on the screen, **Say** is what to tell. Run a chunk with `Ctrl+Shift+Enter` while the cursor is inside it, and wait for the output before you talk about it.

---

## Intro · about 30 seconds

**Show:** the top of `demo.qmd`, the title.

**Say:**

> Hi, I'm Jolanta. I'm travelling on the day of the presentations, so in this second video I show the code behind my final graph.
>
> My finding is: at the same income, Baltic women live as long as other Europeans, but Baltic men live up to 4.6 years shorter.
>
> The full analysis, with six data tables, cleaning and seventeen exploration graphs, is in my main report. Here I build only the final graph, in five steps, so I can explain every line.

---

## Step 1 · Packages and the clean data · about 45 seconds

**Show:** scroll to Step 1, run the chunk. Point to the output `270 18`.

**Say:**

> First I load two packages with `pacman::p_load()`. It installs a package if it's missing and then loads it, so the file runs on any computer. Tidyverse is for working with data and for the charts, and `here` builds file paths from the project folder, so I never type a full path like C colon users.
>
> Then `read_csv()` reads my clean panel. I saved it in Section 4 of the report, after cleaning and joining all the tables. `show_col_types = FALSE` just keeps the output short.
>
> `dim()` shows the size: 270 rows and 18 columns. That's 27 EU countries times 10 years, one row per country and year. That's what I expected, so the file loaded correctly.

---

## Step 2 · The year and the comparison group · about 1 minute

**Show:** run the Step 2 chunk. Point to the table with the ten countries.

**Say:**

> Now I keep only 2024, the latest year, with `filter(year == 2024)`. That gives one row per country.
>
> The Baltic states are poorer than most of the EU. If I compared them with the EU average, I would mix up income and health. So I compare them only with countries that are about as rich.
>
> That's the second `filter()`. The exclamation mark means "not", so `!baltic` keeps every country that is not a Baltic state. Then I keep only countries with a GDP index between 60 and 90. The index is GDP per person, where the EU average is 100.
>
> The result is these ten countries: Bulgaria, Croatia, Czechia, Greece, Hungary, Poland, Portugal, Romania, Slovakia and Slovenia. This is my comparison group.

---

## Step 3 · The difference for women and men · about 1 minute

**Show:** run the Step 3 chunk. Point to the six rows of output, especially Latvia, Men, minus 4.6.

**Say:**

> Here I compute the main number of the graph. I take the three Baltic states with `filter(baltic)`.
>
> Then `mutate()` creates two new columns. For women, it's the Baltic country's life expectancy minus the average of the ten comparison countries. The same for men. Zero means "the same as in equally rich countries", a negative number means shorter lives.
>
> `select()` keeps only the country and these two columns.
>
> Then `pivot_longer()` puts women and men into one column called `sex`, and the numbers into one column called `difference`. I need this long shape because ggplot draws one panel per value of `sex`.
>
> The last line, `factor()` with `levels`, sets the order, so women come first and men second.
>
> In the output you can already see the finding: the women's numbers are close to zero, and Latvian men are at minus 4.6.

---

## Step 4 · Why not plain bars? · about 40 seconds

**Show:** run the Step 4 chunk. Point to the bars, which all look about the same height.

**Say:**

> This was my first version. It shows total life expectancy, women and men side by side, with `geom_col()` and `position = "dodge"`.
>
> The problem is easy to see. Bars must start at zero, so a difference of four or five years on an 80-year bar is only a few millimetres. You can't see the finding. The title only names the topic, and the legend shows the code names of my columns.
>
> That's why the final graph shows the difference, not the total. That change in the measure is the most important step in the whole graph.

---

## Step 5 · The final graph, layer by layer · about 2 and a half minutes

**Say:**

> Now I build the final graph one layer at a time, so you can see what each part adds.

### 5a · Bars and panels

**Show:** run 5a.

**Say:**

> `fct_reorder()` sorts the countries by their lowest value, which is the men's, so Latvia, with the largest shortfall, is on top.
>
> `geom_col()` draws the bars, the length is the difference in years. `geom_hline()` draws the line at zero. `coord_flip()` turns the bars sideways, so the country names are easy to read. And `facet_wrap(~ sex)` makes one panel for women and one for men.
>
> I save the graph in an object called `p`, so I can add layers to it step by step.

### 5b · Gray plus one colour

**Show:** run 5b.

**Say:**

> `scale_fill_manual()` sets the colours: gray for the women and firebrick red for the men. That's the course rule "gray plus one colour", so the eye goes straight to the men.
>
> I checked it in a colour blindness simulator: the red is much darker than the gray, so the two stay apart even for people who can't see red.
>
> `theme_minimal()` with `base_size = 14` gives a clean background and text that's large enough for a screen. And I remove the legend, because the panel titles already say "Women" and "Men".

### 5c · The numbers

**Show:** run 5c.

**Say:**

> `geom_text()` writes the number at the end of each bar. `sprintf()` with plus point one f writes it with one decimal and always with a sign, so plus 1.1 or minus 4.6.
>
> `if_else()` puts the number to the left of negative bars and to the right of positive ones, so it never sits on top of the bar.
>
> With the numbers, the graph can be read without any colour at all.

### 5d · The annotation

**Show:** run 5d. Point to the red text next to Latvia.

**Say:**

> One annotation points to the strongest evidence. I build it as a small table with `tibble()`, with `sex` set to "Men", so it appears only in the men's panel.
>
> The text says Latvian men live 71.4 years, and men in equally rich countries 76. `sprintf()` takes both numbers from the data, so I never type a result by hand.

### 5e · Title, labels and source

**Show:** run 5e. Let the full final graph stay on screen for a moment.

**Say:**

> Last, `labs()` adds the text. The title states the finding, not the topic. The subtitle says what is compared and the year. The y axis says what is measured, in years. And the caption names the source, Eurostat, with the table codes.
>
> The last `theme()` removes grid lines that carry no data, and makes the panel titles bold.
>
> And that's the final graph.

---

## Optional · Does the result depend on the income band? · about 30 seconds

Only if the video is still under about 6 and a half minutes.

**Show:** run the optional chunk.

**Say:**

> The comparison group is a choice, so I checked it. Here I use a wider band, from 50 to 100. The numbers move, but the pattern stays: Baltic men fall far more behind than Baltic women. In the report I tried three different bands, and the men were always 2 to 4 years further behind than the women.

---

## Close · about 20 seconds

**Show:** scroll back up to the final graph from 5e.

**Say:**

> So that's the code: five steps, from the clean data to the finding. At the same income, Baltic women keep up with Europe. Baltic men don't, and in Latvia they live 4.6 years shorter.
>
> Thank you for watching.
