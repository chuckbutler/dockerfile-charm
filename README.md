# Dockerfile Charm

Deploy a repository containing a dockerfile with the `docker-build-hook` charm.


This charm predicates a repository containing potentially 3 files

- A Dockerfile
    - A typical Dockerfile + any supporting files to create the container
- A build script
    - Any script, bash / python / etc to build the docker container
- A Start Script
    - Called at the end of the build script, or via `build-hook.py` provided by the `docker-build-hook` charm.

Assumptions
----------

You want to build the container on the host. *not suitable for production*

If you wish to build in paralell with your running container - use the `build-hook.py` script.

This charm is intended to be deployed to the singluar Docker host you wish to
run/build the container. If you have multiple docker-hosts, this can be achieved
by simply targeting the hosts.

    juju deploy dockerfile --to 1
    juju add-unit dockerfile --to 3

assuming hosts 1, and 3 are Docker infrastructure charms.

Once the `dockerfile` charm is deployed, and configuration is set - simply add
the relationship to the `docker-build-hook` charm over the **project** relationship.

> **!NOTE!** You must have the Build script in the repository BEFORE deploying
the charm and relating to the `docker-build-hook` service. The builds will
consistently fail otherwise.

Deploy
-----

    juju deploy trusty/docker
    juju deploy cs:~lazypower/trusty/dockerfile --to 1
    juju deploy cs:~lazypower/trusty/docker-build-hook
    juju add-relation docker-build-hook:project dockerfile:project


Caveats
------

Several, experiment and file bugs please!

