# ha1az cluster plan

this is a plan that allows for deploying an HA cluster into 1az in aws. this will update the replicas to be 3 for CP and workers. it then uses the defaults for the dev plan.


# Usage 

copy the `cluster-template-definition-ha1az.yaml` into `~/.config/tanzu/tkg/providers/infrastructure-aws/<version>/cluster-template-definition-ha1az.yaml` 

copy `ha1az.yml` into `~/.config/tanzu/tkg/providers/ytt/03_customizations` 

specify the plan name in the cluster config `CLUSTER_PLAN: ha1az`
