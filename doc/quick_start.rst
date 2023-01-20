RIS Quick Start
===============

Getting an account
------------------

In order to get an account, go to this link:
https://ris.wustl.edu/services/compute/resources/ and click “Request Service”.
You’ll need to log in with your WUSTL Key. Then, click on the first link, which
is “Request a NEW Compute Service”. You’ll need to fill a form, indicating your
PI. Ask your PI about the cost center number, which should be the same for a
given department.

Logging in
----------

There are 4 log-in nodes that you can connect to: ``client-1``, ``client-2``, ``client-3``,
and ``client-4``. You can connect to e.g. ``client-1`` using:

.. code-block:: bash

    ssh wustlkey@compute1-client-1.ris.wustl.edu

Substitute you own WUSTL Key for ``wustlkey``, and substitute ``client-1`` for
other clients like ``client-3``. Note that you’ll need to use the WashU VPN if
you connect from outside the campus: https://it.wustl.edu/items/connect/.

It is recommended to set up an SSH key for authentication. A guide is provided
by the RIS here:
https://docs.ris.wustl.edu/doc/compute/recipes/ssh-keypair.html#ris-ssh-keypair.

Using Docker
------------

Almost everything you do on the RIS cluster needs to be done through a Docker
container. A Docker container is an immutable environment that contains whatever
software packages you need. For more information, visit the Docker website:
https://www.docker.com/.

One can use a basic Docker image such as ``ubuntu/20.04``. The following command
will pull the image from Docker Hub, request an interactive session on the RIS
cluster, and put you under a bash prompt:

.. code-block:: bash

    bsub -Is -G compute-USERNAME -q general-interactive -a 'docker(ubuntu:20.04)' bash

The part following ``-G`` is the PI’s compute group account (often the username
of the PI), and the path in parenthesis is the Docker image path on Docker Hub.
This command will request a compute node, pull the Docker image from the Hub
(which takes a few minutes), and finally put you in an interactive bash
environment.

Due to the nature of Docker, any packages you install while you are in the
Docker container is not saved. Fortunately, your home directory from the RIS
system is mounted in the Docker container. Therefore, it is possible to make
modifications to the content of your home directory while you are in the Docker
container, and retain those modifications even after you log out. One can also
mount the scratch filesystem and write the simulation data there. For example,
you can export the following environment variable before you launch ``bsub``:

.. code-block:: bash

    export LSF_DOCKER_VOLUMES='/scratch1/fs1/${COMPUTE_ALLOCATION}:/scratch1/fs1/${COMPUTE_ALLOCATION}'

where ${COMPUTE_ALLOCATION} may be your username or the PI's. This will mount
the scratch partition to the same location inside the Docker container. Storage
mounting is documented here:
https://docs.ris.wustl.edu/doc/compute/recipes/job-execution-examples.html#start-an-interactive-job-with-access-to-all-my-data
