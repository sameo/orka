# Software Defined Network

## Analysis

### Examples

- [Kindnet](https://www.tkng.io/cni/kindnet/)

### Sources

- [CNI](https://github.com/containernetworking/cni) also make a good framework for creating a new container networking project from scratch
- [Networks - treeptik k8s](https://treeptik.gitbook.io/k8s/fundamentals/)
- [Explain NS linux](https://www.youtube.com/watch?v=j_UUnlVC2Ss)
- [Spec - CNI](https://www.cni.dev/docs/spec/)
- [TKNG - The Kubernetes Networking Guide](https://www.tkng.io/)

## Network domain

### Glossary

- *runtime* is the program responsible for executing CNI plugins.
- *plugin* is a program that applies a specified network configuration.  
  (it's a binary or an executable)

### Definition

The **orka SDN** implements the [`CNI`](https://www.cni.dev/docs/spec/) specification.

It is a **CNI plugin** and must be called by e.g. the node Agent through the `CNI` specified
command line interface. It configure workloads networking according to a set of `CNI`
compliant configuration files, as described in the following sections.

The goal is to be able to:

- manage networks through the software
- isolate instance/pods with isolated networks
- apply network security policies

The *plugins* must be installed on every node in the cluster.

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

- RBAC, Network Policies
- Monitoring network traffic (e.g. metrics-server)
- Load balancing (e.g. service)
- Load balancer (e.g. klipper, metallb)

## API - Component interface

### CNI

#### Configuration

Configuration files (`.conf`) are located in `/etc/cni/net.d/`.

Example of a configuration file:

```
{
    "cniVersion": "<VERSION>",
    "name": "<CNI_NAME>",
    "type": "<CNI_BINARY_NAME>",
    // other variables to pass to plugin
}
```

Each configuration file references a CNI plugin.

#### CNI plugins

**CNI plugin** are located in `/opt/cni/bin/`.

There are 3 methods available:

- `ADD` create a network interface
- `DELETE` delete a network interface
- `CHECK` check if the configuration is as expected
- `VERSION` (optional for us) the cni version of the command

As specified by the CNI
documentation ([Execution protocol](https://www.cni.dev/docs/spec/#section-2-execution-protocol)):

The *runtime* passes parameters to the *plugin* via:

- environment variables
- configuration: it supplies configuration via stdin (mostly JSON)

The plugin returns a result on stdout on success, or an error on stderr if the operation fails.
Configuration and results are encoded in JSON.

### Simple CNI plugin

> We need to define a first plugin to create network interface inside a Pod
