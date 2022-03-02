# increase kubeadm verbosity

this overlay adds a an option to the `kubeadmconfig` for control planes and worker nodes to increase or decrease log level.

## Usage

Copy the `increase_logging.yml` and the `increase_logging_values.yml` into the `~/.config/tanzu/tkg/providers/ytt/03_customizations` folder on your workstation

Copy the content from `cluster_config.yml` into your config for the cluster you are building. update the value to have the logging level you would like.


create your cluster as usual