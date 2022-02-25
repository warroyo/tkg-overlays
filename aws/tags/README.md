# Custom tags for aws resources

This overlay adds the ability to specify custom tags that will be added to the created aws resources. 


# Usage 

Copy the `aws_tags_values.yml` and the `aws_tags.yml` into the `~/.config/tanzu/tkg/providers/ytt/03_customizations` folder on your workstation

Copy the content from `cluster_config.yml` into your config for the cluster you are building. update the values.


create your cluster as usual