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

# see node property
scontrol show node <nodename>
```
