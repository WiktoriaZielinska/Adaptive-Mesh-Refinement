1. Open CMD on Windows.

2. Log into Frontier using your XCAMS/UCAMS username and password: ssh USERNAME@frontier.olcf.ornl.gov
NOTES:
The password is your pin + your 6 digit RSA code

3. Type: git clone https://github.com/parthenon-hpc-lab/athenapk.git athenapk

4. Type: cd athenapk

5. Type: git submodule init

6. Type: git submodule update

7. Type: module load cpe/23.09

8. Type: module load PrgEnv-amd

9. Type: module load craype-accel-amd-gfx90a

10. Type: module load cmake

11. Type: module load hdf5

12. cd into athenapk if you are not already there.

13. Type: ls

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/82c7e6b0-6543-420b-a45a-75197a2c9ab8)

14. Type: vim FrontierAndCrusher.cmake

15. Press the letter “i” on the keyboard.

16. Copy and paste everything below into that file. Do not copy the stuff after the “STOP COPYING HERE, DO NOT INCLUDE THIS” text.

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




STOP COPYING HERE, DO NOT INCLUDE THIS
STOP COPYING HERE, DO NOT INCLUDE THIS
STOP COPYING HERE, DO NOT INCLUDE THIS

17. Press ESC

18. Type: wq

19. Press Enter

This saves this file and allows you to exit.

This will create a folder in your athenapk directory, which is where you can find the athenaPK executable in the bin directory (i.e., cd build/bin).

20. Type: cd build

21. Type: cd bin

22. Type: vim run.sh

23. Press the letter “i” on the keyboard

Below is an example submission script to run the classic problem and generate data to visualize.

Copy and paste everything below into that file. Do not copy the stuff after the “STOP COPYING HERE, DO NOT INCLUDE THIS” text.

#!/bin/bash


#SBATCH -A GEN243
#SBATCH -J AthenaPK
#SBATCH -o %x-%j.out
#SBATCH -t 00:03:00
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
    problem/blast/pressure_ratio=1000 \
    parthenon/time/nlim=50 \
    parthenon/mesh/numlevel=5 \
    parthenon/mesh/nx1=32 parthenon/mesh/nx2=48 parthenon/mesh/nx3=1 \
    parthenon/meshblock/nx1=4 parthenon/meshblock/nx2=4 parthenon/meshblock/nx3=1 \
    parthenon/output0/dt=0.05 \
    parthenon/output0/variables=cons




STOP COPYING HERE, DO NOT INCLUDE THIS
STOP COPYING HERE, DO NOT INCLUDE THIS
STOP COPYING HERE, DO NOT INCLUDE THIS

NOTES:

You’ll need to replace the srun parameter pointing to the input with the path to your input (i.e.,  -i /<path-to-your-athenapk-folder>/athenapk/inputs/blast_image.in \)

This is the line that you will need to change: 

srun -N1 -n8 -c7 --ntasks-per-node=8 --gpus-per-node=8 --gpu-bind=closest ./athenaPK \
    -i /ccs/home/wiktoria_zielinska/athenapk/inputs/blast_image.in \

24. To run, type: sbatch run.sh

When run, you’ll see output like the below in the directory you ran from after you type “ls” 

athenaPK  AthenaPK-2044267.out  parthenon.out0.00000.phdf  parthenon.out0.00000.phdf.xdmf  parthenon.out0.00001.phdf  parthenon.out0.00001.phdf.xdmf  run.sh

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/8a06cc24-f5db-47e2-ad11-477d346e7f32)

The numbers may or may not be different

25. To see the status of your job, type: squeue -u USERNAME

Where USERNAME is the same username that you used to log into Frontier.

Your output will either show a time value, or simply say "TIME". You will know when it is done. The image below shows both cases, the top being a job that hasn't finished yet, and the bottom being a job that has finished running.

![image](https://github.com/WiktoriaZielinska/Adaptive-Mesh-Refinement/assets/112288108/b54c2827-7524-498b-b56e-1a8cd50b7f04)

















