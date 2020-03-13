# Helm chart setup

# HELM Chart application setup

Ref docd::https://medium.com/@gajus/the-missing-ci-cd-kubernetes-component-helm-package-manager-1fe002aac680

Use helm client to deploy tiller to the Kubernetes cluster:
helm init uses the ~/.kube/config configuration to connect to the Kubernetes cluster. Ensure that your configuration is referencing a cluster that is safe to make test deployments.
```
helm init
```
helm init will deploy tiller agent in  cluster

```
[root@ templates]# kubectl get svc --all-namespaces
NAMESPACE     NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
default       hello-node      NodePort    CLUSTER-IP   <none>        8080:31064/TCP   28h
default       kubernetes      ClusterIP   CLUSTER-IP       <none>        443/TCP          29h
kube-system   kube-dns        ClusterIP   CLUSTER-IP      <none>        53/UDP,53/TCP    29h
**kube-system   tiller-deploy   ClusterIP   CLUSTER-IP     <none>        44134/TCP        29h**

```
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

[root@ myapp]# helm list
NAME            REVISION        UPDATED                         STATUS          CHART           APP VERSION     NAMESPACE
kindled-seal    2               Thu Mar 12 10:10:50 2020        DEPLOYED        myapp-0.2.0     1.0             default
[root@perf05buildserver myapp]# helm history kindled-seal
REVISION        UPDATED                         STATUS          CHART           DESCRIPTION
1               Thu Mar 12 09:14:18 2020        SUPERSEDED      myapp-0.1.0     Install complete
2               Thu Mar 12 10:10:50 2020        DEPLOYED        myapp-0.2.0     Upgrade complete
[root@ myapp]#
[root@ myapp]#
```
Update chart to new revision

```
[root@ ~]# helm upgrade kindled-seal myapp

```
Get all resources created by helm chart
```
[root@ ~]# helm list
NAME            REVISION        UPDATED                         STATUS          CHART           APP VERSION     NAMESPACE
kindled-seal    2               Thu Mar 12 10:10:50 2020        DEPLOYED        myapp-0.2.0     1.0             default
[root@ ~]# helm status kindled-seal
LAST DEPLOYED: Thu Mar 12 10:10:50 2020
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ClusterRole
NAME           AGE
secret-reader  20h

==> v1/Service
NAME        TYPE       CLUSTER-IP      EXTERNAL-IP  PORT(S)   AGE
hello-node  ClusterIP  <>             <none>       8080/TCP  20h

==> v1beta1/Deployment
NAME        DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
hello-node  1        1        1           1          20h

==> v1/Pod(related)
NAME                       READY  STATUS   RESTARTS  AGE
hello-node-dff5879d-p7fps  1/1    Running  0         19h
```



