# docker compose 101
This is project is simple example how to use [bind mount](https://docs.docker.com/engine/storage/bind-mounts/) (dicrectly mount external directories into contianers) and orchestrate multiple services under one umbrella

## docker compose
Docker compose allows to run and configure multiple container by one file `docker-compose.yaml`. In this example 2 nginx containers used to expose `/mnt/freebsd/` loop mount and `/mnt/ubunutu` mount.

### How we build and why?
Instead of using `image` definition for service `Dockerfile` definition is used just to provide exemple of build and how it is easy and flexibile to build and customize container.

When we run docker compose first time build happens automatically. After that to update images if `Dockerfile` changes we need to execute build command specifically

    docker compose build

### bindmount configuration
Bind mount configuration allow to `mount` any file or directory into container before it starts. In `nginx-ubuntu` example, we mount `default.conf` nginx configuration to enable index for `/` path. This handy when we want to avoid rebuild on every `default.conf` change.

### Build time configuration
For `nginx-freebsd` we copy `default.conf` into image. It will have same effect as in previous section but if/when we need to change it we need rebuild image.

### Mounting external sources
at current level I assume that both `/mnt` mount are accesisble to everyone.

## How to test this setup.

    mkdir iso
    cd iso
    wget https://cdimage.ubuntu.com/releases/22.04/release/ubuntu-22.04.5-live-server-arm64+largemem.iso
    wget https://download.freebsd.org/releases/ISO-IMAGES/14.2/FreeBSD-14.2-RELEASE-arm64-aarch64-bootonly.iso
    sudo mount -o loop ubuntu-22.04.5-live-server-arm64+largemem.iso /mnt/ubuntu/
    sudo mkdir /mnt/ubuntu
    sudo mkdir /mnt/freebsd
    sudo mount -o loop FreeBSD-14.2-RELEASE-arm64-aarch64-bootonly.iso /mnt/freebsd
    sudo mount -o loop ubuntu-22.04.5-live-server-arm64+largemem.iso /mnt/ubuntu/

    git clone
    cd docker-bind-demo
    docker compose up -d
    curl localhost:5000
    curl localhost:6000

    docker compose down