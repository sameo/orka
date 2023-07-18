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

- **CNI plugins** (us)
- **DNS server/forwarder** (us)
- etcd (not us)
- scheduler (not us)
- apiserver (not us)
- controller (not us)

## API - Component interface

3 services to define. (see above :arrow_up:)

> ⚠️ Working on it...
