# add tolerations for AKO pod on windows clusters

this can be applied after the fact to a mgmt cluster in order to allow for the AKO pods to run on the control plane nodes which is necessary on windows clusters


# Usage

Connect to your management cluster and run the following.

on linux

```
export AKO_OVERLAY=`cat ako-toleration-overlay.yml| base64 -w 0`
```

on mac:

```
export AKO_OVERLAY=`cat ako-toleration-overlay.yml| base64`
```

Run the below commands to patch the secret and add the overlay.

get the secret name to modify. the below command will list out some secrets, grab the name of the one that has your cluster name as a prefix.


```
kubectl get secrets | grep ingress
```

```
kubectl patch secret <secret-from-above> -p '{"data": {"overlays.yaml": "'$AKO_OVERLAY'"}}'
```


connect to your windows cluster and you should see the AKO pod recreate shortly after.
