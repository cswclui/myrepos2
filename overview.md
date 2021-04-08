# Overview

> Try the interactive exercise at: [https://www.katacoda.com/rwclui/scenarios/kubernetes](https://www.katacoda.com/rwclui/scenarios/kubernetes)

> kubectl commands: [https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

## Exploring the environment

Wait for Kubernetes cluster to be initialized.

```text
controlplane $ launch.sh 
Waiting for Kubernetes to start... Kubernetes started
```

Print the client and server version information.

```text
controlplane $ kubectl version
Client Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.0", GitCommit:"9e991415386e4cf155a24b1da15becaa390438d8", GitTreeState:"clean", BuildDate:"2020-03-25T14:58:59Z", GoVersion:"go1.13.8", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.0", GitCommit:"9e991415386e4cf155a24b1da15becaa390438d8", GitTreeState:"clean", BuildDate:"2020-03-25T14:50:46Z", GoVersion:"go1.13.8", Compiler:"gc", Platform:"linux/amd64"}
```

View the nodes in the cluster

```text
controlplane $ kubectl get nodes
NAME           STATUS   ROLES    AGE   VERSION
controlplane   Ready    master   14m   v1.18.0
node01         Ready    <none>   14m   v1.18.0
```

Use the  option `-o wide`to list more information about the node \(e.g. the IP address of the nodes in the cluster\) 

```text
$ kubectl get nodes -o wide 
NAME           STATUS   ROLES    AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION       CONTAINER-RUNTIME
controlplane   Ready    master   22m   v1.18.0   172.17.0.54   <none>        Ubuntu 18.04.5 LTS   4.15.0-122-generic   docker://19.3.13
node01         Ready    <none>   22m   v1.18.0   172.17.0.60   <none>        Ubuntu 18.04.5 LTS   4.15.0-122-generic   docker://19.3.13
```

## Create Deployment

Create a deployment named `mywebserver` with the `nginx`image.

```text
kubectl create deployment mywebserver --image=stenote/nginx-hostname

```

Get all resources

{% tabs %}
{% tab title="Command" %}
```text
kubectl get all 
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Sample Output" %}
```text
NAME                               READY   STATUS              RESTARTS   AGE
pod/mywebserver-5cb858fd59-mjh72   0/1     ContainerCreating   0          11s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   9m22s

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mywebserver   0/1     1            0           11s

NAME                                     DESIRED   CURRENT   READY   AGE
replicaset.apps/mywebserver-5cb858fd59   1         1         0       11s
```
{% endtab %}
{% endtabs %}

Show the details about the `mywebserver`deployment.

```text
kubectl describe deployment mywebserver
```

{% tabs %}
{% tab title="Sample Output" %}
```text
Name:                   mywebserver
Namespace:              default
CreationTimestamp:      Thu, 08 Apr 2021 12:29:27 +0000
Labels:                 app=mywebserver
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=mywebserver
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=mywebserver
  Containers:
   nginx:
    Image:        nginx:latest
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   mywebserver-5cb858fd59 (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  3m25s  deployment-controller  Scaled up replica set mywebserver-5cb858fd59 to 1
```
{% endtab %}
{% endtabs %}

Show the details about the pods

```text
kubectl describe pod
```

```text
Name:         mywebserver-6556654865-k5bwj
Namespace:    default
Priority:     0
Node:         node01/172.17.0.33
Start Time:   Thu, 08 Apr 2021 16:46:40 +0000
Labels:       app=mywebserver
              pod-template-hash=6556654865
Annotations:  <none>
Status:       Running
IP:           10.244.1.7
IPs:
  IP:           10.244.1.7
Controlled By:  ReplicaSet/mywebserver-6556654865
Containers:
  hello:
    Container ID:   docker://f466d3ba3c570a3b3d3a0dba4c8fab1b8a720a6235e4a63b8998d554973186a2
    Image:          nginxdemos/hello
    Image ID:       docker-pullable://nginxdemos/hello@sha256:f5a0b2a5fe9af497c4a7c186ef6412bb91ff19d39d6ac24a4997eaed2b0bb334
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Thu, 08 Apr 2021 16:46:44 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-9kcqc (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-9kcqc:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-9kcqc
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age        From               Message
  ----    ------     ----       ----               -------
  Normal  Scheduled  <unknown>  default-scheduler  Successfully assigned default/mywebserver-6556654865-k5bwj to node01
  Normal  Pulling    4m19s      kubelet, node01    Pulling image "nginxdemos/hello"
  Normal  Pulled     4m16s      kubelet, node01    Successfully pulled image "nginxdemos/hello"
  Normal  Created    4m16s      kubelet, node01    Created container hello
  Normal  Started    4m16s      kubelet, node01    Started container hello
```

## Create a NodePort Service

Create a NodePort Service object that exposes the deployment:

{% tabs %}
{% tab title="Command" %}
```text
kubectl create service nodeport mywebserver --tcp 10080:80 --node-port=30000
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Sample output" %}
```text
controlplane $ kubectl create service nodeport mywebserver --tcp 10080:80 --node-port=30000
service/mywebserver created
```
{% endtab %}
{% endtabs %}

Check the created service.

{% tabs %}
{% tab title="Command" %}
```text
kubectl get services 
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Sample output" %}
```text
controlplane $ kubectl get services 
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE
kubernetes    ClusterIP   10.96.0.1       <none>        443/TCP           53m
mywebserver   NodePort    10.98.254.151   <none>        10080:30000/TCP   11m
```
{% endtab %}
{% endtabs %}

Check all the resources associated with the label `app=mywebserver`

```text
controlplane $ kubectl get all -o wide --selector=app=mywebserver
NAME                               READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
pod/mywebserver-5cb858fd59-fs6dk   1/1     Running   0          33m   10.244.1.3   node01   <none>           <none>

NAME                  TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE     SELECTOR
service/mywebserver   NodePort   10.98.254.151   <none>        10080:30000/TCP   9m46s   app=mywebserver

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES         SELECTOR
deployment.apps/mywebserver   1/1     1            1           33m   nginx        nginx:latest   app=mywebserver

NAME                                     DESIRED   CURRENT   READY   AGE   CONTAINERS   IMAGES         SELECTOR
replicaset.apps/mywebserver-5cb858fd59   1         1         1       33m   nginx        nginx:latest   app=mywebserver,pod-template-hash=5cb858fd59
```

Connect to the web server through the created service.

{% tabs %}
{% tab title="Command" %}
```text
curl localhost:30000
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Sample output" %}
```text
controlplane $ curl localhost:31956
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
{% endtab %}
{% endtabs %}

We can also access the web-server through the hostname/IP address of the nodes \(the service will route the traffic to the appropriate node in the cluster\).

```text
controlplane $ curl controlplane:30000
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
controlplane $ curl controlplane:31956
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

```text
controlplane $ curl node01:30000
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
controlplane $ 
```

## Scaling out 

We may scale your replicaset by setting a new size for your Deployment or  ReplicatSet.

```text
kubectl scale --replicas=3 deployment/mywebserver
```

```text
controlplane $ kubectl scale --replicas=3 deployment/mywebserver
deployment.apps/mywebserver scaled
```

Check that 3 pods are created and in running state. The replicaset should indicate there are 3 instances of nginx running.

```text
controlplane $ kubectl get all -l app=mywebserver
NAME                               READY   STATUS    RESTARTS   AGE
pod/mywebserver-5cb858fd59-4gj8z   1/1     Running   0          3m13s
pod/mywebserver-5cb858fd59-7zkp5   1/1     Running   0          3m13s
pod/mywebserver-5cb858fd59-jpc9n   1/1     Running   0          5m26s

NAME                  TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)           AGE
service/mywebserver   NodePort   10.108.217.244   <none>        10080:30000/TCP   4m45s

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mywebserver   3/3     3            3           5m26s

NAME                                     DESIRED   CURRENT   READY   AGE
replicaset.apps/mywebserver-5cb858fd59   3         3         3       5m26s
```

The service will load balance the request across the multiple pods

```text
controlplane $ curl localhost:30000
mywebserver-564ccd895-m4fzf
controlplane $ curl localhost:30000
mywebserver-564ccd895-m4fzf
controlplane $ curl localhost:30000
mywebserver-564ccd895-m4fzf
controlplane $ curl localhost:30000
mywebserver-564ccd895-7lxrh
```



## others



To execute a command in the container in a certain pod, use `kubectl exec`.

> Note: you should replace the pod name with your the actual name of your pod.

```text
controlplane $ kubectl exec -it  mywebserver-5cb858fd59-4gj8z -- bash
root@mywebserver-5cb858fd59-4gj8z:/
```

Within the container, you may access other pods or service by using the internal IP address/hostname assigned by Kubernetes.

```text
 curl mywebserver.default:10080
```



You may  port-forward a port to localhost to a pod/service's port .

```text
controlplane $ kubectl port-forward pod/mywebserver-5cb858fd59-4gj8z 20080:80
Forwarding from 127.0.0.1:20080 -> 80
Forwarding from [::1]:20080 -> 80
```



We may use the `kubectl get pod` command  with the option `-o wide`to list more information, including the node the pod resides on, and the pod’s cluster IP.

```text
controlplane $ kubectl get pods -o wide 
NAME                           READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
mywebserver-5cb858fd59-phfgj   1/1     Running   0          29m   10.244.1.3   node01   <none>           <none>
```



We may select resources with the specified label\(s\) using the `-l` option.

```text
controlplane $ kubectl get all -l app=mywebserver -o wide
NAME                               READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
pod/mywebserver-5cb858fd59-phfgj   1/1     Running   0          61m   10.244.1.3   node01   <none>           <none>

NAME                 TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE   SELECTOR
service/my-service   NodePort   10.97.207.253   <none>        10080:31956/TCP   57m   app=mywebserver

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES         SELECTOR
deployment.apps/mywebserver   1/1     1            1           61m   nginx        nginx:latest   app=mywebserver

NAME                                     DESIRED   CURRENT   READY   AGE   CONTAINERS   IMAGES         SELECTOR
replicaset.apps/mywebserver-5cb858fd59   1         1         1       61m   nginx        nginx:latest   app=mywebserver,pod-template-hash=5cb858fd59
```



```text
controlplane $ kubectl get all -o wide
NAME                               READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
pod/mywebserver-5cb858fd59-phfgj   1/1     Running   0          38m   10.244.1.3   node01   <none>           <none>

NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE   SELECTOR
service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP           38m   <none>
service/my-service   NodePort    10.97.207.253   <none>        10080:31956/TCP   34m   app=mywebserver

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES         SELECTOR
deployment.apps/mywebserver   1/1     1            1           38m   nginx        nginx:latest   app=mywebserver

NAME                                     DESIRED   CURRENT   READY   AGE   CONTAINERS   IMAGES         SELECTOR
replicaset.apps/mywebserver-5cb858fd59   1         1         1       38m   nginx        nginx:latest   app=mywebserver,pod-template-hash=5cb858fd59
```

Delete deployment and service

```text
controlplane $ kubectl delete deploy/mywebserver
deployment.apps "mywebserver" deleted
controlplane $ kubectl get all
NAME                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)           AGE
service/kubernetes    ClusterIP   10.96.0.1        <none>        443/TCP           53m
service/mywebserver   NodePort    10.108.217.244   <none>        10080:30000/TCP   52m
controlplane $ kubectl delete svc/mywebserver
service "mywebserver" deleted
```



