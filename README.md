# eks-aws-auth-backdoor
A tool that can be used while working on the aws-auth ConfigMap within an EKS cluster in the event that there is a possibility of having access revoked

## Why

Sometimes when working with `aws-auth` ConfigMap in EKS clusters, there is a possibility to have the access revoked by mistake. See https://stackoverflow.com/questions/59085639/mistakenly-updated-configmap-aws-auth-with-rbac-lost-access-to-the-cluster for an example.

The only way to recover the access to the cluster, is via the IAM user or Role that who first created the cluster.

This is not viable all the times, and this project aims to "bypass" this limitation, adding a cluster-admin shell exposed with Gotty via LoadBalancer/NodePort and password protected.

This project **must** be used with caution, and **only** while operating in the `aws-auth` ConfigMap

## Deploy with Kustomize

In the `kustomize` directory, change the value of the `CREDENTIALS` environment variable with your desired username and password.

Deploy the project with:

```bash
cd kustomize
kustomize build . | kubectl apply -f -
```

A deployment named `eks-aws-auth-backdoor` will be created in the `kube-system` namespace, with a ClusterRoleBinding and a ServiceAccount
attached to `cluster-admin` ClusterRole.

Get the URL of the LoadBalancer with:

```bash
kubectl get svc eks-aws-auth-backdoor -n kube-system
```