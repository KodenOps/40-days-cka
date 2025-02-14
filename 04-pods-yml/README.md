# Introduction to Kubernetes Pods Creation

## What is a Pod?
A Pod is the smallest deployable unit in Kubernetes. Think of it as a **container for containers**, allowing them to share the same network (IP), storage, and communicate seamlessly with each other.

## Single-Container Pods vs. Multi-Container Pods
Pods can either contain a **single container** or **multiple containers**. There are cases where a service requires additional supporting containers. For example:

- A **single-container pod** can be used to run a web server like **Nginx**.
- A **multi-container pod** might be required for services that rely on helper containers, such as a log management pod that fetches and indexes logs.

### Example of Single-container Pod: ELK Stack
The **rule of thumb** is to group only **tightly coupled** containers that must run together.
- **Elasticsearch nodes** (es01, es02, es03) should run in **separate pods** for high availability, fault tolerance, and scalability. Each node can have persistent storage attached.
- **Kibana** should also run in a **separate pod** to provide UI-based log visualization.

### Example of Multi-Container Pod: Nginx + Fluentd (Sidecar Pattern)
In a log management system, **Fluentd** depends on **Nginx** to collect logs:
- Nginx logs are stored in `/var/log/nginx`.
- Fluentd picks up these logs and forwards them to a log management system.
- Since they must work together, **both Nginx and Fluentd should be in the same pod**.

## Imperative vs. Declarative Pod Creation
### Imperative Approach
This method is quick and easy, allowing you to create pods using built-in commands. Kubernetes handles most of the configuration.

#### Create a Pod by specifying only the image:
```sh
kubectl run <pod-name> --image=<image-name>:<tag> --restart=Never
```
Example:
```sh
kubectl run apache-kafka --image=apache/kafka:latest --restart=Never
```

#### Create a Pod with a specific namespace and port:
```sh
kubectl run <pod-name> --image=<image-name>:<tag> -n <namespace> --port=<port> --restart=Never
```
Example:
```sh
kubectl run apache-kafka --image=apache/kafka:latest -n monitoring-namespace --port=9094 --restart=Never
```

To verify the created pods:
```sh
kubectl get pods -n monitoring-namespace
```

### Declarative Approach
This method allows for greater control by explicitly defining the pod configuration in a YAML file. It is preferred for production environments where configurations are stored in version control.

#### Steps:
1. Create a YAML file:
   ```sh
   touch run-nginx-pod.yml
   ```
2. Edit the file:
   ```sh
   vi run-nginx-pod.yml
   ```
3. Add the following content:
   ```yaml
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
4. Apply the configuration:
   ```sh
   kubectl apply -f run-nginx-pod.yml
   ```
5. Verify the pod creation:
   ```sh
   kubectl get pods
   ```

## Troubleshooting Tips
- Check pod status:
  ```sh
  kubectl get pods
  ```
- View detailed pod information:
  ```sh
  kubectl describe pod <pod-name>
  ```
- View pod logs:
  ```sh
  kubectl logs <pod-name>
  ```

By following these methods, you can efficiently create and manage Kubernetes pods for different use cases.
