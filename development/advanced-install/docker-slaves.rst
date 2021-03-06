Test/Dev using Docker Worker Nodes
----------------------------------

Digital Rebar developers are *strongly* encouraged to always build and test
deployment code in multi-node situations. However, this practice on VMs
or physical servers has required significant computer resources. With
Docker, developers and testers can spin up a working multi-node
environment much more quickly and with lower resource requirements.
While the containerized nodes are not fully equivalent, they are more
than enough for the vast majority of deployment scenarios.

Benefits:

-  lower resources requirements on development and test systems
-  much faster bring up times (no operating systems to install and boot)
-  very consistent and repeatable system configuration
-  good separation of nodes helps find of issues related to multi-node
   deployment

    Containers do still use system resources! Do not assume that you can
    start unlimited containers - watch your RAM and CPU utilization.

Not Currently Available (but expected):

-  support heterogeneous Linux operating systems (important for testing)
-  deploy across multiple physical nodes (import for scale)
-  use of multiple NICs (converts all conduits to eth0 for now)

Using docker-slaves with *docker-slaves* script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Prereqs
^^^^^^^

1. Make sure that your Docker is updated to at least Docker 1.7. There
   are timing problems with previous versions and SSDs that cause the
   devicemapper or btrfs to not find volumes or mounts during the bring
   up process. You can update here:
   https://docs.docker.com/installation/#installation
2. Pull the slave container: ``docker pull digitalrebar/ubuntu-slave``

    The docker-slaves script uses the docker-slave script. They are
    different!

Wait until the admin node is up and admin node annealing is complete!

Start Up
^^^^^^^^

From the dev system, ``tools/docker-slaves <number of slaves>``

This creates the number of Docker nodes requested using the Digital Rebar CLI on
the Admin node. This script relies on ``ssh root@172.17.0.2`` to access
the Digital Rebar CLI and will fail if that access is not available. However
configuring keys and ssh is part of the normal ``docker-admin`` script
process.

    if your Admin IP is not 172.17.0.2, then will need to change the
    *temporarily* hard coded

It will run up to 40 of them all under TMux if they were created
successfully. Please review `TMux
intro <http://code.tutsplus.com/tutorials/intro-to-tmux--net-33889>`__
to learn how to use this tool. Quick tips are: Ctrl-B then N(ext),
P(revious) X(close)

    Detaching from the tmux session will kill all the nodes!

Docker slaves is currently hard coded to only work when the admin node
is running in a container as well.

Using Docker Slaves without the script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please expand this section!

The critical step is to create the node with the 'rebar-docker-node'
role.
