# EXPERIMENT PODMAN

## REFERENCES

https://podman.io/docs/installation  
https://github.com/containers/PodmanHello  
https://docs.podman.io/en/latest/markdown/podman-network.1.html  
https://docs.podman.io/en/latest/markdown/podman-inspect.1.html  
https://simpledns.plus/private-ipv6

https://www.youtube.com/watch?v=WnGBO7Jhya4&list=PLn6POgpklwWo_IZ1s2v1Ijf-SnPQY8J57&index=2


## INSTALL - PODMAN 5 - DEBIAN 13

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

```bash
$ podman run --quiet --rm quay.io/podman/hello
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

$ sudo loginctl show-user user | fgrep Linger
Linger=yes
```

```bash
$ mkdir --verbose $HOME/.config/containers
mkdir: created directory '/home/user/.config/containers'

$ cat >$HOME/.config/containers/registries.conf <<EOF
unqualified-search-registries = ["docker.io"]
EOF
```

```bash
$ podman network ls
NETWORK ID    NAME        DRIVER
2f259bab93aa  podman      bridge

$ podman inspect podman | jq --sort-keys '.[0] | del(.created) | del(.containers)'
{
  "dns_enabled": false,
  "driver": "bridge",
  "id": "2f259bab93aaaaa2542ba43ef33eb990d0999ee1b9924b557b7be53c0b7a1bb9",
  "internal": false,
  "ipam_options": {
    "driver": "host-local"
  },
  "ipv6_enabled": false,
  "name": "podman",
  "network_interface": "podman0",
  "subnets": [
    {
      "gateway": "10.88.0.1",
      "subnet": "10.88.0.0/16"
    }
  ]
}

$ podman inspect podman |
jq --sort-keys '.[0] | del(.created) | del(.containers) | .ipv6_enabled=true' |
jq '.subnets[0]={"gateway":"172.18.0.1","subnet":"172.18.0.0/24"}' |
jq '.subnets[1]={"gateway":"fd5e:822b:4924:c112::1","subnet":"fd5e:822b:4924:c112::/64"}' |
sponge .local/share/containers/storage/networks/podman.json

$ podman inspect podman | jq --sort-keys '.[0] | del(.created) | del(.containers)'
{
  "dns_enabled": false,
  "driver": "bridge",
  "id": "2f259bab93aaaaa2542ba43ef33eb990d0999ee1b9924b557b7be53c0b7a1bb9",
  "internal": false,
  "ipam_options": {
    "driver": "host-local"
  },
  "ipv6_enabled": true,
  "name": "podman",
  "network_interface": "podman0",
  "subnets": [
    {
      "gateway": "172.18.0.1",
      "subnet": "172.18.0.0/24"
    },
    {
      "gateway": "fd5e:822b:4924:c112::1",
      "subnet": "fd5e:822b:4924:c112::/64"
    }
  ]
}
```

&nbsp;

`-`

[![Monster](https://avatars.githubusercontent.com/u/47848582?s=96&v=4 "Boo!")](../README.md)
