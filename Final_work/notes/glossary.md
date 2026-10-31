# Glossary

Each new function: one-line meaning and a tiny example.

- `paste0()`: glues pieces of text together with no space between them. `paste0("demo_", "mlexpec")` gives `"demo_mlexpec"`.
- `download.file(url, destfile, mode = "wb")`: saves a file from the internet to my computer. `"wb"` means copy it byte for byte.
- `file.exists(path)`: `TRUE` if the file is already there, `FALSE` if not. `file.exists(here("data-raw", "demo_mlexpec.csv"))`.
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
