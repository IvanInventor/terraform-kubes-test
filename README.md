# Terraform-test

Testing terraform config with kubernetes provider.

## Dependencies
- terraform
- kubernetes (kubeadm, kubelet, kubectl)
- kind

## Setup
### Kubes config
- Create kubernetes cluster with provided or custom config
```bash
$ kind create cluster --name terraform-test --config kind-config.yaml
```
- point kubectl to cluster
```bash
$ kubectl cluster-info --context kind-terraform-test
```
### Provider config
- Configure provider in `terraform.tfvars`, taking variables from
```bash
$ kubectl config view --minify --flatten --context=kind-terraform-test
```
```ini
host                   = "https://127.0.0.1:32768"  /* clusters.cluster.server */
client_certificate     = "LS0tLS1CRUdJTiB..."       /* users.user.client-certificate */
client_key             = "LS0tLS1CRUdJTiB..."       /* users.user.client-key */
cluster_ca_certificate = "LS0tLS1CRUdJTiB..."       /* clusters.cluster. certificate-authority-data */
```
### Initialize terraform
```bash
$ terraform init
```
Check for running services
```bash
$ kubectl get services
```
```text
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        48m
nginx-example   NodePort    10.96.209.138   <none>        80:30201/TCP   3m47s
```

