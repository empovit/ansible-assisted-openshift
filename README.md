# Ansible Collection for Assisted Installation of OpenShift Clusters

This is a collection of roles that can be useful when installing an OpenShift cluster with the [assisted installer](https://cloud.redhat.com/blog/openshift-assisted-installer-is-now-generally-available).

It mostly builds on, and complements the [karmab.aicli](https://github.com/karmab/ansible-aicli-modules) modules.

## Requirements

- [aicli](https://github.com/karmab/aicli) tool and library installed (`pip install aicli`).

## Roles

### cluster_installation

Installs a _previously created_ [Assisted Installer cluster](https://console.redhat.com/openshift/assisted-installer/clusters), and saves its `kubeconfig` file and `kubeadmin` password.

Usage:

```yaml
roles:
- name: empovit.assisted_openshift.cluster_installation
  vars:
    cluster_name: my-cluster
    ocm_offline_token: <ocp offline token from https://console.redhat.com/openshift/token>
    temp_directory: tmp
```

The role registers the following variables:

- `kubeconfig` - path to the cluster's `kubeconfig` file
- `kubeadmin_password` - path to a file that contains the cluster's `kubadmin` password
- `assisted_cluster` - cluster info of the installed cluster

### etc_hosts

_Optionally_, updates `/etc/hosts` with the DNS entries of a newly installed OpenShift cluster. Can be useful if you cannot create public DNS entries.

By default, invoking this role does nothing, you need to tell it explicitly to update the file: `update_etc_hosts: yes`.

> Note that a backup of the updated file will be created.

Usage:

```yaml
roles:
- name: empovit.assisted_openshift
  vars:
    update_etc_hosts: yes
    api_vip: "{{ assisted_cluster.api_vip }}"
    ingress_vip: "{{ assisted_cluster.ingress_vip }}"
    cluster_name: "{{ assisted_cluster.name }}"
    openshift_base_domain: "{{ assisted_cluster.base_dns_domain }}"
```