# SLURM Useful Commands

```bash
# submit a job on a particular node
sbatch --nodelist <nodename> job.slurm

# see running jobs
watch -n 0.1 squeue

# start an interactive job on a node
srun --nodelist <nodename> --pty bash -i

# see nodes status and partitions
sinfo -l

# see node properties
scontrol show node <nodename>

# see job properties
scontrol show job <jobid>
```

### Example of SLURM job

```bash
#!/bin/bash

# Script from Guglielmo Camporese
# guglielmo.camporese@phd.unipd.it

# GPU kind              | node name         | property       | cpus |
# ----------------------|-------------------|----------------|------|
# Tesla V100-PCIE-16GB  | dellcuda0         | ?              | 32   |
# A100-PCIE-40GB        | dellcuda1         | VIMP (1/3)     | 32   |
# Tesla V100-PCIE-32GB  | dellcuda2         | VIMP           | 64   |
# Tesla V100-FHHL-16GB  | dellsrv1          | ?              | 40   |
# Tesla T4              | dellsrv2          | VIMP           | 40   |
# Tesla T4              | dellsrv3          | VIMP           | 40   |
# Tesla T4              | dellsrv4          | VIMP           | ?    |
# Tesla T4              | dellsrv5          | VIMP           | ?    |
# Tesla T4              | dellsrv6          | ?              | ?    |
# A5000                 | vgpu0-0           | Math Depatment | ?    |
# A5000                 | vgpu0-1           | Math Depatment | ?    |
# A5000                 | vgpu1-0           | Math Depatment | ?    |
# A5000                 | vgpu1-1           | Math Depatment | ?    |
# A5000                 | vgpu2-0           | Math Depatment | ?    |
# A5000                 | vgpu2-1           | Math Depatment | ?    |
# A5000                 | vgpu3-0           | Math Depatment | ?    |
# A5000                 | vgpu3-1           | Math Depatment | ?    |
# A5000                 | vgpu4-0           | Math Depatment | ?    |
# A5000                 | vgpu4-1           | Math Depatment | ?    |
# A5000                 | vgpu5-0           | Math Depatment | ?    |
# ------------------------------------------------------------------|

### SLURM Directives
#SBATCH --nodelist=dellcuda2
#SBATCH --job-name=”gpu_benchmark”
#SBATCH --partition=allgroups
#SBATCH --ntasks=1
#SBATCH --mem=32G
#SBATCH --time=14:00:00
#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=32

### Run program
echo "*running node: ${SLURM_NODELIST}"
nvidia-smi
which nvcc

### With singularity...
#CONTAINER_PATH="$HOME/containers/torch_container.sif"
#SHAREDIR="/conf/shared-software/Singularity/CUDA"
#singularity exec -B ${SHAREDIR}/driver/dellcuda2:/nvdriver "${CONTAINER_PATH}" \
#  python3 ${HOME}/projects/gpu_benchmark/mnist.py

### Without singularity...
source $HOME/miniconda3/etc/profile.d/conda.sh
conda activate torch
python3 ${HOME}/projects/gpu_benchmark/mnist.py
```
