# To reproduce

## Set up the cluster

1. Login to https://metakube.syseleven.de/
2. Press the "Create Cluster" button
3. Choose aws provider and complete the create cluster wizard

When the cluster is up and running,

1. Download the kubeconfig file.  
2. Set the KUBECONFIG environment variable `export KUBECONFIG=$PWD/kubeconfig`.

You might have to allow ports 30000-32767 depending on you security settings to allow the creation of NodePorts.

## Run the conformance test

```
$ curl -L https://raw.githubusercontent.com/cncf/k8s-conformance/master/sonobuoy-conformance.yaml | kubectl apply -f -

$ kubectl logs -f -n sonobuoy sonobuoy


$ kubectl cp sonobuoy/sonobuoy:/tmp/sonobuoy ./results

$ untar the tarball, then add plugins/e2e/results/{e2e.log,junit_01.xml}
```
