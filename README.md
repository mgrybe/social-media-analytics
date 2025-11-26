Create virtual environment

```shell
python -m venv .venv
```

Activate virtual environment

```shell
. .venv/bin/activate
```

Install dependencies

```shell
pip install -r requirements.txt
```

Use JupyterLab / Notebooks

1) Register this virtualenv as a Jupyter kernel (one-time per env):

```shell
python -m ipykernel install --user --name social-media-analytics --display-name "Python (social-media-analytics)"
```

2) Launch JupyterLab:

```shell
jupyter lab
```

- In your browser, open preprocessing.ipynb.
- When prompted (or via the Kernel menu), select the "Python (social-media-analytics)" kernel.

Notes

- You can also open the notebook directly with VS Code and select the same kernel.
- .gitignore already excludes .ipynb_checkpoints.

Python version and dependency ranges

- This project is verified with Python 3.13.
- requirements.txt pins lower bounds to the currently working versions on Python 3.13 and caps to the next major version to avoid breaking changes from future major releases.
- To install: pip install -r requirements.txt (as above).

Use Jupyter with R (IRkernel)

You can run R code in Jupyter by installing the R kernel (IRkernel). This is separate from Python and does not require changing requirements.txt.

1) Install R

- Conda (recommended if you already use Conda):

```shell
# create or use your existing R env
conda activate r_env  # or: conda create -n r_env -c conda-forge r-base r-essentials -y && conda activate r_env
# add IRkernel (R kernel for Jupyter)
conda install -c conda-forge r-irkernel -y
```

- macOS (Homebrew):

```shell
brew install r
```

- Ubuntu/Debian:

```shell
sudo apt-get update && sudo apt-get install -y r-base
```

2) Register the R kernel with Jupyter

- If you installed via Conda and used `r-irkernel`, the kernelspec is often registered automatically. If not, or to set a custom name, run in your Conda R env:

```shell
conda activate r_env
R -e 'IRkernel::installspec(user = TRUE, name = "ir-conda", displayname = "R (Conda r_env)")'
```

- If you installed R outside Conda, start an R session and run:

```r
install.packages("IRkernel")
IRkernel::installspec(user = TRUE)  # registers the kernel for the current user
```

3) Verify the kernel is available to Jupyter

```shell
jupyter kernelspec list
```

You should see an entry like `ir-conda` or `ir` pointing to an R kernelspec. Kernels registered with `user = TRUE` are visible to any Jupyter installation on your user account (even from a different Conda/env).

4) Use R in JupyterLab

- Launch JupyterLab (from any Python env that has Jupyter installed — e.g., your `py_env`):

```shell
# Example: use your Python env for Jupyter, while the R kernel comes from r_env
conda activate py_env
jupyter lab
```

- In the launcher, click "R" under Notebook, or open an existing notebook and switch the kernel to "R (Conda r_env)" (or "R (IRkernel)") via the Kernel menu.

Tips

- Multiple Conda envs: It’s common to keep Jupyter in a Python env and the R kernel in a separate `r_env`. Because kernels are registered per-user, Jupyter will discover the R kernel automatically once registered.
- If Jupyter does not show the R kernel, ensure you ran the `IRkernel::installspec` step from the R environment you intend to use and re-check with `jupyter kernelspec list`.
- Python and R kernels are independent. You can have separate notebooks for each.
- If you need tight interop (calling Python from R or vice versa), consider:
  - R → Python: the `reticulate` R package (install in your R env).
  - Python → R: the `rpy2` Python package (optional install in your Python env; not included in requirements.txt).
