# Software Defined Network

## Analysis

### Examples

- Flannel: good example, focused on networking
- Calico
- Weave
- Cilium

### Sources

- [CNI](https://github.com/containernetworking/cni) also make a good framework for creating a new container networking project from scratch
- [Networks - treeptik k8s](https://treeptik.gitbook.io/k8s/fundamentals/)
- [Explain NS linux](https://www.youtube.com/watch?v=j_UUnlVC2Ss)
- [Spec - CNI](https://www.cni.dev/docs/spec/)

## Network domain

### Vocabulary

- *runtime* is the program responsible for executing CNI plugins
- *plugin* is a program that applies a specified network configuration

### Definition

The *plugins* (= binaries) needs to be installed on every node in the cluster.

For that, we have to create *plugins* (= binaries) implementing the CNI specifications.

That way, the *runtime* (e.g. the node agent) can use the *plugins* to create network interfaces between pods.

The goal is to be able to:

- manage networks through the software
- isolate instance/pods with isolated networks
- apply network security policies

### Scope

- Pod-to-Pod network packets on the same host
	- Firewall (e.g. iptables)
		- Need to ACCEPT FORWARDING traffic for pod CIDR because packets are dropped by the Linux kernel by default  
		  (Linux treats network packets in non-default network namespaces as external packets by default)
	- Allow outgoing internet
- Cross-node pod-to-pod network packets
- Port mapping / Selectors pods
- DNS routing (e.g. ingress controller)
- DNS resolution (e.g. coreDNS) -> pod-to-pod and pod-to-service
	- "<NAME_SVC>.<NAMESPACE>.svc.cluster.local"
	- "<POD_IP_WITH_DASH_INSTEAD_DOT>.<NAMESPACE>.pod.cluster.local"

### Out of scope

| Not for now                                      | Not for us |
|--------------------------------------------------|------------|
| RBAC, Network Policies                           | kube proxy |
| Monitoring network traffic (e.g. metrics-server) |            |
| Load balancing (e.g. service)                    |            | 
| Load balancer (e.g. klipper, metallb)            |            |

### Components list of a minimal k8s (sample)

- **CNI plugins** (us)
- **DNS server/forwarder** (us)
- etcd (not us)
- scheduler (not us)
- apiserver (not us)
- controller (not us)

## API - Component interface

3 services to define. (see above :arrow_up:)

> ⚠️ Working on it...
