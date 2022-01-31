# cert overlay for windows nodes

this overlay adds a root CA to the windows nodes in a windows cluster. it also adds a workaround for tkg 1.4.1 for adding the vsphere server name.

## Usage

Copy the `inject-cert.yml` and the `inject-cert-values.yml` into the `~/.config/tanzu/tkg/providers/ytt/03_customizations` folder on your workstation

Copy the content from `cluster_config.yml` into your config for the cluster you are building. update the value to have the base64 encoded content of your CA cert.


create your cluster as usual