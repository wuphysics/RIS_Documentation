Package Management for Docker Images
====================================

One potential problem for the Docker system is that when the user has too many
software packages, especially large ones such as the Intel oneAPI or the CUDA
Toolkit, then the Docker image becomes very large and difficult to use. A
potential way to solve this problem is to install these packages in the storage
partition instead of the Docker image itself.

Take CUDA toolkit as an example. The install size of the complete CUDA Toolkit
is about 5GB. However, one can install it in an interactive bash session on the
RIS, as long as they point the installation prefix to a mounted storage
location. A step-by-step guide is as follows:

0. Download your package installer. We will be using CUDA 11.7.1 as an example::

     cd /storage1/fs1/${COMPUTE_ALLOCATION}/Active
     wget https://developer.download.nvidia.com/compute/cuda/11.7.1/local_installers/cuda_11.7.1_515.65.01_linux.run

   Substitute ``${COMPUTE_ALLOCATION}`` with the name of your storage
   allocation. It is very likely just the username of the PI.

1. Start with a base Docker image. We will be using ``ubuntu:20.04``. Request an
   interactive session with your storage partition mounted at a specific system
   location::

    export LSF_DOCKER_VOLUMES='/storage1/fs1/${COMPUTE_ALLOCATION}/Active/modules:/usr/local/modules /storage1/fs1/${COMPUTE_ALLOCATION}/Active:/storage1'
    cd
    bsub -Is -G ${COMPUTE_ALLOCATION} -q general-interactive -a 'docker(ubuntu:20.04)' bash

   The ``LSF_DOCKER_VOLUMES`` environment variable has the format ``source:destination``, where ``source`` is the location accessible to the login node, while ``destination`` is on your Docker image. Different mounts are separated by space. We are also mounting the whole active storage to ``/storage1`` so that we can access our installer. The interactive session will always mount the current directory you are at, so it is often useful to ``cd`` back to home before calling ``bsub``, so that you retain access to your home directory.

2. Now you are in the ``bash`` prompt of your Docker image. We can go to ``/storage1`` and run the CUDA installer. Remember to add ``--installpath`` pointing towards the other mounted directory::

     cd /storage1
     bash cuda_11.7.1_515.65.01_linux.run --silent --driver --toolkit --installpath=/usr/local/modules/cuda-11.7.1

   Unfortunately this will not work out of the box since we are missing some CUDA dependencies (e.g. ``libxml2``) in the base ``ubuntu:20.04`` image. The steps outlined above is mostly to demonstrate a principle. This principle applies to many other packages too, especially software that you want to build from source. In that case, you would want to do ``./configure --prefix=/usr/local/modules/name-of-software-with-version`` before building and installing. One of the problems with the Docker environment on the RIS is that users do not have superuser privileges. This somewhat negates that since you should always have write permission in the directory you mounted, which is ``/usr/local/modules``. Other choices for the mounting point can be e.g. ``/etc/modules``, ``/opt/modules``.

3. Before we are completely done, it is useful to build a Docker image that knows the location of the software you installed with the help of environment variables. Normally you could do this with a line ``ENV VARIABLE value`` in the ``Dockerfile``, but on RIS the ``ENV`` lines do not seem to be propagated, so one can instead write them into the global ``bashrc`` file located at ``/etc/bashrc``::

     FROM ubuntu:20.04

     RUN echo "export PATH=/usr/local/modules/cuda-11.7.1/bin:$PATH" >> /etc/bashrc

This method also has its own drawbacks. One example is what we have encountered: if the installer has some external dependencies, then one needs to start with a Docker image with that installed. The other problem is that anything in the ``/storage1`` partition automatically do not have execute permission. This can be problematic for installers that extract into a temporary location, then run the install scripts. Your best bet is to instruct the installer to extract into your home directory, and run the installation there.
