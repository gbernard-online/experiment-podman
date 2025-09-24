# EXPERIMENT PODMAN

## REFERENCES

https://docs.podman.io/en/latest/markdown/podman-run.1.html  
https://docs.podman.io/en/latest/markdown/podman-unshare.1.html

## PRACTICE #2 - PODMAN 5 - DEBIAN 13

[![Podman](img/podman.webp "Podman")](https://podman.io)5
[![Debian](img/debian.webp "Debian")](https://debian.org)13

```bash
$ podman run --detach --name=nginx --network=bridge --publish=8080:80 --quiet --rm nginx:alpine
509604f10a24c7ee48dbbfd531ccc9133a1316cb5ab3023001cf9213f9dcabda

$ podman ps --all --format='{{ .ID }} {{ .Ports }}'
509604f10a24 0.0.0.0:8080->80/tcp

$ curl --connect-timeout 5 --fail --ipv4 --show-error --silent localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
|...|

$ curl --connect-timeout 5 --fail --ipv6 --show-error --silent localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
|...|
```

```bash
$ podman inspect nginx --format='{{ .HostConfig.NetworkMode }}'
bridge

$ podman exec nginx ls /sys/class/net
eth0
lo

$ podman exec nginx ip address show dev eth0 scope global
2: eth0@if4: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP qlen 1000
    link/ether f6:7f:79:a2:6d:d6 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.2/24 brd 172.18.0.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fd5e:822b:4924:c112::2/64 scope global 
       valid_lft forever preferred_lft forever

$ podman inspect nginx --format='{{ .NetworkSettings.IPAddress }}'
172.18.0.2

$ podman inspect nginx --format='{{ .NetworkSettings.GlobalIPv6Address }}'
fd5e:822b:4924:c112::2

$ curl --connect-timeout 5 --fail --show-error --silent 172.18.0.2
curl: (28) Failed to connect to 172.18.0.2 port 80 after 5001 ms: Timeout was reached

$ curl --connect-timeout 5 --fail --show-error --silent [fd5e:822b:4924:c112::2]
curl: (28) Failed to connect to fd5e:822b:4924:c112::2 port 80 after 5001 ms: Timeout was reached

$ ip address show scope global
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:00:00:e3 brd ff:ff:ff:ff:ff:ff
    altname enx5254000000e3
    inet 172.16.0.227/24 brd 172.16.0.255 scope global dynamic noprefixroute enp0s3
       valid_lft 2347sec preferred_lft 1780sec
    inet6 fd05:4e60:543f:dcc1:5054:ff:fe00:e3/64 scope global dynamic mngtmpaddr proto kernel_ra 
       valid_lft forever preferred_lft forever
    inet6 fd05:4e60:543f:dcc1:7c4f:15da:c032:ada/64 scope global mngtmpaddr noprefixroute 
       valid_lft forever preferred_lft forever
4: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether de:32:9a:e6:77:b7 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/24 brd 172.17.0.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fd7f:e996:4f39:5d47::1/80 scope global nodad 
       valid_lft forever preferred_lft forever

$ podman unshare --rootless-netns ip address show scope global
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 65520 qdisc fq_codel state UNKNOWN group default qlen 1000
    link/ether 72:d3:f7:26:0c:21 brd ff:ff:ff:ff:ff:ff
    inet 172.16.0.227/24 brd 172.16.0.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fd05:4e60:543f:dcc1:7c4f:15da:c032:ada/64 scope global nodad mngtmpaddr noprefixroute 
       valid_lft forever preferred_lft forever
    inet6 fd05:4e60:543f:dcc1:5054:ff:fe00:e3/64 scope global nodad mngtmpaddr proto kernel_ra 
       valid_lft forever preferred_lft forever
3: podman0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether ae:4b:c6:fd:07:7c brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.1/24 brd 172.18.0.255 scope global podman0
       valid_lft forever preferred_lft forever
    inet6 fd5e:822b:4924:c112::1/64 scope global 
       valid_lft forever preferred_lft forever

$ podman unshare --rootless-netns \
curl --connect-timeout 5 --fail --show-error --silent 172.18.0.2
!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
|...|

$ podman unshare --rootless-netns \
curl --connect-timeout 5 --fail --show-error --silent [fd5e:822b:4924:c112::2]
!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
|...|

$ podman unshare --rootless-netns /sbin/iptables-save

$ podman unshare --rootless-netns /sbin/ip6tables-save

$ podman unshare --rootless-netns /sbin/nft list tables
table inet netavark

$ podman unshare --rootless-netns /sbin/nft list table inet netavark | expand --tabs=2
table inet netavark {
  chain INPUT {
    type filter hook input priority filter; policy accept;
    ip saddr 172.18.0.0/24 meta l4proto { tcp, udp } th dport 53 accept
    ip6 saddr fd5e:822b:4924:c112::/64 meta l4proto { tcp, udp } th dport 53 accept
  }

  chain FORWARD {
    type filter hook forward priority filter; policy accept;
    ct state invalid drop
    jump NETAVARK-ISOLATION-1
    ip daddr 172.18.0.0/24 ct state established,related accept
    ip saddr 172.18.0.0/24 accept
    ip6 daddr fd5e:822b:4924:c112::/64 ct state established,related accept
    ip6 saddr fd5e:822b:4924:c112::/64 accept
  }

  chain POSTROUTING {
    type nat hook postrouting priority srcnat; policy accept;
    meta mark & 0x00002000 == 0x00002000 masquerade
    ip saddr 172.18.0.0/24 jump nv_2f259bab_172_18_0_0_nm24
    ip6 saddr fd5e:822b:4924:c112::/64 jump nv_2f259bab_fd5e-822b-4924-c112--_nm64
  }

  chain PREROUTING {
    type nat hook prerouting priority dstnat; policy accept;
    fib daddr type local jump NETAVARK-HOSTPORT-DNAT
  }

  chain OUTPUT {
    type nat hook output priority dstnat; policy accept;
    fib daddr type local jump NETAVARK-HOSTPORT-DNAT
  }

  chain NETAVARK-HOSTPORT-DNAT {
    tcp dport 8080 jump nv_2f259bab_172_18_0_0_nm24_dnat
    tcp dport 8080 jump nv_2f259bab_fd5e-822b-4924-c112--_nm64_dnat
  }

  chain NETAVARK-HOSTPORT-SETMARK {
    meta mark set meta mark | 0x00002000
  }

  chain NETAVARK-ISOLATION-1 {
  }

  chain NETAVARK-ISOLATION-2 {
  }

  chain NETAVARK-ISOLATION-3 {
    oifname "podman0" drop
    jump NETAVARK-ISOLATION-2
  }

  chain nv_2f259bab_172_18_0_0_nm24 {
    ip daddr 172.18.0.0/24 accept
    ip daddr != 224.0.0.0/4 masquerade
  }

  chain nv_2f259bab_fd5e-822b-4924-c112--_nm64 {
    ip6 daddr fd5e:822b:4924:c112::/64 accept
    ip6 daddr != ::/8 masquerade
  }

  chain nv_2f259bab_172_18_0_0_nm24_dnat {
    ip saddr 172.18.0.0/24 tcp dport 8080 jump NETAVARK-HOSTPORT-SETMARK
    ip saddr 127.0.0.1 tcp dport 8080 jump NETAVARK-HOSTPORT-SETMARK
    tcp dport 8080 dnat ip to 172.18.0.2:80
  }

  chain nv_2f259bab_fd5e-822b-4924-c112--_nm64_dnat {
    ip6 saddr fd5e:822b:4924:c112::/64 tcp dport 8080 jump NETAVARK-HOSTPORT-SETMARK
    tcp dport 8080 dnat ip6 to [fd5e:822b:4924:c112::2]:80
  }
}
```

```bash
$ podman kill nginx
nginx

$ podman ps --all --quiet

$ podman system prune --all --force
Deleted Images
4a86014ec6994761b7f3118cf47e4b4fd6bac15fc6fa262c4f356386bbc0e9d9
Total reclaimed space: 53.89MB
```

&nbsp;

`-`

[![Monster](https://avatars.githubusercontent.com/u/47848582?s=96&v=4 "Boo!")](../README.md)
