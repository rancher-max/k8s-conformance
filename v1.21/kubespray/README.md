Download and install Kubespray 2.17.1
```shell
git clone https://github.com/kubernetes-sigs/kubespray.git
git checkout v2.17.1
```

Start vagrant deployment and access kube-master
```shell
$ vagrant up
$ vagrant ssh k8s-1
```

Run [Sonobuoy](https://github.com/heptio/sonobuoy) as instructed [here](https://github.com/cncf/k8s-conformance/blob/master/instructions.md).
