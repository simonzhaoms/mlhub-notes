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


## Reference ##

* MLHub
    + [GitHub repo](https://github.com/mlhubber/mlhub)
    + [Webpage](https://mlhub.ai)

