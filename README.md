# MLHub Notes #

Notes for [MLHub](https://github.com/mlhubber/mlhub).

* [Development Environment Setup](#development-environment-setup)
* [Source Code Structure](#source-code-structure)
    + [`setup.py`](#setuppy)
    + [`__init__.py`](#__init__py)
    + [`commands.py`](#commandspy)
* [MLHub Subcommands](#mlhub-subcommands)
    + [`ml available`](#ml-available)
    + [`ml installed`](#ml-installed)
* [Reference](#reference)


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
# replace <branch> with the actual branch name or commit hash
pip install -e git://github.com/mlhubber/mlhub.git@<branch>#egg=mlhub
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
      included in the package.  See [Data Files
      Support](https://setuptools.readthedocs.io/en/latest/userguide/datafiles.html).
* `console_scripts` in `entry_points`
    + Register Python functions (parts after `:`) in a package (parts
      before `:`) as command-line tool (parts before `=`).  See
      [Command Line
      Scripts](https://python-packaging.readthedocs.io/en/latest/command-line-scripts.html).
* `install_requires`
    + Specify Python dependencies.


### `__init__.py` ###

The `main()` function is the entry point of the `ml` command.  It uses
[**argparse**](https://docs.python.org/3/library/argparse.html) (See
also [Parse command line
arguments](https://github.com/simonzhaoms/tips/blob/master/python/parse-command-line-args.md))
to parse the arguments passed to `ml`.  Arugment parsing consists of 2
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

Logging configuration and error handling will also be dealt with in
`main()`.


### `commands.py` ###

`commands.py` defines all the sub-commands of `ml`.  The mapping
between sub-command names and functions is described in the
`mlhub.constants.COMMANDS` dict.


## MLHub Subcommands ##

### `ml available` ###

It will call `mlhub.commands.list_available()` to print the curated
list of available packages at `mlhub.constants.MLHUB +
mlhub.constants.META_YAML`(https://mlhub.ai/Packages.yaml).

`ml available --name-only` will print the package names only.


#### `ml installed` ####

It will call `mlhub.commands.list_installed()` to print the list of
locally-installed packages in the `mlhub.constants.MLINIT`
(`~/.mlhub/`) folder by default.

A valid MLHub model package directory will contain a file called
`MLHUB.yaml` that describe the package.  For example:

```yaml
--- # animate
meta:
  name         : animate
  title        : Tell a data narative through animations
  keywords     : r, animation, plot, sports, line chart, athletics
  version      : 2.1.5
  languages    : R
  modeller     : gganimate
  type         : plot
  display      : demo
  license      : gpl3
  author       : Vincent Yu
  maintainer   : Graham.Williams@togaware.com
  url          : https://github.com/gjwgit/animate
dependencies:
  system:
    - eom
    - cargo
  cran:
    - progress
    - png
    - tidyverse
    - RColorBrewer
    - gifski
    - farver
    - tweenr
    - gganimate
  github:
    - ellisp/ggflags
  files:
    - animate_800.gif
    - demo.R
    - gganimate.tar.gz
    - iaaf.csv
    - iaaf.R
    - README.md
commands:
  demo : Step through a series of animation demos.
```




## Reference ##

* MLHub
    + [GitHub repo](https://github.com/mlhubber/mlhub)
    + [Webpage](https://mlhub.ai)

