# Conformance testing Ridge Kubernetes Service

### Create RKS cluster

To create a cluster:

```
POST https://api.ridge.co/rks/unstable/orgs/{org}/projects/{project}/clusters
```
`org` is the identifier of your organization

`project` is the project where you want to create the cluster.

Place your API key in the header as the token  in `Authorization: Bearer` <Token>

Create a cluster by submitting the following JSON
```
{
    "name": "my-cluster",
    "display_name": "my-cluster",
    "highly_available": true,
    "desired_version": "1.21.7,
    "desired_locations": [
        "US--New York"
    ],
    "node_pools": [
	    {
	      "auto_scaling": false,
	      "display_name": "Pool 1",
	      "initial_kubernetes_labels": {},
	      "initial_taints": [],
	      "max_nodes": 5,
	      "min_nodes": 5,
	      "name": "pool1",
	      "quantities": {"cpu_cores":2,"ram":2147483648,"ephemeral_storage":2147483648}
	    }
  ]
}
```

for more details and api referance refer to https://dev.ridge.co

### Run conformance tests

Once the RKS cluster has been provisioned, connect to the cluster and commence the conformance test by following these steps:

```
$ go get -u -v github.com/vmware-tanzu/sonobuoy

$ sonobuoy run --mode=certified-conformance

$ sonobuoy status

$ sonobuoy logs

$ sonobuoy retrieve ./results

# untar the tarball, then add plugins/e2e/results/global/{e2e.log,junit_01.xml}
```

