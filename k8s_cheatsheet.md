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
|                                                              |                                           |
|                                                              |                                           |
|                                                              |                                           |

## Other resources

<https://www.mankier.com/1/kubectl-auth-can-i> - check whether an action is allowed in your Kubernetes cluster