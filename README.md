# Physics Department RIS Documentations

This is the GitHub repository for the documentation on how to use the WashU
Research Infrastructure Services (RIS), written by people from the WashU physics
department. The main goal of this repository is to provide a more specialized
tutorial for users in the physics department who plan to utilize RIS for
high-performance computing (HPC) purposes.

This repo contains two parts: some tutorials on how to use the WashU RIS system,
and some `Dockerfile`s that we gathered from individual groups. The
`Dockerfile`s are in the `Dockers` directory, while the source files of the
documentation are in the `doc` directory. The documentation is hosted via
`gh-pages` at: https://wuphysics.github.io/RIS_Documentation/

# Dockerfiles included

`Dockerfile.cuda_hpc` is the one used by Alex Chen and Yajie Yuan's group,
configured with `OpenMPI`, `Cuda`, `HDF5`, `gcc`, and a bunch of development
libraries. It is hosted at `fizban007/hpc_ubuntu` on the Docker Hub.

`Dockerfile.demo_mpi` is supplied by the RIS staff Shin Leong. It is more of a
proof-of-concept Dockerfile since it probably does not work directly on the RIS
docker build system. The reason for that is when you do `docker_build` on the
RIS, the LSF libraries are not mounted by default at
`/opt/ibm/lsfsuite/lsf/10.1`. But compiling OpenMPI requires the LSF libraries.
Therefore it is easier to go into an interactive session to do the compiling.

`Dockerfile.raytrace` is supplied by Andrew West, who uses this environment for
his black hole ray tracing studies.
