# Getting Started

You can quickly experience MesaLock Linux in the container environment using
Docker.

```sh
$ docker run -it mesalocklinux/mesalock-linux
```

## Building

Currently, MesaLock Linux is provided in two versions: live ISO and rootfs. The
live ISO image can be used to create a bootable live USB, or boot in a virtual
machine. The rootfs (i.e., root file system) can be used as a minimal root
image for a container.

### Requirements

#### Clone MesaLock repository

Clone `mesalock-distro` and `pacakges` repositories.

```sh
$ mkdir mesalock-linux && cd mesalock-linux
$ git clone https://github.com/mesalock-linux/mesalock-distro.git
$ git clone https://github.com/mesalock-linux/packages.git
$ cd mesalock-distro
```

#### Build in Docker

We provide a `Dockerfile` for building MesaLock Linux with all dependencies
installed. You can build the docker image first and then in the building
container environment, you can build packages, live ISO, and rootfs.

```sh
$ docker build --rm -t mesalocklinux/build-mesalock-linux -f Dockerfile.build .
$ docker run -v $(dirname $(pwd)):/mesalock-linux -w /mesalock-linux/mesalock-distro \
    -it mesalocklinux/build-mesalock-linux /bin/bash
```

The image of building environment are also provided from [Docker
Hub](https://hub.docker.com/r/mesalocklinux/build-mesalock-linux/). You can
pull and run the container with the repo name `mesalocklinux/build-mesalock-linux`.

#### Build on Ubuntu

You can also build a Ubuntu machine, please install these building dependencies
first:

```sh
$ # install packages
$ apt-get update && \
  apt-get install -q -y --no-install-recommends \
           curl \
           git \
           build-essential \
           cmake \
           wget \
           bc \
           gawk \
           parallel \
           pigz \
           cpio \
           xorriso \
           fakeroot \
           syslinux-utils \
           uuid-dev \
           libmpc-dev \
           libisl-dev \
           libz-dev \
	   python-pip \
	   python-setuptools \
           software-properties-common

$ # install dependencies of building pypy
$ apt-get install -q -y --no-install-recommends \
        pypy \
        gcc \
        make \
        libffi-dev \
        pkg-config \
        zlib1g-dev \
        libbz2-dev \
        libsqlite3-dev \
        libncurses5-dev \
        libexpat1-dev \
        libssl-dev \
        libgdbm-dev \
        tk-dev \
        libgc-dev \
        python-cffi \
        liblzma-dev \
        libncursesw5-dev

$ # install wheel and sphinx
$ pip install wheel
$ pip install sphinx

$ # install Go
$ add-apt-repository -y ppa:gophers/archive && \
  apt-get update && \
  apt-get install -q -y --no-install-recommends \
           golang-1.9-go

$ # install Rust
$ curl https://sh.rustup.rs -sSf | sh -s -- -y && \
    rustup override set nightly-2018-01-14

$ # setup PATH
$ export PATH="$HOME/.cargo/bin:/usr/lib/go-1.9/bin:$PATH"
```

### Build packages, live ISO, and rootfs

After installing building dependencies, you can run following commands to build
packages, live ISO, and rootfs.

  - First build all packages: `./mkpkg`
  - Build the live ISO: `./mesalockiso`
  - Build the container rootfs: `./mesalockrootfs`
  - Build a specific package only: `./mkpkg <package_name>`

The live ISO (`mesalock-linux.iso`) and rootfs (`rootfs.tar.xz`) can be found
in the `build` directory.

## Trying

MesaLock Linux can be run in real devices (e.g., boot from a Live USB), virtual
machines, and docker containers.

### Virtual machine

You can try MesaLock Linux with Live ISO or in a docker container. Here are
steps to try MesaLock Linux in VirtualBox.

  1. Open VirtualBox and "New" a VM.
  2. In the VM settings, choose `mesalock-linux.iso` as "Optical Drive".
  3. Start the VM and explore MesaLock Linux.

### Docker container

We provide a simple `Dockerfile` for MesaLock Linux. Here are steps to try
MesaLock Linux in a docker container.

  1. Build packages and rootfs: `./mkpkg && ./mesalockrootfs`
  2. Build the docker image: `docker build --rm -t mesalocklinux/mesalock-linux .`
  3. Run the image and expeience MesaLock Linux: `docker run --rm -it mesalocklinux/mesalock-linux`

The latest rootfs image with all pacakges are pushed to [Docker
Hub](https://hub.docker.com/r/mesalocklinux/mesalock-linux/). You can also
directly run the image with the repo name `mesalocklinux/mesalock-linux`.

### Demos

#### Hosting web servers

The `mesalock-demo` package provides several examples and will be installed
under the `/root/mesalock-demo` directory. For instance, we made several web
server demos written in [Rocket](https://github.com/SergioBenitez/Rocket/),
which is a web framework written in Rust.  To try these demos in the VM, please
follow these instructions.

  1. In the VM settings, select "NAT" for network adapter and use port
     forwarding function in the advanced settings to bind host and guest
     machines. Here we add a new rule to bind host IP (`127.0.0.1:8080`) with
     guest IP (`10.0.2.15:8000`).
  2. Start MesaLock Linux.
  3. Bring up all network devices. Here we use `ip` command:

    ```
    $ ip link set lo up
    $ ip link set eth0 up
    ```

  4. Setup IP address of the network devices.

    ```
    $ ip address add 10.0.2.15/24 dev eth0
    ```

  5. Run a web server.

    ```
    $ cd /root/mesalock-demo/rocket-hello-world && ./hello_world
    $ # or
    $ cd /root/mesalock-demo/rocket-tls && ./tls
    ```

  6. Finally, connect to the web server using a browser. In this example, type
     in `http://127.0.0.1:8080` in the browser.

You can also try our demos in the docker image directly.

  1. Run the MesaLock docker and export port 8000 to 8000: `docker run -it -p 8000:8000 mesalocklinux/mesalock-linux`
  2. Run a web server in the `/root/mesalock-demo/` directory.
  3. Visit the website in the browser.

#### Working on machine learning tasks

[Rusty-machine](https://github.com/AtheMathmo/rusty-machine) is a general
purpose machine learning library implemented entirely in Rust. We put several
demo examples of machine learning tasks in the `mesalock-demo` package. You can
find them in the `/root/mesalock-demo/rusty-machine/` directory.
