# Running MPI on Topanga (single node and multi-node)

This guide covers building and running MPI programs on Topanga, on one node or across several. It is part of the Topanga user guide series.

## How multi-node sessions work on Topanga

When you launch a multi-node session, Topanga gives you one container on each node. The container your terminal lands in is called `main1`; the others are `sub1`, `sub2`, and so on. You never need to log in to the sub nodes: you work entirely from `main1`, and `mpirun` starts and manages the processes on the other nodes for you. The nodes reach each other by those hostnames, which is why the MPI hostfile below simply lists them.

All nodes in a session see the network storage systems: Topanga's data folder as well as `/home1`, `/scratch1`, and `/project2`. A program built once in any of these locations is immediately visible on every node, so there is no copying or staging step.

## Quick start

```bash
# 1. Environment: use HPC-X MPI, not the spack OpenMPI
source /apps/lmod/set-lmod.sh
module load gcc/13.3.0 hpcx-mpi/2.20
export LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LIBRARY_PATH   # linker needs this image's crt1.o

# 2. Build
mpif90 -O3 -o myapp myapp.f90        # Fortran
mpicc  -O3 -o myapp myapp.c          # C

# 3a. Run on a single node
mpirun -n 50 --bind-to none ./myapp

# 3b. Run across multiple nodes (full-speed InfiniBand)
mpirun --hostfile hostfile -np 200 --bind-to none -x UCC_TL_SHM_TUNE=0 ./myapp
```

where `hostfile` lists your session's nodes and how many ranks each may hold:

```
main1 slots=62
sub1  slots=62
sub2  slots=62
sub3  slots=62
```
where `slots=62` shows the number of CPU cores that each of your nodes have and number of subs depend on the size of the cluster that you launched.

This is the whole recipe. The rest of this page explains the non-obvious flags and how to recognize the errors you will hit if you deviate from it.

## Building

### Use HPC-X, not the spack OpenMPI

The spack `openmpi/5.0.5` module is built against Slurm/munge, which Topanga does not have, so its compiler wrappers fail before they can compile anything:

```
error while loading shared libraries: libmunge.so.2: cannot open shared object file
```

Use `hpcx-mpi/2.20` instead (NVIDIA HPC-X, which is OpenMPI 4.1.7 + UCX). It has no munge dependency and drives the InfiniBand network natively. Load it together with `gcc/13.3.0`.

### Two environment fixes for this container image

The container image keeps its C runtime and system headers in places the spack toolchain does not search by default:

```bash
export LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LIBRARY_PATH    # crt1.o, needed for any linking
export CPATH=/usr/include/x86_64-linux-gnu:/usr/include:$CPATH # system headers, only if gcc reports them missing
```

`LIBRARY_PATH` is required for every build. `CPATH` is only needed if your C code includes system headers that gcc reports as missing (for example `fatal error: sys/ipc.h: No such file or directory`).

## Running

### Single node

```bash
mpirun -n 50 --bind-to none ./myapp
```

`--bind-to none` is required everywhere on Topanga: the containers do not allow MPI to pin processes to specific CPU cores, so without this flag `mpirun` fails at startup.

### Multiple nodes

```bash
mpirun --hostfile hostfile -np 200 --bind-to none -x UCC_TL_SHM_TUNE=0 ./myapp
```

This runs at full InfiniBand speed between nodes and over shared memory within each node. No network tuning is needed; the only addition over the single-node command is the hostfile and one flag, explained next.

### The one extra multi-node flag

Without `-x UCC_TL_SHM_TUNE=0`, a multi-node run hangs at the first collective operation (Bcast, Allreduce, Barrier, ...) and floods the screen with:

```
tl_shm_team.c ... Child failed to attach to shmseg, errno: Invalid argument
```

In plain terms: HPC-X ships with an add-on called UCC that tries to speed up collective operations. One of UCC's tricks is a special shared-memory channel between ranks on the same node, and that particular trick is broken inside Topanga's containers when many ranks run per node. The flag tells UCC to skip that one trick; everything else works normally. This is a bug in the UCC library itself, not a system limit, so there is nothing to fix on your side or to request from the admins.

Any one of these three flags fixes it (all three were verified to give identical, correct results; the first and third were also timed back-to-back and show no performance difference):

| Flag | Effect |
|---|---|
| `-x UCC_TL_SHM_TUNE=0` | **Recommended.** Turns off only the broken part of UCC. |
| `-x UCC_TLS=ucp` | Keeps UCC but routes its collectives over the network layer. |
| `--mca coll_ucc_enable 0` | Turns off UCC entirely. Simplest to remember. |

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| `libmunge.so.2: cannot open shared object file` | Using the spack OpenMPI | `module load gcc/13.3.0 hpcx-mpi/2.20` |
| Linker cannot find `crt1.o` | Toolchain does not see the image's C runtime | `export LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LIBRARY_PATH` |
| `fatal error: sys/ipc.h: No such file or directory` | Spack gcc does not see system headers | `export CPATH=/usr/include/x86_64-linux-gnu:/usr/include:$CPATH` |
| `mpirun` fails at startup around CPU binding | Containers do not allow core pinning | Add `--bind-to none` |
| Multi-node run hangs; screen floods with `TL_SHM ERROR ... failed to attach to shmseg` | Broken shared-memory path in UCC | Add `-x UCC_TL_SHM_TUNE=0` (or an alternative from the table above) |
| `ibv_reg_mr ... Cannot allocate memory` at startup | Memory-lock limit regressed | Run the memlock check above; report to the system team |
| A normally fast run is suddenly much slower | Cluster contention (shared nodes, network, or filesystem) | Not a configuration issue; results stay correct. Re-run when the cluster is quieter |

## Complete example script

```bash
#!/bin/bash
# Build + run an MPI code across all nodes of a Topanga multi-node session.
set -e
cd /project2/<your_project>/<your_dir>     # any shared location works (/home1, /scratch1, /project2)

NP=${NP:-200}                              # total ranks across all nodes

# Environment: HPC-X MPI + this image's C runtime for the linker
source /apps/lmod/set-lmod.sh
module load gcc/13.3.0 hpcx-mpi/2.20
export LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LIBRARY_PATH

# Build
mpif90 -O3 -o myapp myapp.f90

# Run over full-speed InfiniBand
#   --bind-to none      : containers do not allow CPU core pinning
#   UCC_TL_SHM_TUNE=0   : skip UCC's broken shared-memory collective path
mpirun --hostfile hostfile -np "$NP" --bind-to none \
  -x UCC_TL_SHM_TUNE=0 \
  ./myapp
```
