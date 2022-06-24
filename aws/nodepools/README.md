# Node Pools on Cluster Create 

This overlay adds the ability to specify custom node pools when creating a TKG cluster. typically this is done after the fact with the CLI. This allows for using the same cluster config to specify node pool details at creation. Also you can combine this with [kapp managed clusters](https://github.com/warroyo/future-blog/tree/main/TKG/kapp-managed-clusters) to get declarative updates and addition of nodepools.


# Usage 

Copy the `nodepools_values.yml` and the `nodepools.yml` into the `~/.config/tanzu/tkg/providers/ytt/03_customizations` folder on your workstation

Copy the content from `cluster_config.yml` into your config for the cluster you are building. update the values. This uses the same config format defined in the [TKG docs for node pools](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.5/vmware-tanzu-kubernetes-grid-15/GUID-tanzu-k8s-clusters-node-pool.html#aws-configuration-3)


create your cluster as usual


![](images/2022-06-24-12-35-20.png)