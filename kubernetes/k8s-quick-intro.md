
## What is Kubernetes?
Kubernetes is a portable, extensible, opensource platform for managing containerized workloads

## Features:
- Service discovery and load balancing
- Storage orchestration
- Automated rollouts and rollback
- Automatic bin packing
- Self-healing
- Secret and configuration management

## High level K8s architecture
- A k8s custer has a Master and one or more Worker Nodes
- Every nodes have container runtime installed. (container runtime such as docker or some other CR)

## Conponents in Master node
- Contrainer runtime ( such as Docker )
- etcd > is a key-value store. stores master and node information
- kube-scheduler > 
- kube-apiserver >
- kube controller manager >
- Cloud controller manager >

## Conponents in Worker node
- Contrainer runtime ( such as Docker )
- kubelet > 
- kube-proxy > 
