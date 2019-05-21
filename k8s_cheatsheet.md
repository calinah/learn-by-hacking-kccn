This guide has been created to help engineers debug applications that are deployed into Kubernetes and not behaving correctly.

## Pod & Container Introspection

| Command                                                      | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `kubectl get pods`                                           | lists the current pods in the current namespace              |
| `kubectl get pods -w`                                        | watches pods continuously                                    |
| `kubectl describe pod <name>`                                | describe pod <name>                                          |
| `kubectl get rc`                                             | list the replication controllers                             |
| `kubectl get services` or `kubectl get svc`                  | list the services in the current namespace                   |
| `kubectl describe service <name>` or `kubectl describe svc <name>` | describe service <name>                                      |
| `kubectl delete pod <name> `                                 | delete pod <name>                                            |
| `kubectl get pods -o wide –w`                                | watch pods continuously and show  <br />info such as IP addresses & nodes provisioned on |

## Cluster Introspection

| Command                        | Description                                                  |
| :----------------------------- | :----------------------------------------------------------- |
| `kubectl version`              | get version info                                             |
| `kubectl cluster-info`         | get cluster info                                             |
| `kubectl config view`          | get cluster config                                           |
| `kubectl describe node <name>` | output info about a node                                     |
| `kubectl get nodes –w`         | watch nodes continuously                                     |
| `kubectl get nodes -o wide`    | gives a detailed view of nodes -  including internal & external IP address |

## Debugging

| Command                                                      | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `kubectl exec -ti <pod> <command>  [-c <container>]`         | execute command on pod , optionally on a<br />given container |
| `klog <pod> [-c <container>]` or<br />`kubectl logs -f <pod> [-c <container>`] | get logs of a given pod or optionally container              |
|                                                              |                                                              |
|                                                              |                                                              |

## Networking

| Command                                                      | Description                               |
| ------------------------------------------------------------ | ----------------------------------------- |
| `kubectl exec -ti <pod_name> -- /bin/sh -c  "curl -v <br /> telnet://<service_name>:<service-port>"` | testing TCP connectivity between services |

## Other resources

<https://www.mankier.com/1/kubectl-auth-can-i> - check whether an action is allowed in your Kubernetes cluster

Use `amicontained` to find out what container runtime you're using as well as what capabilities the your container has.

```
# Export the sha256sum for verification.
$ export AMICONTAINED_SHA256="4e32545f68f25bcbcd4cce82743e916a054e1686df44fab68420fc9f94f80b21"

# Download and check the sha256sum.
$ curl -fSL "https://github.com/genuinetools/amicontained/releases/download/v0.4.7/amicontained-linux-amd64" -o "/usr/local/bin/amicontained" \
	&& echo "${AMICONTAINED_SHA256}  /usr/local/bin/amicontained" | sha256sum -c - \
	&& chmod a+x "/usr/local/bin/amicontained"

$ echo "amicontained installed!"

# Run it!
$ amicontained -h
```

Add these functions to your environment so that you can scan for open ports

``` sudo apt-get update
sudo apt-get install nmap
nmap-kube () 
{ 
nmap --open -T4 -A -v -Pn -p 443,2379,4194,6782-6784,6443,8443,8080,9099,10250,10255,10256 "${@}"
}
nmap-kube-discover () {
local LOCAL_RANGE=$(ip a | awk '/eth0$/{print $2}' | sed 's,[0-9][0-9]*/.*,*,');                                                                  
local SERVER_RANGES=" ";
SERVER_RANGES+="10.0.0.1 ";
SERVER_RANGES+="10.0.1.* ";
SERVER_RANGES+="10.*.0-1.* ";
nmap-kube ${SERVER_RANGES} "${LOCAL_RANGE}"
}
nmap-kube-discover
```

Useful commands for finding open ports:

```
command nmap -Pn -T4 -F --open kube-andy-1
# scanning every port, more is open
command nmap -Pn -T4 --open kube-andy-1 -p 0-65535
command nmap -Pn -T4 --open kube-andy-1 -p 30081
```

