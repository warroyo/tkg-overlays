# Integrate Harbor with a Cert-Manager cluster issuer


This overlay will add the correct annotations to the Harbor ingress to allow for cert manager to issue a certificate for Harbor.


# Prerequisites

* cert manager installed
* cluster issuer created -  if the name is not "letsencrypt-prod" then you will need to change the name in the overlay to match
* an ingress controller that supports native k8s ingress -  this was tested with contour



# Usage

## Update Harbor package values

There are only two values that need to change specifically for this overlay. The below values should be added to the harbor values file. We need to use `harbor-tls` as the secret name since this is the default secret name and currently(3/10/2022) there is a bug in the package that will cause the harbor core container to fail. If you absolutely need to change this see [this overlay](). We are also setting `tlsCertificateSecretName` in order to prevent harbor form generating default certs. We then disable contour's HTTPProxy since it does not support the cert manager annotations. 

`tlsCertificateSecretName: harbor-tls`
`enableContourHttpProxy: false`


## Install Harbor as usual

```
tanzu package install harbor --package-name harbor.tanzu.vmware.com --version 2.3.3+vmware.1-tkg.1 --values-file harbor-data-values.yaml -n default
```

## Add the overlay to the package

First we create a secret in the same namespace as the harbor package since in the above example we created it in `default` thats where the secret goes

```
kubectl -n default create secret generic clusterissuer-overlay -o yaml --dry-run=client --from-file=add-cluster-issuer-overlay.yml | kubectl apply -f -
```

Now we need to annotate the package to add the overlay

```
kubectl -n default annotate packageinstalls harbor ext.packaging.carvel.dev/ytt-paths-from-secret-name.0=clusterissuer-overlay
```


the package will eventually reconcile and you will see a secret created called `harbor-tls` with the cert in it.