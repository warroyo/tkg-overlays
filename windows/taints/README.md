# overlay to remove control plane taints

This overlay removes the no schedule taints from the control plane nodes so that windows clusters can run AVI, pinniped and TMC components.

## Usage

Copy the `removecptaints.yml` into the `~/.config/tanzu/tkg/providers/ytt/03_customizations` folder on your workstation.


create your cluster as usual