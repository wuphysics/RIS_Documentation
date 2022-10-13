# Temporary Repo for A&S RIS Documentations

This repo contains two parts: some documentation on the WashU RIS system, and some `Dockerfile`s that we gathered. The `Dockerfile`s are in the `Dockers` directory, while the source files of the documentation are in the `doc` directory. The documentation is hosted as `gh-pages` at: http://www.pulsarmagnetosphere.com/RIS_Documentation/

# Dockerfiles included

`Dockerfile.cuda_hpc` is my current one, configured with `OpenMPI`, `Cuda`, `HDF5`, `gcc`, and a bunch of development libraries. It is hosted at `fizban007/hpc_ubuntu` on the Docker Hub.

`Dockerfile.demo_mpi` is supplied by the RIS staff Shin Leong. It is more of a proof-of-concept Dockerfile since it probably does not work directly on the RIS docker build system. The reason for that is when you do `docker_build` on the RIS, the LSF libraries are not mounted by default at `/opt/ibm/lsfsuite/lsf/10.1`. But compiling OpenMPI requires the LSF libraries. Therefore it is easier to go into an interactive session to do the compiling.

`Dockerfile.raytrace` is supplied by Andrew West, who uses this environment for his black hole ray tracing studies. 
