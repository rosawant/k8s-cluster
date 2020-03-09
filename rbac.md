# Setup RBAC user for eks cluster

## On K8s master machine
Create iam user with EKS read permission

```
#kubectl get cm --all-namespaces
NAMESPACE     NAME                                 DATA   AGE
kube-system   aws-auth                             1      27h
kube-system   coredns                              1      27h


#kubectl edit cm/aws-auth --all-namespaces
```

```
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: v1
data:
  mapRoles: |
    - rolearn: arn:aws:iam::555555555555:role/devel-worker-nodes-NodeInstanceRole-74RF4UBDUKL6
      username: system:node:{{EC2PrivateDNSName}}
      groups:
        - system:bootstrappers
        - system:nodes
  mapUsers: |
    - userarn: arn:aws:iam::555555555555:user/admin
      username: admin
      groups:
        - system:masters
    - userarn: arn:aws:iam::111122223333:user/ops-user
      username: ops-user
      groups:
        - system:masters
```

## On agent node

Pre-requisites-
aws cli
kubectl

```
#aws eks update-kubeconfig --name  <cluster-name>
```
we wil get new /home/user/.kube/config file created
```
#kubectl get po
```

