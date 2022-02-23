# overlay to remove control plane taints

This overlay removes the no schedule taints from the control plane nodes so that windows clusters can run AVI, pinniped and TMC components.

## Usage

Copy the `removecptaints.yml` and the `removecptaints-values.yml` into the `~/.config/tanzu/tkg/providers/ytt/03_customizations` folder on your workstation.


Copy the content from `cluster_config.yml` into your config for the cluster you are building. update the value to be true or false


create your cluster as usual