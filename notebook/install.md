# EXPERIMENT PODMAN

## REFERENCES

https://podman.io/docs/installation  
https://github.com/containers/PodmanHello
https://docs.podman.io/en/latest/markdown/podman-network.1.html
https://docs.podman.io/en/latest/markdown/podman-inspect.1.html
https://simpledns.plus/private-ipv6

https://www.youtube.com/watch?v=WnGBO7Jhya4&list=PLn6POgpklwWo_IZ1s2v1Ijf-SnPQY8J57&index=2

## INSTALL - PODMAN - DEBIAN 12

[![Podman](img/podman.webp "Podman")](https://podman.io/)4
[![Debian](img/debian.webp "Debian")](https://debian.org)12

```bash
$ apt-cache policy podman
podman:
  Installed: (none)
  Candidate: 4.3.1+ds1-8+deb12u1+b1
  Version table:
     4.3.1+ds1-8+deb12u1+b1 500
        500 http://deb.debian.org/debian bookworm/main amd64 Packages

$ apt depends podman
podman
  Depends: libc6 (≥ 2.34)
  Depends: libdevmapper1.02.1 (≥ 2:1.02.97)
  Depends: libgpgme11 (≥ 1.4.1)
  Depends: libseccomp2 (≥ 2.5.0)
  Depends: libsubid4 (≥ 1:4.11.1)
  Depends: conmon (≥ 2.0.18~)
  Depends: golang-github-containers-common
 |Depends: crun
  Depends: runc (≥ 1.0.0~rc92~)
  Breaks: buildah (≪ 1.10.1-6)
  Breaks: fuse-overlayfs (≪ 0.7.1)
  Breaks: slirp4netns (≪ 0.4.1)
  Recommends: buildah (≥ 1.28)
  Recommends: dbus-user-session
  Recommends: fuse-overlayfs (≥ 1.0.0~)
  Recommends: slirp4netns (≥ 0.4.1~)
 |Recommends: catatonit
 |Recommends: tini
  Recommends: dumb-init
  Recommends: uidmap
  Suggests: containers-storage
  Suggests: docker-compose
  Suggests: iptables

$ sudo apt install --no-install-recommends --quiet=2 --yes podman
The following additional packages will be installed:
  conmon containernetworking-plugins crun golang-github-containers-common golang-github-containers-image libgpgme11
  libsubid4 libyajl2
Suggested packages:
  containers-storage docker-compose
Recommended packages:
  netavark buildah fuse-overlayfs slirp4netns catatonit | tini | dumb-init uidmap
The following NEW packages will be installed:
  conmon containernetworking-plugins crun golang-github-containers-common golang-github-containers-image libgpgme11
  libsubid4 libyajl2 podman
0 upgraded, 9 newly installed, 0 to remove and 0 not upgraded.
|...|

$ sudo apt install --quiet=2 --yes uidmap
The following NEW packages will be installed:
  uidmap
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
|...|

$ sudo apt install --quiet=2 --yes slirp4netns
The following additional packages will be installed:
  libslirp0
The following NEW packages will be installed:
  libslirp0 slirp4netns
0 upgraded, 2 newly installed, 0 to remove and 0 not upgraded.
|...|

$ whereis podman
podman: /usr/bin/podman /usr/lib/podman /usr/share/man/man1/podman.1.gz
```

```bash
$ podman --version
podman version 4.3.1

$ whereis podman
podman: /usr/bin/podman /usr/lib/podman /usr/share/man/man1/podman.1.gz

$ podman info
host:
  arch: amd64
  buildahVersion: 1.28.2
  cgroupControllers:
  - cpu
  - memory
  - pids
  cgroupManager: systemd
  cgroupVersion: v2
|...|
  networkBackend: cni
  ociRuntime:
    name: crun
    package: crun_1.8.1-1+deb12u1_amd64
    path: /usr/bin/crun
    version: |-
      crun version 1.8.1
      commit: f8a096be060b22ccd3d5f3ebe44108517fbf6c30
      rundir: /run/user/1000/crun
      spec: 1.0.0
      +SYSTEMD +SELINUX +APPARMOR +CAP +SECCOMP +EBPF +YAJL
  os: linux
  remoteSocket:
    path: /run/user/1000/podman/podman.sock
|...|
  slirp4netns:
    executable: /usr/bin/slirp4netns
    package: slirp4netns_1.2.0-1_amd64
    version: |-
      slirp4netns version 1.2.0
      commit: 656041d45cfca7a4176f6b7eed9e4fe6c11e8383
      libslirp: 4.7.0
      SLIRP_CONFIG_VERSION_MAX: 4
      libseccomp: 2.5.4
|...|
store:
  configFile: /home/user/.config/containers/storage.conf
  containerStore:
    number: 0
    paused: 0
    running: 0
    stopped: 0
  graphDriverName: vfs
  graphOptions: {}
  graphRoot: /home/user/.local/share/containers/storage
  graphRootAllocated: 13775888384
  graphRootUsed: 6031396864
  graphStatus: {}
  imageCopyTmpDir: /var/tmp
  imageStore:
    number: 0
  runRoot: /run/user/1000/containers
  volumePath: /home/user/.local/share/containers/storage/volumes
version:
  APIVersion: 4.3.1
  Built: 0
  BuiltTime: Thu Jan  1 01:00:00 1970
  GitCommit: ""
  GoVersion: go1.19.8
  Os: linux
  OsArch: linux/amd64
  Version: 4.3.1

$ podman info --format='{{ .Host.Arch }}'
amd64

$ podman info --format='{{ .Version.Version }}'
4.3.1
```

## INSTALL - PODMAN - DEBIAN 13

[![Podman](img/podman.webp "Podman")](https://podman.io/)5
[![Debian](img/debian.webp "Debian")](https://debian.org)13

```bash
$ apt policy podman
podman:
  Installed: (none)
  Candidate: 5.4.2+ds1-2+b1
  Version table:
     5.4.2+ds1-2+b1 500
        500 http://deb.debian.org/debian trixie/main amd64 Packages

$ apt depends podman
podman
  Depends: conmon
 |Depends: crun
  Depends: runc
    containerd.io
  Depends: golang-github-containers-common
  Depends: netavark
  Depends: init-system-helpers (≥ 1.52)
  Depends: libc6 (≥ 2.38)
  Depends: libgpgme11t64 (≥ 1.23.2)
  Depends: libseccomp2 (≥ 2.5.0)
  Depends: libsqlite3-0 (≥ 3.36.0)
  Depends: libsubid5 (≥ 1:4.16.0)
  Recommends: buildah (≥ 1.31)
  Recommends: ca-certificates
 |Recommends: catatonit
 |Recommends: tini
  Recommends: dumb-init
  Recommends: containers-storage
  Recommends: criu
  Recommends: dbus-user-session
  Recommends: libcriu2
  Recommends: passt
  Recommends: slirp4netns
  Recommends: uidmap
  Suggests: containernetworking-plugins
  Suggests: docker-compose
  Suggests: iptables

$ sudo apt install --no-install-recommends --quiet=2 --yes podman
Installing:
  podman

Installing dependencies:
  conmon                       golang-github-containers-common  libgpgme11t64  netavark
  containernetworking-plugins  golang-github-containers-image   libsubid5

Suggested packages:
  docker-compose

Recommended packages:
  gpg-wks-client  buildah    | tini       containers-storage  libcriu2  slirp4netns
  aardvark-dns    catatonit  | dumb-init  criu                passt     uidmap

Summary:
  Upgrading: 0, Installing: 8, Removing: 0, Not Upgrading: 0
  Download size: 38.5 MB
  Space needed: 163 MB / 7,827 MB available
|...|

$ sudo apt install --quiet=2 --yes uidmap
Installing:
  uidmap

Summary:
  Upgrading: 0, Installing: 1, Removing: 0, Not Upgrading: 0
  Download size: 194 kB
  Space needed: 328 kB / 7,663 MB available
|...|

$ sudo apt install --quiet=2 --yes passt
Installing:
  passt

Summary:
  Upgrading: 0, Installing: 1, Removing: 0, Not Upgrading: 0
  Download size: 233 kB
  Space needed: 667 kB / 7,662 MB available
|...|

$ whereis podman
podman: /usr/bin/podman /usr/lib/podman /usr/libexec/podman /usr/share/man/man1/podman.1.gz
```

```bash
$ podman --version
podman version 5.4.2

$ whereis podman
podman: /usr/bin/podman /usr/lib/podman /usr/libexec/podman /usr/share/man/man1/podman.1.gz

$ podman info
host:
  arch: amd64
  buildahVersion: 1.39.3
  cgroupControllers:
  - cpu
  - memory
  - pids
  cgroupManager: systemd
  cgroupVersion: v2
|...|
  networkBackend: netavark
  networkBackendInfo:
    backend: netavark
    dns:
      package: Unknown
    package: netavark_1.14.0-2_amd64
    path: /usr/lib/podman/netavark
    version: netavark 1.14.0
  ociRuntime:
    name: runc
    package: containerd.io_1.7.27-1_amd64
    path: /usr/bin/runc
    version: |-
      runc version 1.2.5
      commit: v1.2.5-0-g59923ef
      spec: 1.2.0
      go: go1.23.7
      libseccomp: 2.6.0
  os: linux
  pasta:
    executable: /usr/bin/pasta
    package: passt_0.0~git20250503.587980c-2_amd64
    version: ""
  remoteSocket:
    path: /run/user/1000/podman/podman.sock
  rootlessNetworkCmd: pasta
|...|
store:
  configFile: /home/user/.config/containers/storage.conf
  containerStore:
    number: 0
    paused: 0
    running: 0
    stopped: 0
  graphDriverName: overlay
  graphOptions: {}
  graphRoot: /home/user/.local/share/containers/storage
  graphRootAllocated: 13775888384
  graphRootUsed: 5391732736
  graphStatus:
    Backing Filesystem: extfs
    Native Overlay Diff: "true"
    Supports d_type: "true"
    Supports shifting: "false"
    Supports volatile: "true"
    Using metacopy: "false"
  imageCopyTmpDir: /var/tmp
  imageStore:
    number: 0
  runRoot: /run/user/1000/containers
  transientStore: false
  volumePath: /home/user/.local/share/containers/storage/volumes
version:
  APIVersion: 5.4.2
  BuildOrigin: Debian
  Built: 1753478586
  BuiltTime: Fri Jul 25 21:23:06 2025
  GitCommit: ""
  GoVersion: go1.24.4
  Os: linux
  OsArch: linux/amd64
  Version: 5.4.2

$ podman info --format='{{ .Host.Arch }}'
amd64

$ podman info --format='{{ .Version.Version }}'
5.4.2
```

## INSTALL - PODMAN

[![Podman](img/podman.webp "Podman")](https://podman.io/)4+5
[![Debian](img/debian.webp "Debian")](https://debian.org)12+13

```bash
$ systemctl --user enable --now podman.socket
Created symlink '/home/user/.config/systemd/user/sockets.target.wants/podman.socket' →
'/usr/lib/systemd/user/podman.socket'.

$ systemctl --user is-active podman.socket
active

$ systemctl --user enable --now podman-restart.service
Created symlink '/home/user/.config/systemd/user/default.target.wants/podman-restart.service' →
'/usr/lib/systemd/user/podman-restart.service'.

$ systemctl --user is-active podman-restart.service
active

$ sudo loginctl enable-linger user
```

```bash
$ podman container run --quiet --rm quay.io/podman/hello
!... Hello Podman World ...!

         .--❞--.
       ⧸ -     - ⧹
      ⧸ (O)   (O) ⧹
   ~~~| -=(,Y,)=- |
    .---. ⧸‛  ⧹   |~~
 ~⧸  o  o ⧹~~~~.----. ~~
  | =(X)= |~  ⧸ (O (O) ⧹
   ~~~~~~~  ~| =(Y_)=-  |
  ~~~~    ~~~|   U      |~~

Project:   https://github.com/containers/podman
Website:   https://podman.io
Desktop:   https://podman-desktop.io
Documents: https://docs.podman.io
YouTube:   https://youtube.com/@Podman
X/Twitter: @Podman_io
Mastodon:  @Podman_io@fosstodon.org

$ podman system prune --all --force
Deleted Images
5dd467fce50b56951185da365b5feee75409968cbab5767b9b59e325fb2ecbc0
Total reclaimed space: 787kB
```

&nbsp;

`-`

[![Monster](img/monster.webp "Boo!")](../README.md)
