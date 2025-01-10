# Build a Kubernetes cluster using k3s via Ansible

Author: <https://github.com/itwars>

## K3s Ansible Playbook

Build a Kubernetes cluster using Ansible with k3s. The goal is easily install a Kubernetes cluster on machines running:

- [X] Debian
- [X] Ubuntu
- [X] CentOS

on processor architecture:

- [X] x64
- [X] arm64
- [X] armhf

## System requirements

Deployment environment must have Ansible 2.4.0+
Master and nodes must have passwordless SSH access

## Usage

First create a new directory based on the `sample` directory within the `inventory` directory:

```bash
cp -R inventory/sample inventory/my-cluster
```

Second, edit `inventory/my-cluster/hosts.ini` to match the system information gathered above. For example:

```bash
[master]
192.16.35.12

[node]
192.16.35.[10:11]

[k3s_cluster:children]
master
node
```

If needed, you can also edit `inventory/my-cluster/group_vars/all.yml` to match your environment.

Start provisioning of the cluster using the following command:

```bash
ansible-playbook site.yml -i inventory/my-cluster/hosts.ini
```

## Kubeconfig

To get access to your **Kubernetes** cluster just

```bash
scp debian@master_ip:~/.kube/config ~/.kube/config
```

## /etc/hosts file
On Ubuntu 24 the `/etc/hosts` file will be refreshed from a template file `/etc/cloud/templates/hosts.debian.tmpl`, therefore, it is important to add your hosts entries in the template file and run the following command:

```bash
$ ansible pi -m copy -b -a "src=/etc/cloud/templates/hosts.debian.tmpl dest=/etc/cloud/templates/hosts.debian.tmpl mode=0644"
```

## The kube config file
We copy the `~/.kube/config` file from the master node to all our nodes:

```bash
$ ansible pi -m copy -a "src=/home/gdha/.kube/config dest=/home/gdha/.kube/config mode=0600"
```
