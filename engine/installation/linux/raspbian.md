---
redirect_from:
- /engine/installation/raspbian/
description: Instructions for installing Docker on Raspbian.
keywords: Docker, Docker documentation, installation, raspbian
title: Install on Raspbian
---

Docker is supported on the following versions of Raspbian:

 - *Raspbian Jessie*

 >**Note**: If you previously installed Docker using `APT`, make sure you update
 your `APT` sources to the new `APT` repository.

## Prerequisites

 Docker requires  your kernel to be 3.10 at minimum. The latest 3.10 minor
 version or a newer maintained version are also acceptable.

 Kernels older than 3.10 lack some of the features required to run Docker
 containers. These older versions are known to have bugs which cause data loss
 and frequently panic under certain conditions.

 To check your current kernel version, open a terminal and use `uname -r` to
 display your kernel version:

     $ uname -r

### Update your apt repository

Docker's `APT` repository contains Docker 1.12.1 and higher. To set `APT` to use
from the new repository:

 1. If you haven't already done so, log into your machine as a user with `sudo` or `root` privileges.

 2. Open a terminal window.

 3. Update package information, ensure that APT works with the `https` method, and that CA certificates are installed.

         $ apt-get update
         $ apt-get install apt-transport-https ca-certificates

 4. Add the new `GPG` key.

         $ apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

 5. Open the `/etc/apt/sources.list.d/docker.list` file in your favorite editor.

     If the file doesn't exist, create it.

 6. Remove any existing entries.

 7. Add an entry for your Raspbian operating system.

        deb https://apt.dockerproject.org/repo raspbian-jessie main

 8. Save and close the file.

 9. Update the `APT` package index.

         $ apt-get update

 10. Verify that `APT` is pulling from the right repository.

         $ apt-cache policy docker-engine

     From now on when you run `apt-get upgrade`, `APT` pulls from the new apt repository.

## Install Docker

Before installing Docker, make sure you have set your `APT` repository correctly as described in the prerequisites.

1. Update the `APT` package index.

        $ sudo apt-get update

2. Install Docker.

        $ sudo apt-get install docker-engine

3. Start the `docker` daemon.

        $ sudo service docker start

4. Verify `docker` is installed correctly.

        $ sudo docker run armhf/hello-world

    This command downloads a test image and runs it in a container. When the
    container runs, it prints an informational message. Then, it exits.


## Giving non-root access

The `docker` daemon always runs as the `root` user and the `docker`
daemon binds to a Unix socket instead of a TCP port. By default that
Unix socket is owned by the user `root`, and so, by default, you can
access it with `sudo`.

If you (or your Docker installer) create a Unix group called `docker`
and add users to it, then the `docker` daemon will make the ownership of
the Unix socket read/writable by the `docker` group when the daemon
starts. The `docker` daemon must always run as the root user, but if you
run the `docker` client as a user in the `docker` group then you don't
need to add `sudo` to all the client commands. From Docker 0.9.0 you can
use the `-G` flag to specify an alternative group.

> **Warning**:
> The `docker` group (or the group specified with the `-G` flag) is
> `root`-equivalent; see [*Docker Daemon Attack Surface*](../../security/security.md#docker-daemon-attack-surface) details.

**Example:**

    # Add the docker group if it doesn't already exist.
    $ sudo groupadd docker

    # Add the connected user "${USER}" to the docker group.
    # Change the user name to match your preferred user.
    # You may have to logout and log back in again for
    # this to take effect.
    $ sudo gpasswd -a ${USER} docker

    # Restart the Docker daemon.
    $ sudo service docker restart

## Upgrade Docker

To install the latest version of Docker with `apt-get`:

    $ apt-get upgrade docker-engine

## Uninstall

To uninstall the Docker package:

    $ sudo apt-get purge docker-engine

To uninstall the Docker package and dependencies that are no longer needed:

    $ sudo apt-get autoremove --purge docker-engine

The above commands will not remove images, containers, volumes, or user created
configuration files on your host. If you wish to delete all images, containers,
and volumes run the following command:

    $ rm -rf /var/lib/docker

You must delete the user created configuration files manually.

## What next?

Continue with the [User Guide](../../userguide/index.md).
