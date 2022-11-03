
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
- ### etcd > 
  is a key-value store. stores master and node information. Also used as k8s backend store for all cluster data.
  
- ### kube-scheduler > 
  It is responsible for distributing containers across across multiple worker nodes. It finds unassigned pod and assign node to it. 
  
- ### kube-apiserver > 
  It exposes k8s APIs. everyone, clitool kubectl, Users and masters components like scheduler, controller manager, etcd, worker node components like 'kubelet', everyone talk with API server. 
  
- ### kube controller manager >
  Basically a watcher for resiliency. If any container, node or endpoint goes down then it brings up new  container, node or endpoint . 
    - Node controller >
    - Replication Controller >
    - Endpoint Controller >
    - Service Account & Token Controller > 

- ### Cloud controller manager >

## Conponents in Worker node
- Contrainer runtime ( such as Docker )
- kubelet > 
- kube-proxy > 
