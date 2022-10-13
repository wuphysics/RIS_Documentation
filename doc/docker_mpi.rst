MPI on RIS
==========

In order to utilize cross-node communication correctly using MPI, one needs to
at least have several ingredients:

    1. The correct version of MLNX_OFED driver for the InfiniBand connection (https://network.nvidia.com/products/infiniband-drivers/linux/mlnx_ofed/ and the correct version is 5.4-3.1.0.0).

    2. A relatively recent version of UCX libraries.

    3. Compile OpenMPI with the configuration option ``--with-lsf=`` enabled, and pointing correctly to the RIS location of the LSF libraries: ``/opt/ibm/lsfsuite/lsf/10.1``.

The recommended version of OpenMPI seems to be 4.0.1, as I was having troubles
with the latest version of 4.1.4. If one needs CUDA as well, then one needs
to: 1. Obviously install CUDA. Preferrably matching the version on RIS which is
11.6. 2. Compile OpenMPI with option ``--with-cuda=`` pointing to the location of
the CUDA installation. Even with everything configured correctly, one needs to
invoke ``mpirun`` with a bunch of additional parameters, e.g. ``mpirun --mca btl ^vader,tcp,openib,uct --mca pml ucx --mca opal_common_ucx_opal_mem_hooks 1 -x -UCX_NET_DEVICES=mlx5_0:1 -np 8 ./a.out``. The extra parameters are to direct MPI to use the correct
drivers and suppress specific warnings. I also found that starting with
base docker images that are not ``ubuntu:20.04`` may create a shell script
problem that prevents one from running ``./configure`` which is crucial
for compiling custom software packages.
