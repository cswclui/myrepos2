# Overview

Try the interactive exercise at: [https://www.katacoda.com/rwclui/scenarios/kubernetes](https://www.katacoda.com/rwclui/scenarios/kubernetes)

kubectl commands: [https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

controlplane $ launch.sh 

Waiting for Kubernetes to start... Kubernetes started



Create a deployment named `mywebserver` with the `nginx`image.

{% tabs %}
{% tab title="Command" %}
```text
 kubectl create deployment mywebserver --image=nginx:latest 
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Sample Output" %}
```text
deployment.apps/mywebserver created 
```
{% endtab %}
{% endtabs %}

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

Describe the `mywebserver` deployment.

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

Create a Service object that exposes the deployment:

{% tabs %}
{% tab title="Command" %}
```text
kubectl expose deployment mywebserver --type=NodePort --name=my-service
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Sample output" %}
```text
service/my-service exposed
```
{% endtab %}
{% endtabs %}

Check the created resource

{% tabs %}
{% tab title="Command" %}
```text
controlplane $ kubectl get service

```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Sample output" %}
```text
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP           15m
my-service   NodePort    10.97.207.253   <none>        10080:31956/TCP   11m
```
{% endtab %}
{% endtabs %}

Connect to the web server through the created service.

{% tabs %}
{% tab title="Command" %}
```text
curl localhost:

```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Sample output" %}
```text
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP           15m
my-service   NodePort    10.97.207.253   <none>        10080:31956/TCP   11m
```
{% endtab %}
{% endtabs %}

Connect to the web server through the created service.



```text
controlplane $ kubectl get nodes -o wide 
NAME           STATUS   ROLES    AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION       CONTAINER-RUNTIME
controlplane   Ready    master   22m   v1.18.0   172.17.0.54   <none>        Ubuntu 18.04.5 LTS   4.15.0-122-generic   docker://19.3.13
node01         Ready    <none>   22m   v1.18.0   172.17.0.60   <none>        Ubuntu 18.04.5 LTS   4.15.0-122-generic   docker://19.3.13
```







NAME TYPE CLUSTER-IP EXTERNAL-IP PORT\(S\) AGE service/kubernetes ClusterIP 10.96.0.1  443/TCP 4m19s service/my-service NodePort 10.97.207.253  10080:31956/TCP 20s

NAME READY UP-TO-DATE AVAILABLE AGE deployment.apps/mywebserver 1/1 1 1 4m12s

NAME DESIRED CURRENT READY AGE replicaset.apps/mywebserver-5cb858fd59 1 1 1 4m3s controlplane $ curl localhost:10080 curl: \(7\) Failed to connect to localhost port 10080: Connection refused controlplane $ curl localhost:31956 &lt;!DOCTYPE html&gt;

Welcome to nginx!  
    body {  
        width: 35em;  
        margin: 0 auto;  
        font-family: Tahoma, Verdana, Arial, sans-serif;  
    }  


## Welcome to nginx!

If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to [nginx.org](http://nginx.org/).  
 Commercial support is available at [nginx.com](http://nginx.com/).

_Thank you for using nginx._

\_\_

\_\_

\_\_





{% tabs %}
{% tab title="Command" %}
```text
kubectl describe pod myweb
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Sample Output" %}
```text
controlplane $ 
error: the server doesn't have a resource type "myweb"
controlplane $ kubectl describe pod myweb
Name:         mywebserver-5cb858fd59-mjh72
Namespace:    default
Priority:     0
Node:         node01/172.17.0.12
Start Time:   Thu, 08 Apr 2021 12:29:27 +0000
Labels:       app=mywebserver
              pod-template-hash=5cb858fd59
Annotations:  <none>
Status:       Running
IP:           10.244.1.3
IPs:
  IP:           10.244.1.3
Controlled By:  ReplicaSet/mywebserver-5cb858fd59
Containers:
  nginx:
    Container ID:   docker://8e3aaa61e71481f455ba8e65a9e12aa9c6b8ddab8af89b95eeb336d6f3eb6818
    Image:          nginx:latest
    Image ID:       docker-pullable://nginx@sha256:bae781e7f518e0fb02245140c97e6ddc9f5fcf6aecc043dd9d17e33aec81c832
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Thu, 08 Apr 2021 12:29:37 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-z6sg9 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-z6sg9:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-z6sg9
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  40m   default-scheduler  Successfully assigned default/mywebserver-5cb858fd59-mjh72 to node01
  Normal  Pulling    40m   kubelet, node01    Pulling image "nginx:latest"
  Normal  Pulled     40m   kubelet, node01    Successfully pulled image "nginx:latest"
  Normal  Created    40m   kubelet, node01    Created container nginx
  Normal  Started    40m   kubelet, node01    Started container nginx
```
{% endtab %}
{% endtabs %}





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









controlplane $ kubectl create deployment mywebserver --image=nginx:latest deployment.apps/mywebserver created 

controlplane $ 

NAME READY STATUS RESTARTS AGE pod/mywebserver-5cb858fd59-mjh72 0/1 ContainerCreating 0 11s

NAME TYPE CLUSTER-IP EXTERNAL-IP PORT\(S\) AGE service/kubernetes ClusterIP 10.96.0.1  443/TCP 9m22s

NAME READY UP-TO-DATE AVAILABLE AGE deployment.apps/mywebserver 0/1 1 0 11s

NAME DESIRED CURRENT READY AGE replicaset.apps/mywebserver-5cb858fd59 1 1 0 11s controlplane $ get nodes

Command 'get' not found, but there are 18 similar ones.

controlplane $ kubectl get nodes NAME STATUS ROLES AGE VERSION controlplane Ready master 10m v1.18.0 node01 Ready  9m32s v1.18.0 controlplane $ kubectl version Client Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.0", GitCommit:"9e991415386e4cf155a24b1da15becaa390438d8", GitTreeState:"clean", BuildDate:"2020-03-25T14:58:59Z", GoVersion:"go1.13.8", Compiler:"gc", Platform:"linux/amd64"} Server Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.0", GitCommit:"9e991415386e4cf155a24b1da15becaa390438d8", GitTreeState:"clean", BuildDate:"2020-03-25T14:50:46Z", GoVersion:"go1.13.8", Compiler:"gc", Platform:"linux/amd64"} controlplane $ kubectl describe mywebserver error: the server doesn't have a resource type "mywebserver" controlplane $ kubectl describe deployment mywebserver Name: mywebserver Namespace: default CreationTimestamp: Thu, 08 Apr 2021 12:29:27 +0000 Labels: app=mywebserver Annotations: deployment.kubernetes.io/revision: 1 Selector: app=mywebserver Replicas: 1 desired \| 1 updated \| 1 total \| 1 available \| 0 unavailable StrategyType: RollingUpdate MinReadySeconds: 0 RollingUpdateStrategy: 25% max unavailable, 25% max surge Pod Template: Labels: app=mywebserver Containers: nginx: Image: nginx:latest Port:  Host Port:  Environment:  Mounts:  Volumes:  Conditions: Type Status Reason

Available True MinimumReplicasAvailable Progressing True NewReplicaSetAvailable OldReplicaSets:  NewReplicaSet: mywebserver-5cb858fd59 \(1/1 replicas created\) Events: Type Reason Age From Message

Normal ScalingReplicaSet 3m25s deployment-controller Scaled up replica set mywebserver-5cb858fd59 to 1 controlplane $ ^C controlplane $

kubectl version

kubectl run http --image=nginx:latest kubectl expose deployment http --type=NodePort --name=http-server

kubectl create deployment mywebserver --image=nginx:latest

\`\`

\`\`

\`\`

\`\`

\`\`

\`\`

\`\`

\`\`

\`\`

\`\`

\`\`

