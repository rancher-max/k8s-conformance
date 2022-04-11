# Mirantis Kubernetes Engine 3.5.0

The Mirantis Kubernetes Engine 3.5.0 platform is made up of a number of
components. Kubernetes is included within MKE component.

## Installing Mirantis Kubernetes Engine 3.5.0 to reproduce the results

You will need to deploy a cluster of 1 or more machines with Mirantis Docker Engine Enterprise 20.10. or newer installed. Mirantis recommends using [Launchpad](https://github.com/Mirantis/launchpad/) tool to provision and install a cluster.

### Provision Cluster via Launchpad + Terraform
1. Install [Terraform](https://learn.hashicorp.com/terraform/getting-started/install) and [yq](https://github.com/mikefarah/yq#install).
2. Navigate to https://github.com/Mirantis/launchpad/ for examples of working with `Launchpad` and `Terraform`. In this case we'll use AWS as cloud provider; therefore we'll use [this](https://github.com/Mirantis/launchpad/tree/master/examples/terraform/aws) example as reference.
    - Caveat. If you use this example make sure ot add `windows_administrator_password = your password` to your `terraform.tfvars` file otherwise you'll get an error.
3. Follow the Steps suggested in the README.md, to user terraform to provision cluster infrastructure, and launchpad to install the Docker Enterprise suite onto the cluster:
    - terraform init
    - terraform apply
    - terraform output -json | yq r --prettyPrint - ucp_cluster.value > cluster.yaml
    - Modify the `cluster.yaml` file to include the following flag --nodeport-range=30000-32767 under the `installFlag` section
    - launchpad apply

### Setting up your local environment
1. To use MKE, you are required to have a Mirantis Kubernetes Engine subscription, or you can test the platform with a free trial license.
    1. Go to [Docker Hub](https://hub.docker.com/editions/enterprise/docker-ee-trial/trial) to get a free trial license.
    2. In your browser, navigate to the MKE web UI, log in with your administrator credentials and upload your license. Navigate to the `Admin Settings` page and in the left pane, click `License`.
    3. Click `Upload License` and navigate to your license (.lic) file. When you’re finished selecting the license, MKE updates with the new settings.
2. Download an admin certificate bundle(under your profile menu) to access the system remotely.
3. Source the `env.sh`(located in the Admin Bundle) to set up your environment for `docker` and `kubectl` CLI access.


## Run Conformance Test

1. Download Sonobuoy CLI

```
$ wget https://github.com/vmware-tanzu/sonobuoy/releases/download/v0.55.0/sonobuoy_0.55.0_linux_amd64.tar.gz
$ tar -xzf sonobuoy_0.55.0_linux_amd64.tar.gz
```

2. Launch the conformance tests

```
$ ./sonobuoy run --wait --mode=certified-conformance --kube-conformance-image-version=v1.21 --plugin-env='e2e.E2E_EXTRA_ARGS=--non-blocking-taints=com.docker.ucp.manager'
```

4. Retrieve the results

```
$ sonobuoy retrieve ./results
```

5. Optionally it is possible to clean up the Kubernetes objects created by Sonobuoy

```
./sonobuoy delete
```
