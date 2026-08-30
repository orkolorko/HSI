# HSI

Lecture notes and Jupyter notebooks for the **Hokkaido Summer Institute** summer
school on dynamics and computation.

This is a living repository: it carries the current version of the material, and
each year is tagged when that year's school finishes. If you attended a
particular year and want the material exactly as it was taught, use its tag; if
you want the best version, use `main`, which will have had later corrections.

| year | tag | school page |
|---|---|---|
| 2026 | *tagged after the school* | [dyn-comp-2026](https://sites.google.com/view/dyn-comp-2026/) |
| 2025 | [`HSI2025`](https://github.com/orkolorko/HSI2025) (archived) | [dyn-comp-2025](https://sites.google.com/view/dyn-comp-2025/) |
| 2024 | [`HSI2024`](https://github.com/orkolorko/HSI2024) (archived) | |
| 2023 | [`HSI2023`](https://github.com/orkolorko/HSI2023) (archived) | |

Years before 2026 have their own repositories, kept read-only. From 2026 the
material lives here, so that a correction is made once rather than once per
year.

## Contents

- `Lecture1.md` — installing Julia, the REPL, packages and environments

The notebooks for the remaining lectures are still to come.

## Setting up

```julia
import Pkg
Pkg.activate(".")
Pkg.instantiate()
```

`Project.toml` lists the packages the school uses. `Manifest.toml` is generated
by `instantiate` and should be committed once it resolves cleanly on the Julia
version the school will actually use, so that every participant gets the same
environment.
