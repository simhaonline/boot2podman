# buildah

Adopted from: [install buildah](https://github.com/containers/buildah/blob/master/install.md).

## Description

`buildah` is a command line tool that can be used to

* create a working container, either from scratch or using an image as a starting point
* create an image, either from a working container or via the instructions in a Dockerfile
* images can be built in either the OCI image format or the traditional upstream docker image format
* mount a working container's root filesystem for manipulation
* unmount a working container's root filesystem
* use the updated contents of a container's root filesystem as a filesystem layer to create a new image
* delete a working container or an image
* rename a local container

## Prerequisites

First, install and set up [podman](building_podman.md).

It describes most things also needed by buildah, including "compiletc" and "go".

### runc Requirement

Buildah uses `runc` to run commands when `buildah run` is used, or when `buildah build-using-dockerfile`
encounters a `RUN` instruction, so you'll also need to build and install a compatible version of runc for Buildah to call for those cases.

### CNI Requirement

When Buildah uses `runc` to run commands, it defaults to running those commands
in the host's network namespace.  If the command is being run in a separate
user namespace, though, for example when ID mapping is used, then the command
will also be run in a separate network namespace.

A newly-created network namespace starts with no network interfaces, so
commands which are run in that namespace are effectively disconnected from the
network unless additional setup is done.  Buildah relies on the CNI library
and plugins to set up interfaces and routing for network namespaces.

## Build and Run Dependencies

### Build

``` console
$ tce-load -wi libseccomp-dev lvm2-dev glib2-dev gpgme-dev

$ go get -d github.com/containers/buildah
$ cd $GOPATH/src/github.com/containers/buildah
$ sed -e 's|"/etc/cni|"/usr/local/etc/cni|' -i util/types.go
$ sed -e 's|/usr/libexec/cni:/opt/cni/bin|/usr/local/lib/cni|' -i util/types.go
$ make buildah
$ sudo install -D -m0755 buildah /usr/local/bin/buildah
$ cd -
```

### Runtime

``` console
$ tce-load -wi libseccomp glib2 gpgme
$ tce-load -i buildah.tcz
```

In order to work on tmpfs, buildah needs to use `no_pivot_root`:

``` sh
export BUILDAH_NOPIVOT=true
```
