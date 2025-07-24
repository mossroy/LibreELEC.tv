# LibreELEC

LibreELEC is a 'Just enough OS' Linux distribution for the award-winning [Kodi](https://kodi.tv) software on popular mediacentre hardware. Further information on the project can be found on the [LibreELEC website](https://libreelec.tv).

## About this fork

This fork allows to run a Kubernetes node (with [k3s](https://k3s.io/)) on the same device as . The goal is to take advantage of the resources of a 24/7 running mediacentre.

This fork also allows to use [Longhorn](https://longhorn.io/) for the kubernetes storage (which requires iSCSI support). It has also been tested with NFS and local-path.

Regarding networking, this fork also allows to use [Calico](https://docs.tigera.io/calico/). But it also works with the default CNI of k3s (Flannel)

### How to compile

Reference: https://wiki.libreelec.tv/development/build-docker

The only difference is to pass `BUILDER_NAME` variable, to help LibreElec maintainers recognize this variant:

    docker build --pull -t libreelec tools/docker/focal
    docker run -it --rm --log-driver none -v `pwd`:/build -w /build --memory "6g" --memory-swap "6g" --cpus "6" -e PROJECT=Generic -e ARCH=x86_64 -e MTPROGRESS=yes -e BUILDER_NAME=mossroy -e libreelec make image

(Adjust cpu to your hardware)

### Binaries

x86-64 binaries are available on https://download.mossroy.fr/LibreELEC/

### How to install k3s

A prerequisite is to create some directories on your device, to allow persistent storage of some directories. Login with SSH on it, and run the following commands (some of them might be skipped, depending on you usage, but it won't hurt to run them all):

    mkdir -p /storage/persistent-fs-dirs/k3s-systemd
    mkdir -p /storage/persistent-fs-dirs/k3s-bin
    mkdir -p /storage/persistent-fs-dirs/var-lib
    mkdir -p /storage/persistent-fs-dirs/etc-rancher
    mkdir -p /storage/persistent-fs-dirs/etc-containerd
    mkdir -p /storage/persistent-fs-dirs/usr-libexec-kubernetes
    # For Calico
    mkdir -p /storage/persistent-fs-dirs/etc-cni
    mkdir -p /storage/persistent-fs-dirs/etc-calico
    mkdir -p /storage/persistent-fs-dirs/opt

The following deploys a k3s node for an existing k3s cluster. However, it certainly works to deploy it as a control-plane, too (untested).

Login with SSH on your LibreElec device, and install it with:

    curl -sfL https://get.k3s.io | INSTALL_K3S_SYSTEMD_DIR=/storage/persistent-fs-dirs/k3s-systemd INSTALL_K3S_BIN_DIR=/storage/persistent-fs-dirs/k3s-bin K3S_URL=https://your-control-plane:6443 K3S_TOKEN=K101bfdda519dc15b3c9400bf91d8ea70961152d44f686bc3feee481481f76157ad::server:01ad7a756859e4a9133a7ebd2c5d54cc sh -s -

(replace K3S_URL and K3S_TOKEN accordingly)

## Original mentions of LibreElec

**Issues & Support**

Please ask questions in the [LibreELEC forum: Help & Support](https://forum.libreelec.tv/forum-3.html) or ask a member of project staff in the #libreelec IRC channel on Libera.Chat. Please report bugs via [GitHub Issues](https://github.com/LibreELEC/LibreELEC.tv/issues).

**Donations**

Contributions towards current project funding goals can be made via [OpenCollective](https://opencollective.com/libreelec/donate).

**License**

LibreELEC original code is released under [GPLv2](https://www.gnu.org/licenses/gpl-2.0.html).

**Copyright**

As LibreELEC includes code from many upstream projects it has many copyright owners; notably [OpenELEC](https://openelec.tv) which we forked from after disagreeing with project direction and management, and [OpenBricks/GeeXboX](https://github.com/OpenBricks/openbricks/blob/master/AUTHORS) the uncredited source of the original 2009 build system. LibreELEC makes no claim of copyright on any upstream code. However all original LibreELEC authored code is copyright LibreELEC.tv. Patches to upstream code have the same license as the upstream project unless specified otherwise. For a complete copyright list please checkout the source code to examine license headers. Unless expressly stated otherwise all code submitted to the LibreELEC project (in any form) is licensed under [GPLv2](https://www.gnu.org/licenses/gpl-2.0.html) and copyright is donated to the project. This approach gives the project freedom to maintain the code without the overhead of preserving contact with every submitter, e.g. GPLv3. You are free to retain copyright by adding your copyright header to each submitted code page. If you submit code that is not your own work it is your responsibility to place a header stating the copyright.
