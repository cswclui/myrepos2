# Deployment through manifest file

## Deploying a Pod

Create the following manifest file.

{% code title="mypod.yaml" %}
```yaml
apiVersion: v1 #version of the API to use
kind: Pod #What kind of object we're deploying
metadata: #information about our object we're deploying
  name: nginx-pod
spec: #specifications for our object
  containers:
  - name: nginx-container #the name of the container within the pod
    image: stenote/nginx-hostname #the image to be pulled
    ports:
    - containerPort: 80 #the port of the container within the pod
```
{% endcode %}

Execute:

```text
cat <<EOF >mypod.yaml
apiVersion: v1 #version of the K8S API to use
kind: Pod #the kind of object we're deploying
metadata: #name of the pod
  name: nginx-pod
spec: #specifications for our object
  containers:
  - name: nginx-container #the name of the container within the pod
    image: stenote/nginx-hostname #which container image should be pulled
    ports:
    - containerPort: 80 #the port of the container within the pod
EOF

```

Check that the manifest file is correctly created.

```text
controlplane $ cat mypod.yaml
apiVersion: v1 #version of the K8S API to use
kind: Pod #the kind of object we're deploying
metadata: #name of the pod
  name: nginx-pod
spec: #specifications for our object
  containers:
  - name: nginx-container #the name of the container within the pod
    image: stenote/nginx-hostname #which container image should be pulled
    ports:
    - containerPort: 80 #the port of the container within the pod
```

Deploy our pod by running the following command

```text
controlplane $ kubectl apply -f mypod.yaml
pod/nginx-pod created
```

```text
ontrolplane $ kubectl describe pod nginx-pod
Name:         nginx-pod
Namespace:    default
Priority:     0
Node:         node01/172.17.0.33
Start Time:   Thu, 08 Apr 2021 17:42:04 +0000
Labels:       <none>
Annotations:  Status:  Running
IP:           10.244.1.10
IPs:
  IP:  10.244.1.10
Containers:
  nginx-container:
    Container ID:   docker://367d82aa2454ef5e68ae3dd607e220f44144d258bbd912eec3e142cfd698e3c8
    Image:          stenote/nginx-hostname
    Image ID:       docker-pullable://stenote/nginx-hostname@sha256:4df775b012b985dc5875e3bfc730a3fbf802b9b1902b57e2ec05e541c2ff8f18
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 08 Apr 2021 17:42:06 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-h4xhv (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-h4xhv:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-h4xhv
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  3m52s  default-scheduler  Successfully assigned default/nginx-pod to node01
  Normal  Pulling    3m51s  kubelet, node01    Pulling image "stenote/nginx-hostname"
  Normal  Pulled     3m50s  kubelet, node01    Successfully pulled image "stenote/nginx-hostname"
  Normal  Created    3m50s  kubelet, node01    Created container nginx-container
  Normal  Started    3m50s  kubelet, node01    Started container nginx-container
```

Delete the created pod

```text
controlplane $ kubectl delete pod/nginx-pod
pod "nginx-pod" deleted
```

## Creating a Deployment

We will create the following manifest file.

{% code title="mydeployment.yaml" %}
```yaml
apiVersion: apps/v1 
kind: Deployment 
metadata: 
  name: nginx-deployment 
  labels: 
    app: nginx
spec: 
  replicas: 2 
  selector: 
    matchLabels:
      app: nginx 
  template: #The pod template that gets deployed
    metadata:
      labels: 
        app: nginx
    spec:
      containers:
      - name: nginx-container 
        image: stenote/nginx-hostname  
        ports:
        - containerPort: 80 
---
apiVersion: v1 
kind: Service 
metadata: 
  name: nginx-service
spec: 
  type: NodePort 
  ports: 
  - name: http
    port: 80
    targetPort: 80
    nodePort: 30001 
    protocol: TCP
  selector: #Label selector used to identify pods
    app: nginx
```
{% endcode %}

Some comments on the fields are shown below.

{% code title="mydeployment.yaml" %}
```yaml
apiVersion: apps/v1 #version of the API to use
kind: Deployment #What kind of object we're deploying
metadata: #information about our object we're deploying
  name: nginx-deployment #Name of the deployment
  labels: #A tag on the deployments created
    app: nginx
spec: #specifications for our object
  replicas: 2 #The number of pods that should always be running
  selector: #which pods the replica set should be responsible for
    matchLabels:
      app: nginx #any pods with labels matching this I'm responsible for.
  template: #The pod template that gets deployed
    metadata:
      labels: #A tag on the replica sets created
        app: nginx
    spec:
      containers:
      - name: nginx-container #the name of the container within the pod
        image: stenote/nginx-hostname  #which container image should be pulled
        ports:
        - containerPort: 80 #the port of the container within the pod     
        
---
apiVersion: v1 #version of the API to use
kind: Service #What kind of object we're deploying
metadata: #information about our object we're deploying
  name: nginx-service #Name of the service
spec: #specifications for our object
  type: NodePort #port exposted in the nodes
  ports: 
  - name: http
    port: 80 #service' port
    targetPort: 80 #container's pod
    nodePort: 30001 
    protocol: TCP
  selector: #Label selector used to identify pods
    app: nginx
```
{% endcode %}

Execute the following command to create the file.

```text
cat <<EOF >mydeployment.yaml
apiVersion: apps/v1 
kind: Deployment 
metadata: 
  name: nginx-deployment 
  labels: 
    app: nginx
spec: 
  replicas: 2 
  selector: 
    matchLabels:
      app: nginx 
  template: #The pod template that gets deployed
    metadata:
      labels: 
        app: nginx
    spec:
      containers:
      - name: nginx-container 
        image: stenote/nginx-hostname  
        ports:
        - containerPort: 80 
---
apiVersion: v1 
kind: Service 
metadata: 
  name: nginx-service
spec: 
  type: NodePort 
  ports: 
  - name: http
    port: 80
    targetPort: 80
    nodePort: 30001 
    protocol: TCP
  selector: #Label selector used to identify pods
    app: nginx
EOF

```

Create the deployment.

```text
 kubectl apply -f mydeployment.yaml
```

{% tabs %}
{% tab title="Sample output" %}
```text
controlplane $ kubectl apply -f mydeployment.yaml
deployment.apps/nginx-deployment created
```
{% endtab %}
{% endtabs %}





## 

## 

## References

* [https://theithollow.com/2019/01/26/getting-started-with-kubernetes/](https://theithollow.com/2019/01/26/getting-started-with-kubernetes/)



