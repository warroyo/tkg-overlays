# Custom subnets for the control plane load balancer

This overlay adds the ability to specify custom subnets that can be used for the control plane loadbalancer. This can be used in collaboration with `AWS_LOAD_BALANCER_SCHEME_INTERNAL` 


# Usage 

Copy the `aws_lb_subnets_values.yml` and the `aws_lb_subnets.yml` into the `~/.config/tanzu/tkg/providers/ytt/03_customizations` folder on your workstation

Copy the content from `cluster_config.yml` into your config for the cluster you are building. update the values.


create your cluster as usual