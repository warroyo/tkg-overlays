# Update Harbor to work with AKO in L7 NodePort mode

This overlay will switch the Harbor from using clusterIP to NodePort so that it will work with NSX ALB L7 ingress using NodePort mode. 


# Usage

## Update Harbor package values

For this overlay there is only 1 value to update. We need to set `enableContourHttpProxy: false`


## Install Harbor as usual

```
tanzu package install harbor --package-name harbor.tanzu.vmware.com --version 2.3.3+vmware.1-tkg.1 --values-file harbor-data-values.yaml -n default
```

## Create an HTTPRule for Avi 

This is needed to tell avi that the backend pools are listening on tls and to re-encrypt traffic.

1. Apply the HTTPRule and update the value for the harbor hostname.

```
ytt -v HARBOR_HOST=<your-harbor-hostname> -f httprule.yml | kubectl apply -f-
```



## Add the overlay to the package

First we create a secret in the same namespace as the harbor package since in the above example we created it in `default` thats where the secret goes

```
kubectl -n default create secret generic avi-l7-overlay -o yaml --dry-run=client --from-file=avi-l7-overlay.yml | kubectl apply -f -
```

Now we need to annotate the package to add the overlay

```
kubectl -n default annotate packageinstalls harbor ext.packaging.carvel.dev/ytt-paths-from-secret-name.0=avi-l7-overlay
```


the package will eventually reconcile and you should see the service now set to NodePort.