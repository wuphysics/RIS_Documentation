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
