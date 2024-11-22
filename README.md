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
| Kubernetes  | 1.31.1  |
| kube-vip  | 0.8.4  |
| Cillium  | 1.16.3  |
| Sandbox Image  | 3.10  |
| Node Exporter  | 1.8.2  |

## Installation steps

### Install prerequisite packages

```console
cd kubernetes_vanilla_ansible
ansible-playbook kubernetes_node_stage_rpi.yaml
```

### Install bootstrap control node

```console
cd kubernetes_vanilla_ansible
ansible-playbook bootstrap_kubernetes.yaml
```

### Install second and third control nodes

### Install worker nodes

### Verify installation

### Install Cillium CNI with Ingress and LB support

### Configure LB pool

### Configure NFS CSI

## Upgrade kubernetes (minor verions)

Get the latest kubernetes client tools

### 
```console
cd kubernetes_vanilla_ansible
ansible-playbook upgrade_kubernetes_tools.yaml
```

### Check for upgrades

```console
cd kubernetes_vanilla_ansible
ansible-playbook upgrade_check_kubernetes.yaml
```
Example output

```console

Executing playbook upgrade_check_kubernetes.yaml

- pi-cluster01 on hosts: pi-cluster01 -
See if there is an upgrade path to the kubernetes cluster...
[WARNING]: Platform linux on host pi-cluster01 is using the discovered Python interpreter at /usr/bin/python3.11, but future installation of another Python interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
  pi-cluster01 done | stdout: [preflight] Running pre-flight checks.
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[upgrade] Running cluster health checks
[upgrade] Fetching available versions to upgrade to
[upgrade/versions] Cluster version: 1.31.0
[upgrade/versions] kubeadm version: v1.31.1
[upgrade/versions] Target version: v1.31.1
[upgrade/versions] Latest version in the v1.31 series: v1.31.1

Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
COMPONENT   NODE      CURRENT   TARGET

Upgrade to the latest version in the v1.31 series:

COMPONENT                 NODE           CURRENT    TARGET
kube-apiserver            pi-cluster01   v1.31.0    v1.31.1
kube-apiserver            pi-cluster02   v1.31.0    v1.31.1
kube-apiserver            pi-cluster03   v1.31.0    v1.31.1
kube-controller-manager   pi-cluster01   v1.31.0    v1.31.1
kube-controller-manager   pi-cluster02   v1.31.0    v1.31.1
kube-controller-manager   pi-cluster03   v1.31.0    v1.31.1
kube-scheduler            pi-cluster01   v1.31.0    v1.31.1
kube-scheduler            pi-cluster02   v1.31.0    v1.31.1
kube-scheduler            pi-cluster03   v1.31.0    v1.31.1
kube-proxy                               1.31.0     v1.31.1
CoreDNS                                  v1.11.3    v1.11.3
etcd                      pi-cluster01   3.5.15-0   3.5.15-0
etcd                      pi-cluster02   3.5.15-0   3.5.15-0
etcd                      pi-cluster03   3.5.15-0   3.5.15-0

You can now apply the upgrade by executing the following command:

        kubeadm upgrade apply v1.31.1

_____________________________________________________________________


The table below shows the current state of component configs as understood by this version of kubeadm.
Configs that have a "yes" mark in the "MANUAL UPGRADE REQUIRED" column require manual config upgrade or
resetting to kubeadm defaults before a successful upgrade can be performed. The version to manually
upgrade to is denoted in the "PREFERRED VERSION" column.

API GROUP                 CURRENT VERSION   PREFERRED VERSION   MANUAL UPGRADE REQUIRED
kubeproxy.config.k8s.io   -                 v1alpha1            no
kubelet.config.k8s.io     v1beta1           v1beta1             no
_____________________________________________________________________ | stderr: W1019 22:12:20.888168   25555 configset.go:78] Warning: No kubeproxy.config.k8s.io/v1alpha1 config is loaded. Continuing without it: configmaps "kube-proxy" not found
W1019 22:12:26.324341   25555 configset.go:78] Warning: No kubeproxy.config.k8s.io/v1alpha1 config is loaded. Continuing without it: configmaps "kube-proxy" not found

- Play recap -
  pi-cluster01               : ok=1    changed=1    unreachable=0    failed=0    rescued=0    ignored=0
```

If the suggestion is to update the cluster then run the following on a control node 

```console
kubeadm upgrade apply v1.31.1
```
If the above command returns successfully then run following on the other control nodes

```console
kubeadm upgrade node
```