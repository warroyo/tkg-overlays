# AWS ACM Integration for Harbor

AWS provides a certificate manager and the ability to use those certs on loadbalancers. The one issue when using public ACM  is that it can't be used with something like cert manager. we need to make some changes to the default contour and harbor installs in order to make this work.


## Traffic Flow

ELB(443 TLS) -> Envoy(8080) -> Harbor(8443 TLS) 

## Prerequisites

* a cert provisioned through ACM for the domain you want to install harbor on.


## Install Contour

We will use contour still because the harbor package assumes ingress of some kind so rather than ripping it all out it's easier to use contour.


### Update the values file

We need a few annotations to make the LB use the cert and to pass traffic correctly to our modified install. add the following section to your values file.

```
envoy: 
  service: 
    annotations: 
      service.beta.kubernetes.io/aws-load-balancer-backend-protocol: tcp
      service.beta.kubernetes.io/aws-load-balancer-healthcheck-protocol: tcp
      service.beta.kubernetes.io/aws-load-balancer-ssl-cert: "<arn of cert created in ACM>"
      service.beta.kubernetes.io/aws-load-balancer-ssl-negotiation-policy: ELBSecurityPolicy-TLS-1-2-2017-01
      service.beta.kubernetes.io/aws-load-balancer-ssl-ports: https
```

Install contour per the docs.


### Add an overlay to modify backend ports

We need to change the https port to send traffic to the backend http port.

First we create a secret in the same namespace as the contour package since in the above example we created it in `contour` thats where the secret goes

```
kubectl -n contour create secret generic change-ports-overlay -o yaml --dry-run=client --from-file=change-port.yml | kubectl apply -f -
```

Now we need to annotate the package to add the overlay

```
kubectl -n contour annotate packageinstalls contour ext.packaging.carvel.dev/ytt-paths-from-secret-name.0=change-ports-overlay
```


the package will eventually reconcile and you will see that the target port on the envoy service for https is now 8080


## Install Harbor

### Update the values file

Follow the docs as usual for installing the package. we just need to make sure of two things:

* `enableContourHttpProxy` is set to `true`
* `hostname` is set to the name you used when creating the cert in ACM

### Add an overlay to turn of tls

We will need to change the `HTTPProxy` for harbor to not use tls.

First we create a secret in the same namespace as the harbor package since in the above example we created it in `default` thats where the secret goes

```
kubectl -n default create secret generic remove-tls-overlay -o yaml --dry-run=client --from-file=remove-tls.yml | kubectl apply -f -
```

Now we need to annotate the package to add the overlay

```
kubectl -n default annotate packageinstalls harbor ext.packaging.carvel.dev/ytt-paths-from-secret-name.0=remove-tls-overlay
```


the package will eventually reconcile and you will should see that the `HTTPProxy` resource is not longer using TLS and at this point harbro should be accessible.