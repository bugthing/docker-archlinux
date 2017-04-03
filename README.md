Docker image for Arch Linux
===========================

An Arch linux base image.

Build
-----

    docker build -t docker-archlinux .

How I built this image
----------------------

When to https://github.com/docker/docker/tree/master/contrib and the downloaded
mkimage-arch.sh and mkimage-arch-pacman.conf.
I then ran the script to create a container using my running docker daemon.
Once created, I extracted the filesystem from the container and put into a .tar.xz file
which I use when building this container.

  wget https://raw.githubusercontent.com/docker/docker/master/contrib/mkimage-arch-pacman.conf
  wget https://raw.githubusercontent.com/docker/docker/master/contrib/mkimage-arch.sh
  chmod +x mkimage-arch.sh
  sudo ./mkimage-arch.sh

The resulting output of the script included a sha, I used this to extract the filesystem

Use docker to export/save the image:

  docker save cb6bfa01c9f3619eb195575fbe6397a3528d7a0f9536fa47194e9f6535011958 -o image.tar

Find the layer.tar file within the docker tar:

  tar -tvf image.tar | grep layer.tar

Pull that layer.tar out of the image.tar (see result of prev. command)

  tar --extract --file=image.tar c5f3671366de897b9fa72099232f89be3be528d32f5648a88a8768870e291310/layer.tar

Rename and compress:

  mv c5f3671366de897b9fa72099232f89be3be528d32f5648a88a8768870e291310/layer.tar ./arch-$(date +%F).tar
  xz ./arch-*.tar

