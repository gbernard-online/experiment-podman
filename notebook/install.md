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

$ sudo apt-get --quiet=2 --yes install podman
...
Created symlink /etc/systemd/system/default.target.wants/podman-auto-update.service → /lib/systemd/system/podman-auto-update.service.
Created symlink /etc/systemd/system/timers.target.wants/podman-auto-update.timer → /lib/systemd/system/podman-auto-update.timer.
Created symlink /etc/systemd/system/default.target.wants/podman-restart.service → /lib/systemd/system/podman-restart.service.
Created symlink /etc/systemd/system/default.target.wants/podman.service → /lib/systemd/system/podman.service.
Created symlink /etc/systemd/system/sockets.target.wants/podman.socket → /lib/systemd/system/podman.socket.
...
update-initramfs: Generating /boot/initrd.img-6.1.0-38-amd64

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
