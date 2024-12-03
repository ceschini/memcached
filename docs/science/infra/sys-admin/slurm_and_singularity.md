# Slurm and Singularity

## Singularity

Singularity is a container manager commonly used within scientific HPCs. Below are some commonly used commands, mainly the `run` and the `build` command.

### Run

```bash
singularity run --nv \  # nvidia
    -B /local/path:/container/path \  # map volume folders
    /path/to/singularity-image.sif \  # container image.sif
    sh /container/path/run.sh  # bash command (sh, python, etc)
```
### Build

There is a script that will build the `.sif` image file for you, call it as follows:

```bash
sbatch /share_zeta/UnderTheSea/singularities/build_sif.srm <sif_name> <docker_hub_name>
```

This command will run `singularity build $SIF_NAME.sif docker://$HUB_NAME`, building a
singularity image from docker hub, and saving it in the current directory. For more options and instructions, check the [build_a_container](https://docs.sylabs.io/guides/3.0/user-guide/build_a_container.html) guide.

## Slurm

But you can't run the singularity command above directly in the cluster, you must use Slurm to schedule a job for you. This is the reason why we won't call `singularity build` directly, for example, and why we must envelop it in a `.srm` file and call it using slurm's `sbatch` command.

### `sbatch`

This command will schedule a job to be run, and accepts options directly in the command call, or from within a special file with the `.srm` extension. This files will have a header with the `sbatch` options, and the script or command to be run. Check the example below.

```shell
#!/bin/sh
#SBATCH --nodes=1
#SBATCH --partition=gpu
#SBATCH --ntasks=1
#SBATCH --mem=7500
#SBATCH --gres=gpu:1
#SBATCH --oversubscribe
#SBATCH --ntasks-per-node=1
#SBATCH -J waifu_inference.srm
#SBATCH -o outputs/inf_output.txt

WAIFU_ROOT=/share_zeta/UnderTheSea/waifu2x

singularity run --nv \
        -B /share_zeta/UnderTheSea/data/test/ciab-23/out_stream:/input_dir \
        -B share_zeta/UnderTheSea/data/results/waifu2x/out_stream_noise_scale4x:/output_dir \
        -B $WAIFU_ROOT/nunif:/root/nunif \
        -B $WAIFU_ROOT/scripts:/scripts \
        /share_zeta/UnderTheSea/singularities/nunif.sif \
        sh /scripts/entrypoints/run_waifu_inference.sh
```

- **-N, --nodes**: Request a minimum and maximum number of nodes to be allocated to the job. If only one number is specified, this is used as both the minimum and maximum node count.
- **-p, --partition**: Request a specific partition for the resource allocation. If not specified, the default partition designated by the system administrator will be selected. 
- **-n, --ntasks**: sbatch does not launch tasks, it requests an allocation of resources and submits a batch script. This option advises the Slurm controller that job steps run within the allocation will launch a maximum of _number_ tasks and to provide for sufficient resources.
- **--mem**: Specify the real memory required per node. Default units are megabytes. Different units can be specified using the suffix \[K|M|G|T].
- **--gres**: Specifies a comma-delimited list of generic consumable resources. The format for each entry in the list is "name\[\[:type]:count]". The _name_ is the type of consumable resource (e.g. gpu). The _type_ is an optional classification for the resource (e.g. a100). The _count_ is the number of those resources with a default value of 1.
- **-s, --oversubscribe**: The job allocation can over-subscribe resources with other running jobs. The resources to be over-subscribed can be nodes, sockets, cores, and/or hyperthreads depending upon configuration. This option may result in the allocation being granted sooner than if the --oversubscribe option was not set and allow higher system utilization, but application performance will likely suffer due to competition for resources.
- **-ntasks-per-node**: Request that _ntasks_ be invoked on each node. If used with the **--ntasks** option, the **--ntasks** option will take precedence and the **--ntasks-per-node** will be treated as a _maximum_ count of tasks per node.
- **-J, --job-name**: Specify a name for the job allocation. The specified name will appear along with the job id number when querying running jobs on the system.
- **-o, --output**: Instruct Slurm to connect the batch script's standard output directly to the file name specified in the "_filename pattern_". By default both standard output and standard error are directed to the same file.

### Auxiliary commands

#### `squeue`

Shows information about jobs located in the Slurm scheduling queue.

```bash
 JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
 17713       cpu proxy_vs   felipe  R    1:33:25      1 cpunode-2-0
 17705       gpu tornado-   heitor  R   18:50:56      1 gpunode-1-7
 17712       gpu deepoil_  abdigal  R   15:26:23      1 gpunode-1-4
 17727       gpu waifu_in lucasces  R       0:01      1 gpunode-1-0
```

Add the `--me` option to display only your jobs.

```bash
$ squeue --me
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
17727       gpu waifu_in lucasces  R       1:09      1 gpunode-1-0

```

#### `scancel`

This command is used to signal or cancel jobs.

```
scancel [OPTIONS...] job_id
```

Add the `--me` option to restrict the cancelling to your user jobs.

#### `sinfo`

Shows information about partitions, its availability, job's time limit, number of nodes, statuses, and node list.

```bash
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
gpu*         up 3-00:00:00      2    mix gpunode-1-[4,7]
gpu*         up 3-00:00:00      5   idle gpunode-1-[0-1,3,5-6]
cpu          up 3-00:00:00      1    mix cpunode-2-0
```

#### `scontrol show node`

Shows information about all available nodes, including its CPU, GPU name, partition, available features, node address and more.

```bash
...
NodeName=gpunode-1-7 Arch=x86_64 CoresPerSocket=6 
   CPUAlloc=2 CPUEfctv=24 CPUTot=24 CPULoad=0.00
   AvailableFeatures=gpu
   ActiveFeatures=gpu
   Gres=gpu:gtx1080:2
   NodeAddr=gpunode-1-7 NodeHostName=gpunode-1-7 Version=23.02.5
   OS=Linux 5.15.0-125-generic #135-Ubuntu SMP Fri Sep 27 13:53:58 UTC 2024 
   RealMemory=22000 AllocMem=6000 FreeMem=22516 Sockets=2 Boards=1
   State=MIXED ThreadsPerCore=2 TmpDisk=0 Weight=1 Owner=N/A MCS_label=N/A
   Partitions=gpu 
   BootTime=2024-12-02T16:14:18 SlurmdStartTime=2024-12-02T16:15:00
   LastBusyTime=2024-12-02T16:15:00 ResumeAfterTime=None
   CfgTRES=cpu=24,mem=22000M,billing=24,gres/gpu=4
   AllocTRES=cpu=2,mem=6000M,gres/gpu=2
   CapWatts=n/a
   CurrentWatts=0 AveWatts=0
   ExtSensorsJoules=n/s ExtSensorsWatts=0 ExtSensorsTemp=n/s
```

