---
authors:
  - shubhamdang
date: 2024-03-06
categories:
  - se1
readtime: 2
---

# Setup OpenMPI and Build/Execute a MPI Job
In this blog, we'll guide you through the seamless installation of a OpenMPI on a RHEL-based operating system, building and running a MPI job. Our setup involves a single node.

Let's start with a step-by-step process, starting from creating virtual machines on the Alces Cloud platform, leading up to the installation of OpenMPI and then execution of the MPI job.
<!-- more -->

## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

## Setup OpenMPI and execute a Job
**Installation of OpenMPI**

Below are the step to install OpenMPI on the RHEL based operating System.

```bash
sudo dnf makecache --refresh
sudo dnf -y install openmpi
```

**Build and Execute the MPI job**

Sample MPI job script is provided below.

```markdown
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    // Initialize the MPI environment
    MPI_Init(NULL, NULL);

    // Get the number of processes
    int world_size;
    MPI_Comm_size(MPI_COMM_WORLD, &world_size);

    // Get the rank of the process
    int world_rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);

    // Get the name of the processor
    char processor_name[MPI_MAX_PROCESSOR_NAME];
    int name_len;
    MPI_Get_processor_name(processor_name, &name_len);

    // Print off a hello world message
    printf("Hello world from processor %s, rank %d out of %d processors\n",
           processor_name, world_rank, world_size);

    // Finalize the MPI environment.
    MPI_Finalize();
}
```

Below are the step to build and execute the MPI job.

```bash
# copy the script content in file mpi-hello-world.c
vi mpi-hello-world.c

# load mpi module 
module load mpi

# build the mpi job 
mpicc -o mpi mpi-hello-world.c

# Execute the MPI job
mpirun -np 4 mpi

# Results
Hello world from processor myinstance.novalocal, rank 1 out of 4 processors
Hello world from processor myinstance.novalocal, rank 2 out of 4 processors
Hello world from processor myinstance.novalocal, rank 0 out of 4 processors
Hello world from processor myinstance.novalocal, rank 3 out of 4 processors
```


