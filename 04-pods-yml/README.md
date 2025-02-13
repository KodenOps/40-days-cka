# INTRODUCTION TO PODS CREATION:
## What is a pod:
A Pod is the smallest deployable unit in Kubernetes. Think of it as a bucket that holds one or more containers, allowing them to share the same network (IP), storage, and communicate seamlessly with each other. 


## Single container pod vs Multi-container pods
Kindly note that pods can either be a single container or multi-container. As we know that there can be instances where the full functionality of a service will require one or two smaller services. A web server functionality can be served by a single container pod (nginx) while a log management pod might have multiple container that fetch and index log in order for it to perform the main task.

So, let's pick ELK stack now. The rule of thumb for pods is that they should only group tightly coupled containers that must run together. For high availability, Elasticsearch nodes (es01, es02, es03) should run in separate pods to ensure better fault tolerance and scalability. Each node can have persistent storage attached, while Kibana should also run in a separate pod for UI-based log visualization.

There are situations where two or more applications are tightly coupled, e.g. Nginx + Fluentd. Fluentd strongly depends on Nginx in the log management system. Nginx is a web server that send its logs to `` /var/log/nginx ``. Fluentd on the other hand is needed to pick the logs from nginx and send for log management. Both Nginx and Fluentd need to be on the same pod for this interaction to be successful.

## Imperative vs Declarative way of creating pods
### Imperative: 
Life is quite cool that we can create pods explicitly using built-in command and this will create out of the box pods. we give the system the little information (e.g. name of pod, namespaces, port etc) and the system handles the rest

- Specifying only pod name
``` kubectl run <Name of Pod> --image=<Image name>:tag  ```  

``` kubectl run apache-kafka --image=apache/kafka:latest ```

- Specifying port and namespace
``` kubectl run <Name of Pod> --image=<Image name>:tag -n namespace-Label --port=xxxx```  

``` kubectl run apache-kafka --image=apache/kafka:latest -n monitoring-namespace --port=9094```

Just specify minimal values and let the system handles the rest

### Declarative: 
There will be an instance where we want to handle how the pods are being created yourself. This way, you create a yml file and explicitly describe every details of your pod.
How?

- create a run-nginx-pod.yml
- Edit the file using `` vi run-nginx-pod.yml `` and paste the content below
``` 

    # A K M S
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    env: demo
    type: frontend
spec:
  containers:
    - name: nginx-container
      image: nginx
      ports:
        - containerPort: 8080

 ```
- run the creation command 
`` kubectl apply -f run-nginx-pod.yml ``

### NOTE: Pods should only group tightly coupled containers that must run together.