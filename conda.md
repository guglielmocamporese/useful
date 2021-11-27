# Conda Useful Commands

### Create
```bash
# create conda env from scratch
conda create -n myenv python=3.7

# create conda env from yml
conda env create -f environment.yml
```

### Export
```bash
# export env to yml, from inside the env
conda env export > environment.yml
```

### Remove
```bash
# remove a conda env
conda remove --name myenv --all
```

### Details
```bash
# see all the conda env installed
conda info --envs
```
