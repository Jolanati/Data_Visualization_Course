# Glossary

Each new function: one-line meaning and a tiny example.

- `paste0()`: glues pieces of text together with no space between them. `paste0("demo_", "mlexpec")` gives `"demo_mlexpec"`.
- `paste(x, collapse = "+")`: joins all values of a vector into one text, with `+` between them. `paste(c("EE", "LV", "LT"), collapse = "+")` gives `"EE+LV+LT"`. Used to put the 27 EU codes into the deaths link.
- `readLines(file, n = 1)`: reads lines of a text file as text, here only the first one. `readLines(deaths_file, n = 1)` gives the header line of the CSV.
- `startsWith(text, "DATAFLOW")`: `TRUE` if the text begins with that word. `startsWith("DATAFLOW,LAST UPDATE", "DATAFLOW")` gives `TRUE`.
- `file.remove(path)`: deletes a file. Used only to remove a failed download, never a real raw file.
- `stop("message")`: stops the code on purpose and shows the message in red. Better than going on with broken data.
- `download.file(url, destfile, mode = "wb")`: saves a file from the internet to my computer. `"wb"` means copy it byte for byte.
- `file.exists(path)`: `TRUE` if the file is already there, `FALSE` if not. `file.exists(here("data", "raw", "demo_mlexpec.csv"))`.
- `x %in% list`: `TRUE` for each value of `x` that is in the list, `FALSE` otherwise. `"LV" %in% eu_codes` gives `TRUE`, `"NO" %in% eu_codes` gives `FALSE`. Inside `filter()` it keeps the rows that match.
- `knitr::kable()`: shows a small table as a neat table in the rendered report. Display only, the data stays the same. `count(df, flag) |> kable(col.names = c("Flag", "Number of values"))`.
- `tbl_summary(missing = "always")`: adds a "Missing" row to every variable, even when the count is 0. `type = x ~ "continuous2"` lets a number show several statistics on separate lines.
- `geom_vline(xintercept = 65)` / `geom_hline(yintercept = 20)`: a vertical or horizontal reference line on a chart. Use `linetype = "dashed"` so it doesn't look like data.
- `as_labeller(c(F = "Women", M = "Men"))`: gives the facet panels readable titles instead of the raw codes. Used inside `facet_wrap(~ sex, labeller = ...)`.
- `scale_fill_viridis_d(begin, end, option)`: fills groups with colours from the viridis palette. `begin` and `end` (0 to 1) choose which part of the palette is used, `option = "plasma"` picks a sister palette. `scale_fill_viridis_d(begin = 0.25, end = 0.75)`.
- `str_detect(text, "u")`: `TRUE` if the text contains the pattern anywhere. `str_detect("bu", "u")` gives `TRUE`. Inside `filter()` it keeps the matching rows. From `stringr`, part of the tidyverse.
- `count(sex, name = "years")`: like `count()`, but the new column is called `years` instead of `n`. Counting twice in a row (`count(sex, year) |> count(sex)`) first lists the combinations, then counts them per group.
- `patchwork`: puts saved ggplot charts together. `p1 + p2` side by side, `p1 / p2` one above the other. Loaded at the start with the other packages.
- `length(x)`: how many values a vector holds. `length(eu_codes)` gives `27`.
- `if (condition) { ... }`: runs the code inside only when the condition is `TRUE`. `if (!file.exists(f)) { download.file(u, f) }`. The `!` means "not".
- `pivot_wider(names_from = sex, values_from = life_expectancy)`: turns long into wide. The values of `sex` (F, M) become column names, and the numbers move into them. Two rows per country and year become one.
- `select(-flag)`: the minus sign means "drop this column", keep all others.
- `summarise(smallest = min(x), largest = max(x))`: turns a whole column into one row of summary numbers. `sum(x < 0)` counts how many values are below 0, because each `TRUE` counts as 1.
- `arrange(desc(x))`: sorts the rows, largest first. Without `desc()`, smallest first.
- `head(5)`: keeps the first 5 rows.
- `kable(digits = 1)`: rounds numbers to one decimal in the table.
- `left_join(x, y, by = join_by(country_code, year))`: keeps every row of `x` and adds the columns of `y` where both keys match. No match means `NA` in the new columns. The number of rows of `x` must not change.
- `anti_join(x, y, by = join_by(...))`: the rows of `x` that have no partner in `y`. Used to see exactly which rows will get `NA`.
- `join_by(country_code, year)`: says which columns must match. Both columns must match at the same time.
- `tibble(a = c(...), b = c(...))`: builds a small table by hand, one vector per column. The vectors must have the same length, and the n-th value of each belongs to the same row.
- `select(a, b, everything())`: puts the named columns first, then all the others in their old order.
- `sum(is.na(x))`: counts the missing values in a column. `is.na()` gives `TRUE` for each `NA`, and `sum()` counts them.
- `df$column`: takes one column out of a table. `gap_panel$country` is the `country` column.
- `write_csv(table, path)`: saves a table as a CSV file. Missing values are written as `NA`. `write_csv(gap_panel, here("data", "clean", "gap_panel.csv"))`.
