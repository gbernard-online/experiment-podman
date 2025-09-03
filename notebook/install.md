# EXPERIMENT PODMAN

## INSTALL - PODMAN - DEBIAN 12

[![Podman](img/podman.webp "Podman")](https://podman.io/)
[![Debian](img/debian.webp "Debian")](https://debian.org)

REF: https://podman.io/docs/installation

```bash
$ apt-cache policy podman
podman:
  Installed: (none)
  Candidate: 4.3.1+ds1-8+deb12u1+b1
  Version table:
     4.3.1+ds1-8+deb12u1+b1 500
        500 http://deb.debian.org/debian bookworm/main amd64 Packages

$ sudo apt-get install --yes podman
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  aardvark-dns buildah catatonit conmon containernetworking-plugins crun fuse-overlayfs fuse3 golang-github-containers-common
  golang-github-containers-image libfuse3-3 libgpgme11 libslirp0 libsubid4 libyajl2 netavark slirp4netns uidmap
Suggested packages:
  containers-storage docker-compose
The following NEW packages will be installed:
  aardvark-dns buildah catatonit conmon containernetworking-plugins crun fuse-overlayfs fuse3 golang-github-containers-common
  golang-github-containers-image libfuse3-3 libgpgme11 libslirp0 libsubid4 libyajl2 netavark podman slirp4netns uidmap
0 upgraded, 19 newly installed, 0 to remove and 0 not upgraded.
Need to get 27.0 MB of archives.
...
Created symlink /etc/systemd/system/default.target.wants/podman-auto-update.service → /lib/systemd/system/podman-auto-update.service.
Created symlink /etc/systemd/system/timers.target.wants/podman-auto-update.timer → /lib/systemd/system/podman-auto-update.timer.
Created symlink /etc/systemd/system/default.target.wants/podman-restart.service → /lib/systemd/system/podman-restart.service.
Created symlink /etc/systemd/system/default.target.wants/podman.service → /lib/systemd/system/podman.service.
Created symlink /etc/systemd/system/sockets.target.wants/podman.socket → /lib/systemd/system/podman.socket.
Setting up fuse3 (3.14.0-4) ...
update-initramfs: deferring update (trigger activated)
Setting up fuse-overlayfs (1.10-1) ...
Setting up buildah (1.28.2+ds1-3+deb12u1+b1) ...
Processing triggers for libc-bin (2.36-9+deb12u10) ...
Processing triggers for man-db (2.11.2-2) ...
Processing triggers for initramfs-tools (0.142+deb12u3) ...
update-initramfs: Generating /boot/initrd.img-6.1.0-37-amd64

# $ systemctl --user enable podman.socket
# Created symlink /home/user/.config/systemd/user/sockets.target.wants/podman.socket → /usr/lib/systemd/user/podman.socket.

# $ systemctl --user enable podman-restart.service
# Created symlink /home/user/.config/systemd/user/default.target.wants/podman-restart.service → /usr/lib/systemd/user/podman-restart.service.

# $ sudo loginctl enable-linger user

# $ sudo reboot

```

```bash
# $ podman --version
# podman version 4.3.1

# $ podman info
# ...

# $ podman info --format='{{ .Host.Arch }}'
# amd64

# $ podman info --format='{{ .Version.Version }}'
# 4.3.1

# $ podman container run --quiet --rm quay.io/podman/hello
# !... Hello Podman World ...!

#          .--"--.
#        / -     - \
#       / (O)   (O) \
#    ~~~| -=(,Y,)=- |
#     .---. /`  \   |~~
#  ~/  o  o \~~~~.----. ~~
#   | =(X)= |~  / (O (O) \
#    ~~~~~~~  ~| =(Y_)=-  |
#   ~~~~    ~~~|   U      |~~

# Project:   https://github.com/containers/podman
# Website:   https://podman.io
# Desktop:   https://podman-desktop.io
# Documents: https://docs.podman.io
# YouTube:   https://youtube.com/@Podman
# X/Twitter: @Podman_io
# Mastodon:  @Podman_io@fosstodon.org

# $ podman system prune --all --force
# Deleted Images
# 5dd467fce50b56951185da365b5feee75409968cbab5767b9b59e325fb2ecbc0
# Total reclaimed space: 787kB
```


&nbsp;

`-`

[![Monster](img/monster.webp "Boo!")](../README.md)
