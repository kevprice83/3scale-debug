# 3scale debug container

This container contains a few debugging tools that may be needed when debugging 3scale issues. Based on alpine.

- Build: `build-base git go bash bash-completion vim jq`
- Databases: `mysql-client postgresql-client redis`
- Network: `bind-tools iputils tcpdump curl nmap tcpflow iftop net-tools mtr netcat-openbsd bridge-utils iperf ngrep`
- Certificates: `ca-certificates openssl`
- Processes/IO: `htop atop strace iotop sysstat ltrace ncdu logrotate hdparm pciutils psmisc tree pv`

## Attach to an existing docker container

```
docker run --rm -ti --net container:<container-id> quay.io/3scale/3scaledebug:latest
```

## Openshift (via oc run) **Recommended** as it doesn't involve changing deployments

```
oc run 3scale-debug -i --tty --rm --image=quay.io/3scale/3scaledebug:latest
```

## Sidecar

### Patch Add

```
oc patch dc/<deployment config name> --type='json' -p='[{"op": "add", "path": "/spec/template/spec/containers/0", "value": {"command": ["/bin/sleep","infinity"],"image": "quay.io/3scale/3scaledebug:latest","name": "3scaledebug"} }]'
```

### Patch Remove

```
oc patch dc/<deployment config name> --type json -p '[{ "op": "remove", "path": "/spec/template/spec/containers/0" }]'
```
