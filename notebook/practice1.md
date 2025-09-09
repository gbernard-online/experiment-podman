# EXPERIMENT PODMAN

## REFERENCES

https://docs.podman.io/en/latest/markdown/podman-run.1.html  
https://docs.podman.io/en/latest/markdown/podman-ps.1.html  
https://docs.podman.io/en/latest/markdown/podman-exec.1.html  
https://docs.podman.io/en/latest/markdown/podman-inspect.1.html  
https://docs.podman.io/en/latest/markdown/podman-kill.1.html  
https://docs.podman.io/en/latest/markdown/podman-system.1.html

https://www.youtube.com/watch?v=WnGBO7Jhya4&list=PLn6POgpklwWo_IZ1s2v1Ijf-SnPQY8J57&index=2

## PRACTICE #1 - PODMAN 5 - DEBIAN 13

[![Podman](img/podman.webp "Podman")](https://podman.io)5
[![Debian](img/debian.webp "Debian")](https://debian.org)13

```bash
$ podman run --detach --name=nginx --publish=80:80 --quiet --rm nginx:alpine
Error: pasta failed with exit code 1:
Failed to bind port 80 (Permission denied) for option '-t 80-80:80-80'

$ podman run --detach --name=nginx --publish=8080:80 --rm nginx:alpine
e68003a2747bf6191382fc89504400c510a43b70eccf069ab6384d7ad7e0bba6

$ podman ps --all --quiet
e68003a2747b

$ podman ps --all --format='{{ .ID }} {{ .Ports }}'
e68003a2747b 0.0.0.0:8080->80/tcp

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

$ podman exec nginx netstat -l -n -t
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN
tcp        0      0 :::80                   :::*                    LISTEN

$ podman exec nginx curl --connect-timeout 5 --fail --ipv4 --show-error --silent localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
|...|

$ podman exec nginx curl --connect-timeout 5 --fail --ipv6 --show-error --silent localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
|...|
```

```bash
$ podman inspect nginx --format='{{ .HostConfig.NetworkMode }}'
pasta

$ podman exec nginx ls /sys/class/net
enp0s3
lo

$ podman exec nginx ip address show dev enp0s3 scope global
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 65520 qdisc fq_codel state UNKNOWN qlen 1000
    link/ether 16:84:f1:a5:0c:6b brd ff:ff:ff:ff:ff:ff
    inet 172.16.0.227/24 brd 172.16.0.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fd05:4e60:543f:dcc1:7c4f:15da:c032:ada/64 scope global noprefixroute flags 102
       valid_lft forever preferred_lft forever
    inet6 fd05:4e60:543f:dcc1:5054:ff:fe00:e3/64 scope global flags 102
       valid_lft forever preferred_lft forever

$ ls /sys/class/net
docker0  enp0s3  lo

$ ip address show dev enp0s3 scope global
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:00:00:e3 brd ff:ff:ff:ff:ff:ff
    altname enx5254000000e3
    inet 172.16.0.227/24 brd 172.16.0.255 scope global dynamic noprefixroute enp0s3
       valid_lft 3409sec preferred_lft 2844sec
    inet6 fd05:4e60:543f:dcc1:5054:ff:fe00:e3/64 scope global dynamic mngtmpaddr proto kernel_ra
       valid_lft forever preferred_lft forever
    inet6 fd05:4e60:543f:dcc1:7c4f:15da:c032:ada/64 scope global mngtmpaddr noprefixroute
       valid_lft forever preferred_lft forever

$ podman inspect nginx --format='{{ .NetworkSettings.IPAddress }}'

$ podman inspect nginx --format='{{ .NetworkSettings.GlobalIPv6Address }}'

$ pidof pasta
2072

$ cat /proc/2072/cmdline | strings -1
/usr/bin/pasta
--config-net
-t
8080-8080:80-80
--dns-forward
169.254.1.1
-u
none
-T
none
-U
none
--no-map-gw
--quiet
--netns
/run/user/1000/netns/netns-f275b19a-9a83-6e8e-7fcd-ea94be23984d
--map-guest-addr
169.254.1.2
```

```bash
$ podman kill nginx
nginx

$ podman ps --all --quiet

$ podman system df
TYPE           TOTAL       ACTIVE      SIZE        RECLAIMABLE
Images         1           0           8.133MB     8.133MB (100%)
Containers     0           0           0B          0B (0%)
Local Volumes  0           0           0B          0B (0%)

$ ls --width=1 .local/share/containers/storage/overlay-images
4a86014ec6994761b7f3118cf47e4b4fd6bac15fc6fa262c4f356386bbc0e9d9
images.json
images.lock

$ podman system prune --all --force
Deleted Images
4a86014ec6994761b7f3118cf47e4b4fd6bac15fc6fa262c4f356386bbc0e9d9
Total reclaimed space: 53.95MB

$ ls --width=1 .local/share/containers/storage/overlay-images
images.json
images.lock
```

&nbsp;

`-`

[![Monster](https://avatars.githubusercontent.com/u/47848582?s=96&v=4 "Boo!")](../README.md)
