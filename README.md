# RESPINOW-realtime

This repository contains the realtime submission pipeline for the
RESPINOW project.\
Follow the steps below to set up the Python and R environments
reproducibly.

------------------------------------------------------------------------

## 1. Python setup (uv)

### Install uv

`curl -LsSf https://astral.sh/uv/install.sh | sh`

Restart your shell and verify:

`uv --version`

------------------------------------------------------------------------

### Sync Python environment

Inside the repository:

```sh
cd RESPINOW-realtime
uv sync
```

This will install the right Python version and all dependencies defined in:

-   `pyproject.toml`
-   `uv.lock`

You can now use `uv run python script.py` to run a script with the right Python version and environment.

------------------------------------------------------------------------

## 2. Jupyter kernel setup

Create a kernel using the uv environment:

`uv run python -m ipykernel install --user --name respinow-realtime`

After this, you can select **respinow-realtime** as kernel in Jupyter notebooks.

------------------------------------------------------------------------

## 3. Install correct R version (4.5.1)

```R
cd "$HOME"
wget https://cran.r-project.org/src/base/R-4/R-4.5.1.tar.gz
tar -xzf R-4.5.1.tar.gz
cd R-4.5.1
./configure --prefix="$HOME/R/4.5.1" --enable-R-shlib --without-x
make -j"$(nproc)"
make install
```

This installs an additional R version but does not change the default, so you always have to manually pick the right one.

------------------------------------------------------------------------

## 4. Restore R environment with renv

Inside the `r` folder of this repo, start the correct R version:

`~/R/4.5.1/bin/R`

Inside the R console:

`renv::restore()`

Notes: 
- Ignore warning about missing package **here** (safe to skip)
- This installs all dependencies into `r/renv/`


------------------------------------------------------------------------

## 5. GitHub authentication (cron / automation)

For non-interactive scripts (cron jobs), GitHub credentials must be provided via an environment variable.
First, create a classic GitHub Personal Access Token (PAT) in your GitHub account settings with the permissions 'repo' and 'workflow'.

Create a token file:

`nano ~/.github_token`

Add:

`export GITHUB_TOKEN="PASTE_YOUR_TOKEN_HERE" (without quotes)`

For example `export GITHUB_TOKEN=ghp_123456789`.

Lock down permissions:

`chmod 600 ~/.github_token`

Your cron job should source this file (so it can access the environment variable `GITHUB_TOKEN`):

`. "$HOME/.github_token"`

Never commit this file or print the token to the console.

------------------------------------------------------------------------


## Summary

You now have:

-   isolated Python environment via uv
-   dedicated Jupyter kernel: respinow-realtime
-   pinned R version (4.5.1)
-   fully reproducible R packages via renv
-   secure GitHub authentication for automation

