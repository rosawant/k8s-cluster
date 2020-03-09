# HELM Chart application setup

Ref docd::https://medium.com/@gajus/the-missing-ci-cd-kubernetes-component-helm-package-manager-1fe002aac680
```
helm create myapp

[root@ ~]# helm install myapp/
Error: no available release name found

[root@ myapp]# kubectl create serviceaccount --namespace kube-system tiller
serviceaccount/tiller created

[root@ myapp]# kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
clusterrolebinding.rbac.authorization.k8s.io/tiller-cluster-rule created

[root@ myapp]# kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
deployment.extensions/tiller-deploy patched

[root@ ~]# helm install myapp/
NAME:   maudlin-wombat
LAST DEPLOYED: Sun Mar  8 08:24:15 2020
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Service
NAME        TYPE      CLUSTER-IP      EXTERNAL-IP  PORT(S)         AGE
hello-node  NodePort  <ClusterIP>  <none>       8080:31064/TCP  0s

==> v1beta1/Deployment
NAME        DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
hello-node  1        1        1           0          0s

==> v1/Pod(related)
NAME                         READY  STATUS             RESTARTS  AGE
hello-node-78cd77d68f-294pw  0/1    ContainerCreating  0         0s


NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=myapp,app.kubernetes.io/instance=maudlin-wombat" -o jsonpath="{.items[0].metadata.name}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl port-forward $POD_NAME 8080:80

```
We can ignore above warning



