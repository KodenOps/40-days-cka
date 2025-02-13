# INTRODUCTION TO PODS CREATION:
## What is a pod:
Pod is the smallest deployable unit of kubernetes. Let's see pod a unit of a microservice that shares a particular task. For example, if you have an application (bank app) that have multiple loosely-coupled services (transfer, buy airtime, pay bills, etc). A service such as transfer can be on a single pod (called Transfer-pod), and others in  separate pods.

## Single container pod vs Multi-container pods
Kindly note that pods can either be a single container or multi-container. As we know that there can be instances where the full functionality of a service will require one or two smaller services. A web server functionality can be served by a single container pod (nginx) while a log management pod might have multiple container that fetch and index log in order for it to perform the main task.

## Imperative vs Declarative way of creating pods
### Imperative: 
Life is quite cool that we can create pods explicitly using built-in command and this will create out of the box pods. we give the system the little information (e.g. name of pod, namespaces, port etc) and the system handles the rest

- Specifying only pod name
``` kubectl run <Name of Pod> --image=<Image name>:tag  ```  

``` kubectl run apache-kafka --image=apache/kafka:latest ```

- Specifying port and namespace
``` kubectl run <Name of Pod> --image=<Image name>:tag  namespace-Label --port=xxxx```  

``` kubectl run apache-kafka --image=apache/kafka:latest monitoring-namespace --port=9094```

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
`` kubectl create pod -f run-nginx-pod.yml ``