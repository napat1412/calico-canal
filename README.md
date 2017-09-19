# calico version 1.6 over flannel network for kubernetes v1.6 (calico-canal)
This directory includes manifests for deploying canal on Kubernetes using the Kubernetes API.
The configuration file follow up calico.yaml of Coreos. https://coreos.com/kubernetes/docs/1.6.1/deploy-master.html
This calico configuration file is tested with kubernetes v1.6 and use etcd data store

## 1) Requirements
- The Kubernetes cluster must be configured to provide serviceaccount tokens to pods.
- kubelets must be started with `--network-plugin=cni` and
  have `--cni-conf-dir` and `--cni-bin-dir` properly set

### 1.1) Etcd
This configuration require Etcd service and design for Etcd TLS.
Etcd endpoint like https://127.0.0.1:2379

### 1.2) Flannel
Flannel must used in kubernetes cluster. (follow up Coreos configuration)


## 2) Configuration
### 2.1) Edit File: calico-configmap.yaml
* Replace ${ETCDENDPOINTS} with the location of your etcd server.
(In this version, we suggest you to use single url for etcd_endpoints)
### 2.2) Edit File: calico-secret.yaml
* Repalce 1st <BASE64_ENCODE> with base64 encode of etcd-key  (etcd client private-key.pem)
* Repalce 2nd <BASE64_ENCODE> with base64 encode of etcd-cert (etcd client public-key.pem)
* Repalce 3rd <BASE64_ENCODE> with base64 encode of etcd-ca   (etcd ca public-key.pem)

### 2.3) Run with kubectl
```
kubectl create -f calico-configmap.yaml
kubectl create -f calico-secret.yaml
kubectl create -f calico-node.yaml
kubectl create -f calico-policy-controller.yaml
```
