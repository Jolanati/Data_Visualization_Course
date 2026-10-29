# Glossary

Each new function: one-line meaning and a tiny example.

- `paste0()`: glues pieces of text together with no space between them. `paste0("demo_", "mlexpec")` gives `"demo_mlexpec"`.
- `download.file(url, destfile, mode = "wb")`: saves a file from the internet to my computer. `"wb"` means copy it byte for byte.
- `file.exists(path)`: `TRUE` if the file is already there, `FALSE` if not. `file.exists(here("data-raw", "demo_mlexpec.csv"))`.
- `if (condition) { ... }`: runs the code inside only when the condition is `TRUE`. `if (!file.exists(f)) { download.file(u, f) }`. The `!` means "not".
