# Automated build of HA k3s Cluster with `kube-vip` and MetalLB

![Fully Automated K3S etcd High Availability Install](https://img.youtube.com/vi/CbkEWcUZ7zM/0.jpg)

This playbook will build an HA Kubernetes cluster with `k3s`, `kube-vip` and MetalLB via `ansible`.

This is based on the work from [this fork](https://github.com/212850a/k3s-ansible) which is based on the work from [k3s-io/k3s-ansible](https://github.com/k3s-io/k3s-ansible). It uses [kube-vip](https://kube-vip.chipzoller.dev/) to create a load balancer for control plane, and [metal-lb](https://metallb.universe.tf/installation/) for its service `LoadBalancer`.

I have simple just rewrote the git as a role instead of a whole collection and renewed the code for Ansible 2.11 and later.

If you want more context on how this works, see:

📄 [Documentation](https://docs.technotim.live/posts/k3s-etcd-ansible/) (including example commands)

## 📖 k3s Ansible Playbook

Build a Kubernetes cluster using Ansible with k3s. The goal is easily install a HA Kubernetes cluster on machines running:

- [X] Debian
- [X] Ubuntu
- [X] CentOS

on processor architecture:

- [X] x64

## ✅ System requirements

* Deployment environment must have Ansible 2.11.0+
* `server` and `agent` nodes should have passwordless SSH access, if not you can supply arguments to provide credentials `-ask-pass --ask-become-pass` to ach command.
* Please see examples/example inventory/site/group_vars for example of needed group vars for this role.

## 🚀 Getting Started

### Preparation

Change the inventory.ini files to the hosts <br>
Example:

```ini
[master]
192.168.30.38
192.168.30.39
192.168.30.40

[node]
192.168.30.41
192.168.30.42

[k3s_cluster:children]
master
node
```

If multiple hosts are in the master group, the playbook will automatically set up k3s in [HA mode with etcd](https://rancher.com/docs/k3s/latest/en/installation/ha-embedded/).

This requires at least k3s version `1.19.1` however the version is configurable by using the `k3s_version` variable.

If needed, you can also edit `inventory/my-cluster/group_vars/all.yml` to match your environment.

### Create Cluster
Start provisioning of the cluster using the following command:

```
ansible-playbook site.yml --ask-become-pass -i inventory/my-cluster/hosts.ini
```

After deployment control plane will be accessible via virtual ip-address which is defined in inventory/group_vars/all.yml as `apiserver_endpoint`

### Remove k3s cluster

```
ansible-playbook reset.yml --ask-become-pass -i inventory/my-cluster/hosts.ini
```

## ⚙️ Kube Config

To copy your `kube config` locally so that you can access your **Kubernetes** cluster run:

```
scp debian@master_ip:~/.kube/config ~/.kube/config
```



Thanks to these repos for code and ideas:

* [k3s-io/k3s-ansible](https://github.com/k3s-io/k3s-ansible)
* [geerlingguy/turing-pi-cluster](https://github.com/geerlingguy/turing-pi-cluster)
* [212850a/k3s-ansible](https://github.com/212850a/k3s-ansible) 