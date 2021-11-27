# Conda Useful Commands

### Create a conda env
```bash
# create conda env from scratch
conda create -n myenv python=3.7

# create conda env from yml
conda env create -f environment.yml
```

### Export env packages to yml
```bash
# export env to yml, from inside the env
conda env export > environment.yml
```

### Remove a conda env
```bash
# remove a conda env
conda remove --name myenv --all
```

### See installed conda env
```bash
# see all the conda env installed
conda info --envs
```
