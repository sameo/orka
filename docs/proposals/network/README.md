# Software Defined Network

## Analysis

### Examples

- Canal
- Calico
- Flannel
- Weave
- Cilium

![benchmark-cni](https://miro.medium.com/v2/resize:fit:1400/1*zDiFQZVXuL923x0LbmXMaA.png)

### Sources

- [CNI](https://github.com/containernetworking/cni) also makes a good framework for creating a new container networking project from scratch
- [Networks - treeptik k8s](https://treeptik.gitbook.io/k8s/fundamentals/)network
- [Explain NS linux](https://www.youtube.com/watch?v=j_UUnlVC2Ss)
- [Spec - CNI](https://github.com/containernetworking/cni/blob/main/SPEC.md)

<br>

## Network domain

### Definition

The goal is to be able to:

- manage networks through the software
- isolate instance/pods with isolated networks
- apply network security policies

Options:

- a library to manage networks directly from the Node Agent
- a deamon running on each worker nodes and another on the master node

### Scope

- Network security policy (access control, filtering, firewall, iptables... ?)  
  "NetworkPolicy describes what network traffic is allowed for a set of Pods"
- Virtual isolated networks by default
- Port mapping / Selectors pods
- DNS routing (e.g. ingress controller)
- DNS resolution (e.g. coreDNS)
- Monitoring network traffic (e.g. metrics-server)
- Load balancing (e.g. klipper, metallb, service)
- Streaming(?)
- kube-proxy

### Components list of a minimal k8s (sample)

- **coreDNS** (us)
- etcd (not us)
- **cni** (us)
- **kube-proxy** (us)
- scheduler (not us)
- apiserver (not us)
- controller (not us)

## API - Component interface

3 services to define. (see above :arrow_up:)

> ⚠️ Working on it...
