# Change out the AMI ID for AWS machines

This overlay adds the ability to specify custom a custom AMI ID that will be injected into the AWSMachinetemplate and used for all VMs. This is helpful for using in combination with image builder.


# Usage 

Copy the `ami_swap_values.yml` and the `ami_swap.yml` into the `~/.config/tanzu/tkg/providers/ytt/03_customizations` folder on your workstation

Copy the content from `cluster_config.yml` into your config for the cluster you are building. update the values.


create your cluster as usual