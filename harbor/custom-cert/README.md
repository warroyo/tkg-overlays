# Bring your own cert for Harbor

This overlay provides a workaround for an issue in the Harbor package that causes teh core container to fail to start when a secret is provided in the values that does not match the default `harbor-tls` name. This also could be needed if you are using contour's tls delegation feature and the secret that the cert is in lives in a different namespace. Also this is handy for combining with cert-manager generated certs.

# Prerequisites

* a k8s secret with your cert and key in it

# Usage

## Update Harbor package values

For this overlay there is only 1 value to update. We need to change the `tlsCertificateSecretName:` to the name of the secret that we have placed our cert in.

Here is an example using a secret in another NS that is using [contour's tls delegation](https://projectcontour.io/docs/main/config/tls-delegation/) feature.

`tlsCertificateSecretName: contour-tls/tls`


## Install Harbor as usual

```
tanzu package install harbor --package-name harbor.tanzu.vmware.com --version 2.3.3+vmware.1-tkg.1 --values-file harbor-data-values.yaml -n default
```

## Add the overlay to the package

First we create a secret in the same namespace as the harbor package since in the above example we created it in `default` thats where the secret goes

```
kubectl -n default create secret generic fix-certs-overlay -o yaml --dry-run=client --from-file=fix-certs-overlay.yml | kubectl apply -f -
```

Now we need to annotate the package to add the overlay

```
kubectl -n default annotate packageinstalls harbor ext.packaging.carvel.dev/ytt-paths-from-secret-name.0=fix-certs-overlay
```


the package will eventually reconcile and you will should see that the correct cert is now on Harbor.