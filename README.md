# HSI2026

Lecture notes and Jupyter notebooks for the **HSI2026** summer school.

[School Home Page](https://sites.google.com/view/dyn-comp-2026/)

## Contents

- `2026Lecture1.md` — installing Julia, the REPL, packages and environments

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
