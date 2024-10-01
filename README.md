# Kubernetes Vanilla Ansible for debian based OSes 
This project aims to create a standard kubernetes cluster using ansible playbooks

* CNI - Cillium
* CSI - csi-driver-nfs, rook, openebs
* Ingress Controller - Cillium mesh
* Loadbalancer - Cillium 
* Kubernetes Dashboard
* Metric Server - For pod scaling
* Promethus Node Exporter for good node metrics

## Prerequisites

Raspbian / Debian / Ubuntu 
10x raspberry pi 5
NFS server
Local Storage for cluster

## Configure ansible hosts file

Example ansible hostfile with server host names. SSH passwordless login must be configured for each machine.

```yaml
# 10 Node cluster, 3 control plane nodes, 7 worker nodes
# Configure host lists for different roles.
picluster:
  hosts:
    pi-cluster01:
    pi-cluster02:
    pi-cluster03:
    pi-cluster04:
    pi-cluster05:
    pi-cluster06:
    pi-cluster07:
    pi-cluster08:
    pi-cluster09:
    pi-cluster10:
picluster_master_node_boot_strap:
  hosts:
    pi-cluster01:
picluster_master_nodes_without_boot_strap:
  hosts:
    pi-cluster02:
    pi-cluster03:
picluster_master_nodes:
  hosts:
    pi-cluster01:
    pi-cluster02:
    pi-cluster03:
picluster_worker_nodes:
  hosts:
    pi-cluster04:
    pi-cluster05:
    pi-cluster06:
    pi-cluster07:
    pi-cluster08:
    pi-cluster09:
    pi-cluster10:
```

## Component versions table

Table to track different versions of components in the playbooks and update as needed

| Component  | Version |
| ------------- | ------------- |
| Kubernetes  | 1.31  |
| Cillium  | 1.16.2  |
| Sandbox Image  | 3.10  |
| Node Exporter  | 1.8.2  |

## Installation steps

### Install prerequisite packages


