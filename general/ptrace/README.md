# Set the ptrace scope to allow for ptrace system calls from containers

this overlay adds a file and a command to the `kubeadmconfig` this will set the ptrace_scope value to `0` . This is particulalry useful on photon os nodes where it is set to `1` and prevents containers from using `SYSTEM_PTRACE` capabilities for things like debuggers doing dumps. see more [here](https://www.kernel.org/doc/Documentation/security/Yama.txt)

## Usage

Copy the `ptrace_scope.yml` and the `ptrace_scope_values.yml` into the `~/.config/tanzu/tkg/providers/ytt/03_customizations` folder on your workstation

Copy the content from `cluster_config.yml` into your config for the cluster you are building. Set `PTRACE: "true` .


create your cluster as usual