1. Open CMD:

![image](https://github.com/user-attachments/assets/1dae5cd3-45dc-42e4-b238-8ebd61bd7760)

2. Log into Frontier using your XCAMS/UCAMS username and password: `ssh USERNAME@frontier.olcf.ornl.gov`

NOTES:
The password is your pin + your 6 digit RSA code

![image](https://github.com/user-attachments/assets/0df40f85-3e6e-4037-a173-c24176fe9f9e)

You will now see a message asking about connecting, simply type "yes":

![image](https://github.com/user-attachments/assets/2df5668f-da69-485f-acec-f5f62e07b785)

Reconnect again with step 2:

![image](https://github.com/user-attachments/assets/b45a245f-b2f5-4eb2-ad1f-5ede3263b7a4)

3. Type: git clone https://github.com/parthenon-hpc-lab/athenapk.git athenapk

![image](https://github.com/user-attachments/assets/7a5a2c73-9e02-4cdd-bdf9-c22bc0c4eb0b)

4. Type: cd athenapk

![image](https://github.com/user-attachments/assets/ca9ce564-5887-4e64-bc6e-9cd7170cad02)

5. Type: git submodule init

![image](https://github.com/user-attachments/assets/aa85372d-9f34-4f22-bc0c-34f5e1e1a2b1)

6. Type: git submodule update

![image](https://github.com/user-attachments/assets/6bfc77ec-512e-4807-ba01-9ae76c6a52dc)

7. Type: module load cpe/23.09

![image](https://github.com/user-attachments/assets/2128a45b-a6c1-4be2-9ac6-cfcd234c864b)

8. Type: module load PrgEnv-amd

![image](https://github.com/user-attachments/assets/ea0c9968-80d4-4105-a587-46d92f93b9ac)

9. Type: module load craype-accel-amd-gfx90a

![image](https://github.com/user-attachments/assets/01d9ba78-bcb6-4920-bef1-476128d51bc7)

10. Type: module load cmake

![image](https://github.com/user-attachments/assets/e374d65d-f350-442c-af97-a90aadca58f1)

11. Type: module load hdf5

![image](https://github.com/user-attachments/assets/9ea35c03-05ec-49e4-a96b-07a188a803b1)

12. cd into athenapk if you are not already there.

![image](https://github.com/user-attachments/assets/3c2b2459-a304-4ae5-9eb4-9162037e1cea)

13. Type: ls

![image](https://github.com/user-attachments/assets/c0df9240-fb2a-409d-bf9d-06df8bde702d)

14. Type: vim FrontierAndCrusher.cmake

![image](https://github.com/user-attachments/assets/27109823-ffd9-4455-8a28-c32727cdfc87)

15. Press the letter “i” on the keyboard:

![image](https://github.com/user-attachments/assets/6dfe3d2e-f533-498b-add9-1f751d1d9430)

16. Copy and paste everything in this box below into that file:

```
#========================================================================================
# Parthenon performance portable AMR framework
# Copyright(C) 2022 The Parthenon collaboration
# Licensed under the 3-clause BSD License, see LICENSE file for details
#========================================================================================
# (C) (or copyright) 2022. Triad National Security, LLC. All rights reserved.
#
# This program was produced under U.S. Government contract 89233218CNA000001 for Los
# Alamos National Laboratory (LANL), which is operated by Triad National Security, LLC
# for the U.S. Department of Energy/National Nuclear Security Administration. All rights
# in the program are reserved by Triad National Security, LLC, and the U.S. Department
# of Energy/National Nuclear Security Administration. The Government is granted for
# itself and others acting on its behalf a nonexclusive, paid-up, irrevocable worldwide
# license in this material to reproduce, prepare derivative works, distribute copies to
# the public, perform publicly and display publicly, and to permit others to do so.
#========================================================================================


message(STATUS "Loading machine configuration for OLCF's Frontier and Crusher.\n"
  "Supported MACHINE_VARIANT includes 'hip' or 'cray', and 'hip-mpi' (default) or 'hip-mpi, respectively.'\n"
  "This configuration has been tested (on 2022-12-23) using the following modules: \n"
  "  $ module load PrgEnv-amd craype-accel-amd-gfx90a cmake hdf5 cray-python amd/5.4.0 cray-mpich/8.1.21\n"
  "and environment variables:\n"
  "  $ export MPICH_GPU_SUPPORT_ENABLED=1\n\n"
  "On Frontier, different modules are required (tested on 2023-06-21): \n"
  "  $ load PrgEnv-cray craype-accel-amd-gfx90a cmake cray-hdf5-parallel cray-python amd-mixed/5.3.0 cray-mpich/8.1.23 cce/15.0.1\n"
  "  $ export MPICH_GPU_SUPPORT_ENABLED=1\n\n"
  "NOTE: In order to run the test suite, the build directory should be on GPFS (or on\n"
  "Frontier, Lustre) work filesystem and not in your NFS user or project home (because\n"
  "they are read-only on compute nodes).\n\n")

# common options
set(Kokkos_ARCH_ZEN3 ON CACHE BOOL "CPU architecture")
set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Default release build")
set(MACHINE_VARIANT "hip-mpi" CACHE STRING "Default build for CUDA and MPI")

# variants
set(MACHINE_CXX_FLAGS "")
if (${MACHINE_VARIANT} MATCHES "hip")
  set(Kokkos_ARCH_VEGA90A ON CACHE BOOL "GPU architecture")
  set(Kokkos_ENABLE_HIP ON CACHE BOOL "Enable HIP")
  set(CMAKE_CXX_COMPILER hipcc CACHE STRING "Use hip wrapper")
elseif (${MACHINE_VARIANT} MATCHES "cray")
  set(Kokkos_ARCH_VEGA90A ON CACHE BOOL "GPU architecture")
  set(Kokkos_ENABLE_HIP ON CACHE BOOL "Enable HIP")
  set(CMAKE_CXX_COMPILER CC CACHE STRING "Use Cray wrapper")
else()
  set(CMAKE_CXX_COMPILER $ENV{ROCM_PATH}/llvm/bin/clang++ CACHE STRING "Use g++")
  set(MACHINE_CXX_FLAGS "${MACHINE_CXX_FLAGS} -fopenmp-simd -fprefetch-loop-arrays")
endif()

# Setting launcher options independent of parallel or serial test as the launcher always
# needs to be called from the batch node (so that the tests are actually run on the
# compute nodes.
set(TEST_MPIEXEC "srun" CACHE STRING "Command to launch MPI applications")
set(TEST_NUMPROC_FLAG "-n" CACHE STRING "Flag to set number of processes")
set(NUM_GPU_DEVICES_PER_NODE "1" CACHE STRING "4x MI250x per node with 2 GCDs each but one visible per rank")
set(PARTHENON_ENABLE_GPU_MPI_CHECKS OFF CACHE BOOL "Disable check by default")

if (${MACHINE_VARIANT} MATCHES "mpi")
  if (${MACHINE_VARIANT} MATCHES "hip")
    # need to set include flags here as the target is not know yet when this file is parsed
    set(MACHINE_CXX_FLAGS "${MACHINE_CXX_FLAGS} -I$ENV{MPICH_DIR}/include")
    set(CMAKE_EXE_LINKER_FLAGS "-L$ENV{MPICH_DIR}/lib -lmpi -L$ENV{CRAY_MPICH_ROOTDIR}/gtl/lib -lmpi_gtl_hsa" CACHE STRING "Default flags for this config")
  elseif (${MACHINE_VARIANT} MATCHES "cray")
    set(MACHINE_CXX_FLAGS "${MACHINE_CXX_FLAGS} -I$ENV{ROCM_PATH}/include")
    set(CMAKE_EXE_LINKER_FLAGS "-L$ENV{ROCM_PATH}/lib -lamdhip64" CACHE STRING "Default flags for this config")
  endif()

  # ensure that GPU are properly bound to ranks
  list(APPEND TEST_MPIOPTS "-c1" "--gpus-per-node=8" "--gpu-bind=closest")
else()
  set(PARTHENON_DISABLE_MPI ON CACHE BOOL "Disable MPI")
endif()

set(CMAKE_CXX_FLAGS "${MACHINE_CXX_FLAGS}" CACHE STRING "Default flags for this config")
```


17. Press ESC

![image](https://github.com/user-attachments/assets/c1512fca-2b9c-4384-9f6e-2fabf0ac8ef7)

18. Type: wq

![image](https://github.com/user-attachments/assets/ea8b349b-9249-4e91-9bff-cefe1c3b1ad5)

19. Press Enter

![image](https://github.com/user-attachments/assets/b3a5389e-7dda-403b-84c2-cc40fb868bbf)

This saves this file and allows you to exit.

This will create a folder in your athenapk directory, which is where you can find the athenaPK executable in the bin directory (i.e., cd build/bin).

20. Type: cd build

![image](https://github.com/user-attachments/assets/756e6780-a6d3-4e17-a46f-0dbef9a08196)

21. Type: cd bin

![image](https://github.com/user-attachments/assets/9ac7ae8a-bb3b-45ea-a33d-5b030d3d7965)

22. Type: vim run.sh

![image](https://github.com/user-attachments/assets/511959e4-8cc9-4eed-af55-6403342c0cb4)

23. Press the letter “i” on the keyboard

![image](https://github.com/user-attachments/assets/f5575142-3e4b-437c-929e-247b732203ab)

Below is an example submission script to run the classic problem and generate data to visualize.

Copy and paste everything below into that file:

```
#!/bin/bash


#SBATCH -A GEN243
#SBATCH -J AthenaPK
#SBATCH -o %x-%j.out
#SBATCH -t 00:10:00
#SBATCH -q debug
#SBATCH -N 1


module load cpe/23.09
module load PrgEnv-amd
module load craype-accel-amd-gfx90a
module load cmake
module load hdf5
module load ums
module load ums022
module -t list


date

export MPICH_GPU_SUPPORT_ENABLED=1

srun -N1 -n8 -c7 --ntasks-per-node=8 --gpus-per-node=8 --gpu-bind=closest ./athenaPK \
    -i /ccs/home/wiktoria_zielinska/athenapk/inputs/blast_image.in \
    problem/blast/input_image=/lustre/orion/stf243/world-shared/holmenjk/image.pbm \
    problem/blast/pressure_ratio=350 \
    parthenon/time/nlim=500 \
    parthenon/mesh/numlevel=5 \
    parthenon/mesh/nx1=32 parthenon/mesh/nx2=48 parthenon/mesh/nx3=1 \
    parthenon/meshblock/nx1=4 parthenon/meshblock/nx2=4 parthenon/meshblock/nx3=1 \
    parthenon/output0/dt=0.05 \
    parthenon/output0/variables=cons
```


NOTES:

You’ll need to replace the srun parameter pointing to the input with the path to your input (i.e.,  -i /<path-to-your-athenapk-folder>/athenapk/inputs/blast_image.in \)

This is the line that you will need to change: 

```
srun -N1 -n8 -c7 --ntasks-per-node=8 --gpus-per-node=8 --gpu-bind=closest ./athenaPK \
    -i /ccs/home/wiktoria_zielinska/athenapk/inputs/blast_image.in \
```

![image](https://github.com/user-attachments/assets/3c2d759a-c9b2-484a-9621-050589e60eb5)

24. To run, type: sbatch run.sh

![image](https://github.com/user-attachments/assets/8cb77230-a0a7-41ed-96d1-e34aaeff52dd)

When run, you’ll see output like the below in the directory you ran from after you type “ls” 

![image](https://github.com/user-attachments/assets/463e0d7f-2029-495d-a72c-e5f6d591fc9d)

The numbers may or may not be different

25. To see the status of your job, type: squeue -u USERNAME

Where USERNAME is the same username that you used to log into Frontier.

Your output will either show a time value, or simply say "TIME". You will know when it is done. The image below shows both cases, the top being a job that hasn't finished yet, and the bottom being a job that has finished running. Wait until the job finishes running to move onto the next step.

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/b54c2827-7524-498b-b56e-1a8cd50b7f04)

26. Congratulations! You are now ready to visualize using VisIt!
