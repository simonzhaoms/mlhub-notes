# MLHub Notes #

## Environment Setup ##

```bash
# Dependencies specified in the setup.py file of MLHub
MLHUB_DEPS="
  distro
  fuzzywuzzy
  python-Levenshtein
  pyyaml
  requests
  yamlordereddictloader
"
conda create -c conda-forge -y -n mlhub ${MLHUB_DEPS}
```
