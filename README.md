# MLHub Notes #

Notes for [MLHub](https://github.com/mlhubber/mlhub).


## Installation ##

```bash

```


## Development Environment Setup ##

```bash
# Python dependencies specified in the 'setup.py' file of MLHub source code
MLHUB_DEPS="
  distro
  fuzzywuzzy
  python-Levenshtein
  pyyaml
  requests
  yamlordereddictloader
"

# create conda environment
CONDA_ENV_NAME='mlhub'
conda create -c conda-forge -y -n ${CONDA_ENV_NAME} ${MLHUB_DEPS}

# install mlhub
# 1. from local copy
conda activate ${CONDA_ENV_NAME}
make dist
pip install dist/mlhub*.gz

# 2. or install mlhub from GitHub
conda activate ${CONDA_ENV_NAME}
pip install -e git://github.com/mlhubber/mlhub.git@<branch>#egg=mlhub  # replace <branch> with the actual branch name or commit hash
```


## Source Code Structure ##

```
mlhub
├── DESCRIPTION                       # for MLHub R packaging
├── Dockerfile                        # MLHUb Dockerfile
├── docs                              # Docs
│   ├── index.rst
│   └── README.md
├── LICENSE
├── logo-mlhub.lic
├── logo-mlhub.svg
├── Makefile
├── man                               # MLHub R Docs
│   ├── mlask.Rd
│   ├── mlcat.Rd
│   └── mlpreview.Rd
├── MANIFEST.in                       # list of files to be packaged
│
├── mlhub                             # source code folder
│   ├── bash_completion.d             # MLHub shell command auto-completion
│   │   └── ml.bash
│   ├── commands.py                   # sub-commands of 'ml' command
│   ├── constants.py                  # constant definitions
│   ├── __init__.py
│   ├── pkg.py
│   ├── scripts
│   │   ├── convert_readme.sh
│   │   └── dep                       # script dealing with package dependencies
│   │       ├── mlhub.sh
│   │       ├── python.sh
│   │       ├── r.R
│   │       ├── system.sh
│   │       └── utils.sh
│   └── utils.py
├── NAMESPACE                         # for MLHub R packaging
├── pandoc.css
├── pyproject.toml                    # 
├── R                                 # for MLHub R packaging
│   ├── mlask.R
│   ├── mlcat.R
│   └── mlpreview.R
├── README.md
├── setup.cfg                         # for MLHub Python packaging
└── setup.py                          # for MLHub Python packaging
```

### `setup.py` ###

`setup.py` is used for describing the MLHub Python package by using
parameters of the `setup()` function of
[setuptools](https://setuptools.readthedocs.io/en/latest/setuptools.html),
such as the package name (mlhub), version, etc.
* `name`
    + Package name
* `version`
    + Package version
* `packages`
    + Specify which packages (`.py` files) will be included.  See
      [Using
      `find_packages()`](https://setuptools.readthedocs.io/en/latest/setuptools.html#using-find-packages).
* `package_data`
    + Specify which files (non-`.py` files) in a sub-package will be
      included in the package.  See [Including Data
      Files](https://setuptools.readthedocs.io/en/latest/setuptools.html#including-data-files).
* `console_scripts` in `entry_points`
    + Register Python functions (parts after `:`) in a package (parts
      before `:`) as command-line tool (parts before `=`).  See
      [Command Line
      Scripts](https://python-packaging.readthedocs.io/en/latest/command-line-scripts.html).
* `install_requires`
    + Specify Python dependencies.


### `__init__.py` ###

The `main()` function is the entry point of the `ml` command.  It uses
[**argparse**](https://docs.python.org/3/library/argparse.html) to
parse the arguments passed to `ml`.  Arugment parsing consists of 2
levels:
1. Parse options of the `ml` command defined by the
   `mlhub.constants.OPTIONS` variable, such as `--version`.  These
   options are available across all commands.
1. Parse subcommands of `ml`.  They could be:
    + the basic commands of `ml` defined by the
      `mlhub.constants.COMMANDS` variable, such as `install`.
    + or the commands defined by the model package.  They will be
      handled in the `mlhub.commands.dispatch()` function.

Then the subcommands will be referred as `args.func` to be called at
the end of `main()`.

Logger and error handling will also be dealt with in `main()`.



## Reference ##

* MLHub
    + [GitHub repo](https://github.com/mlhubber/mlhub)
    + [Webpage](https://mlhub.ai)

