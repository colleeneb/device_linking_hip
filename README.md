# Test code to show linking issue

This shows a simple example where we wanted to define a global device
variable in one object file and use it in another.

This does not work with CHIP-SPV with -fgpu-rdc, but works with CUDA with -rdc=true.

## For CHIP-SPV on JLSE:

```
> module use /soft/modulefiles
> module use /home/pvelesko/local/modulefiles
> module use /home/bertoni/projects/p075.hipblas/testing/chip-spv/modulefiles/

> module purge
> module load chip-spv-new
> module load intel_compute_runtime

> hipcc -c -fPIE -fgpu-rdc GPU_HGP_kernels.cpp
> hipcc -c -fPIE -fgpu-rdc scf.cpp
> hipcc -fPIE -fgpu-rdc --hip-link scf.o GPU_HGP_kernels.o -o exe
clang-15: error: unable to execute command: Executable "lld" doesn't exist!
clang-15: error: amdgcn-link command failed with exit code 1 (use -v to see invocation)

failed to execute:/gpfs/jlse-fs0/users/pvelesko/install/clang/clang15/clang15-spirv-omp/bin/clang++ -D__HIP_PLATFORM_SPIRV__= -L/home/bertoni/projects/p075.hipblas/testing/new-chip-spv/chip-spv/build/install/lib -lCHIP -Wl,-rpath,/home/bertoni/projects/p075.hipblas/testing/new-chip-spv/chip-spv/build/install/lib -I//home/bertoni/projects/p075.hipblas/testing/new-chip-spv/chip-spv/build/install/include -fPIE -fgpu-rdc --hip-link scf.o GPU_HGP_kernels.o -o exe


## For CUDA:

```
> nvcc -x cu -rdc=true -c GPU_HGP_kernels.cpp
> nvcc -c scf.cpp
> nvcc  scf.o GPU_HGP_kernels.o
```
