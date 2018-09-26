Docker image for Arch Linux
===========================

An Arch linux base image.

Build
-----

    docker build -t docker-archlinux .

How I built this image
----------------------

Run the script that builds an Arch chroot, imports into a docker container.

    $ sudo ./mkimage-arch.sh

The resulting output of the script includes the name of docker image.
I used this to extract the filesystem
Use docker to export/save the image:

    $ docker save archlinux -o image.tar

Find the layer.tar file within the docker tar:

    $ tar -tvf image.tar | grep layer.tar

Pull that layer.tar out of the image.tar (see result of prev. command)

    $ tar --extract --file=image.tar c5f3671366de897b9fa72099232f89be3be528d32f5648a88a8768870e291310/layer.tar

Rename and compress:

    $ mv c5f3671366de897b9fa72099232f89be3be528d32f5648a88a8768870e291310/layer.tar ./arch-$(date +%F).tar
    $ xz ./arch-*.tar

Update the Dockfile to ADD the newer arch base system

Commit the updated files to the git repo.

I orginally got the `mkimage-arch.sh` from [here](https://github.com/docker/docker/tree/master/contrib)
and the downloaded `mkimage-arch.sh` and `mkimage-arch-pacman.conf`, like so:

    $ wget https://raw.githubusercontent.com/docker/docker/master/contrib/mkimage-arch-pacman.conf
    $ wget https://raw.githubusercontent.com/docker/docker/master/contrib/mkimage-arch.sh
    $ chmod +x mkimage-arch.sh

