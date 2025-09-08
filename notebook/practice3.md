# EXPERIMENT PODMAN

## REFERENCES

https://docs.podman.io/en/latest/markdown/podman-run.1.html  
https://docs.podman.io/en/latest/markdown/podman-rm.1.html

## PRACTICE #3 - PODMAN 5 - DEBIAN 13

[![Podman](img/podman.webp "Podman")](https://podman.io/)5
[![Debian](img/debian.webp "Debian")](https://debian.org)13

```bash
$ podman run --detach --name=nginx --publish=8080:80 --quiet --restart=always nginx:alpine

$ podman ps --all --format='{{ .ID }} {{ .Ports }}'
2e8bd205e557 0.0.0.0:8080->80/tcp

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

$ podman inspect nginx --format='{{ .HostConfig.RestartPolicy }}'
{always 0}

$ sudo reboot
```

```bash
$ podman ps --all --quiet
d5cf041ff673

$ podman inspect --format='{{ .State.Running }}' nginx
true

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

$ podman kill nginx
nginx

$ podman inspect --format='{{ .State.Running }}' nginx
false

$ curl --connect-timeout 5 --fail --ipv4 --show-error --silent localhost:8080
curl: (7) Failed to connect to localhost port 8080 after 0 ms: Could not connect to server

$ curl --connect-timeout 5 --fail --ipv6 --show-error --silent localhost:8080
curl: (7) Failed to connect to localhost port 8080 after 0 ms: Could not connect to server

$ sudo reboot
````

```bash
$ podman inspect --format='{{ .State.Running }}' nginx
true

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

$ podman kill nginx
nginx

$ podman rm nginx
nginx

$ podman system prune --all --force
Deleted Images
4a86014ec6994761b7f3118cf47e4b4fd6bac15fc6fa262c4f356386bbc0e9d9
Total reclaimed space: 53.95MB
```

&nbsp;

`-`

[![Monster](https://avatars.githubusercontent.com/u/47848582?s=96&v=4 "Boo!")](../README.md)
