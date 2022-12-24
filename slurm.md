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

# Simple (Single-Node) Job

This is the `gpu_benchmark.slurm` file.
```bash
#!/bin/bash

### script from Guglielmo Camporese
### guglielmo.camporese@phd.unipd.it

### logger (the fancy chars are for colored logging..)
function echo() { 
  command echo -e "\e[38;5;209m""\e[1m"[slurm-log "\e[0m"`date +"%T"`"\e[38;5;209m""\e[1m"]"\e[0m" $@;
}

### define slurm commands inside the slurm node (todo: fix this with cluster technical staff)
function scontrol() { $HOME/../gucampo/utils/scontrol $@; }
function srun() { $HOME/../gucampo/utils/srun $@; }

### nodes info
echo "start slurm job"
echo "--- node info"
echo "node:" `hostname`
echo "cuda drivers:" `which nvcc`
echo "GPU info:"
nvidia-smi

### activate conda env
source $HOME/../gucampo/miniconda3/etc/profile.d/conda.sh
conda activate torch
echo "using conda env: $CONDA_DEFAULT_ENV"
echo "----------"

### run program
echo "starting program..."
srun python $HOME/../gucampo/projects/gpu_benchmark/mnist.py
echo "slurm job finished. all done"
```

This is the slurm commands used for running the job.
```bash
# slurm command for running the job
sbatch --nodelist dellcuda2 \
    --job-name gpu_benchmark \
    --partition allgroups \
    --ntasks 1 \
    --mem 32G \
    --time 14:00:00 \
    --gres gpu:1 \
    --cpus-per-task 32 \
    --output gpu_benchmark.log \
    --error gpu_benchmark.log \
    $HOME/../gucampo/projects/gpu_benchmark/gpu_benchmark.slurm

```

This is the command for showing the realtime output log of the slurm experiment.
```bash
tail +1f distributed/torchrun_ddp/gpu_benchmark.log
```

# Distributed (Multi-Node) Job

```bash
#!/bin/bash

### script from Guglielmo Camporese
### guglielmo.camporese@phd.unipd.it

### logger (the fancy chars are for colored logging..)
function echo() { 
  command echo -e "\e[38;5;209m""\e[1m"[slurm-log "\e[0m"`date +"%T"`"\e[38;5;209m""\e[1m"]"\e[0m" $@;
}

### define slurm commands inside the slurm node (todo: fix this with cluster technical staff)
function scontrol() { $HOME/../gucampo/utils/scontrol $@; }
function srun() { $HOME/../gucampo/utils/srun $@; }

### nodes info
echo "start slurm job"
nodes=( $( scontrol show hostnames $SLURM_JOB_NODELIST ) )
nodes_array=($nodes)
head_node=${nodes_array[0]}
head_node_ip=$(srun --nodes=1 --ntasks=1 -w "$head_node" hostname --ip-address)
echo "--- nodes info"
echo "nodes: ${SLURM_NODELIST}"
echo "master node: $head_node"
echo "master node ip: $head_node_ip"
echo "cuda drivers:" `which nvcc`

### activate conda env
source $HOME/../gucampo/miniconda3/etc/profile.d/conda.sh
conda activate torch
echo "using conda env: $CONDA_DEFAULT_ENV"
echo "----------"

### run program
echo "starting program..."
srun torchrun \
  --nnodes=$SLURM_NNODES \
  --nproc_per_node=$SLURM_NTASKS_PER_NODE \
  --rdzv_id $RANDOM \
  --rdzv_backend c10d \
  --rdzv_endpoint $head_node:29500 \
  $HOME/../gucampo/projects/Cluster-Math-Resources/examples/\
    distributed/torchrun_ddp/train.py \
  --model resnet18 \
  --epochs 1 \
  --batch-size 64 \
  --dataset cifar10 \
  --data-path $HOME/../gucampo/datasets/cifar10
echo "slurm job finished. all done"
```

This is the slurm commands used for running the job.
```bash
# slurm command for running the job
sbatch --nodes 2 \
    --ntasks-per-node 1 \
    --partition testing \
    --cpus-per-task 4 \
    --gres gpu:1 \
    --mem 24G \
    --job-name torchrun_ddp \
    --error distributed/torchrun_ddp/torchrun_ddp.log \
    --output distributed/torchrun_ddp/torchrun_ddp.log \
    distributed/torchrun_ddp/job.slurm
```

This is the command for showing the realtime output log of the slurm experiment.
```bash
tail +1f distributed/torchrun_ddp/torchrun_ddp.log
```