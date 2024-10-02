# Kubernetes 

## What ? Why? 
<details>
<summary>What is Kubernetes? Why organizations are using it?</summary><br><b>

Kubernetes is an open-source system that provides users with the ability to manage, scale and deploy containerized applications.

To understand what Kubernetes is good for, let's look at some examples:

* You would like to run a certain application in a container on multiple different locations and sync changes across all of them, no matter where they run
* Performing updates and changes across hundreds of containers
* Handle cases where the current load requires to scale up (or down)

</b></details>

<details>
<summary>When or why NOT to use Kubernetes?</summary><br><b>

  - If you manage low level infrastructure or baremetals, Kubernetes is probably not what you need or want
  - If you are a small team (like less than 20 engineers) running less than a dozen of containers, Kubernetes might be an overkill (even if you need scale, rolling out updates, etc.). You might still enjoy the benefits of using managed Kubernetes, but you definitely want to think about it carefully before making a decision on whether to adopt it.

</b></details>

<details>
<summary>What are some of Kubernetes features?</summary><br><b>

  - Self-Healing: Kubernetes uses health checks to monitor containers and run certain actions upon failure or other type of events, like restarting the container
  - Load Balancing: Kubernetes can split and/or balance requests to applications running in the cluster, based on the state of the Pods running the application
  - Operators: Kubernetes packaged applications that can use the API of the cluster to update its state and trigger actions based on events and application state changes
  - Automated Rollout: Gradual updates roll out to applications and support in roll back in case anything goes wrong
  - Scaling: Scaling horizontally (down and up) based on different state parameters and custom defined criteria
  - Secrets: you have a mechanism for storing user names, passwords and service endpoints in a private way, where not everyone using the cluster are able to view it

</b></details>

<details>
<summary> Imperative vs Declarative approach in Kubernetes </summary><br><b>

  - Imperative: In the imperative approach, you tell Kubernetes what to do by running specific commands directly. This is similar to running a shell command to perform a task.
    Example:
    ```
    kubectl run nginx --image=nginx
    kubectl delete pod nginx
    kubectl expose deployment nginx --port=80
    ```

  - Declarative: In the declarative approach, you tell Kubernetes what the desired state of the system should be by providing configuration files (YAML or JSON), and Kubernetes ensures that the system matches this desired state.

    Example:
    ```
    STEP 1: Create a deployment.yaml and service.yaml file
    STEP 2: Apply these files to the cluster
        # kubectl apply -f deployment.yaml
        # kubectl apply -f service.yaml
    ```
</b></details>

## Kubernetes Architecture

![Kubernetes Architecture Diagram](./images/kubernetes-cluster-architecture.svg)

## Kubernetes Components 

<details>
<summary>What are the components of the master node (aka control plane)?</summary><br><b>

  * API Server - the Kubernetes API. All cluster components communicate through it
  * Scheduler - assigns an application with a worker node it can run on
  * Controller Manager - cluster maintenance (replications, node failures, etc.)
  * etcd - stores cluster configuration

</b></details>

<details>
<summary>What are the components of a worker node (aka data plane)?</summary><br><b>

  * Kubelet - an agent responsible for node communication with the master.
  * Kube-proxy - load balancing traffic between app components
  * Container runtime - the engine runs the containers (Podman, Docker, ...)

</b></details>


## Kubernetes Objects

<details>
<summary>What Kubernetes objects are there?</summary><br><b>

  * Pod - the smallest unit in Kubernetes, a group of containers
  * ReplicaSet - ensures a specified number of pod replicas are running at any given time
  * Deployment - manages ReplicaSets and updates them with new features or changes in the application
  * DaemonSet - ensures that all nodes run a copy of a Pod
  * Namespace - a way to divide cluster resources between multiple users
  * ConfigMap - a way to store configuration data in the key-value format
  * Secret - a way to store sensitive information in the key-value format
  ...
</b></details>

## Tools to work with Kubernetes

<details>
<summary>What is kubectl?</summary><br><b>

Kubectl is the Kubernetes command line tool that allows you to run commands against Kubernetes clusters. For example, you can use kubectl to deploy applications, inspect and manage cluster resources, and view logs.

</b></details>

<details>
<summary>What is MiniKube?</summary><br><b>

Minikube is a tool that makes it easy to run Kubernetes locally. Minikube runs a single-node Kubernetes cluster inside a VM on your laptop for users looking to try out Kubernetes or develop with it day-to-day.

</b></details>

<details> 
<summary>What are managed Kubernetes services (EKS, GKE, AKS)? </summary><br><b>

  - Google Kubernetes Engine (GKE)
  - Amazon Elastic Kubernetes Service (EKS)
  - Azure Kubernetes Service (AKS)

</b></details>

<details>
<summary> What is Helm? </summary><br><b>

Helm is a package manager for Kubernetes that allows you to define, install, and upgrade even the most complex Kubernetes applications. Helm uses a packaging format called charts, which are a collection of files that describe a related set of Kubernetes resources.

</b></details>



# Hands-on 

1. Install Minikube
2. Start Minikube
```
# minikube start
```
3. Write Kubernetes manifest files
```
# kubectl create deployment deploy-warehouseapi --image=prajwalt3498/warehouseapi:latest --replicas=2 --dry-run=client -o yaml > deployment-warehouseap
i.yaml

# kubectl expose deployment deploy-warehouseapi --type=NodePort --port=6789 --name=svc-warehouseapi --dry-run=client -o yaml > service-warehouseapi.yaml
```
4. Deploy the application to Minikube (using kubectl and manifest files)
```
# kubectl apply -f deployment-warehouseapi.yaml
# kubectl apply -f service-warehouseapi.yaml

# minikube service <SERVICE_NAME_FROM_SERVICE.YAML>
```

5. Scale the application
```
# kubectl scale deployment.v1.apps/<DEPLOYMENT_NAME> --replicas=3
```


# Kubectl Cheatsheet

## CREATE OPERATION (AVOID USING `kubectl create` use `kubectl apply` instead)

Create a new namespace
```
# kubectl apply -f namespace.yaml
```

Create a new deployment
```
# kubectl apply -f deployment.yaml
```

Create a new service
```
# kubectl apply -f service.yaml
```

## READ OPERATION
Get all nodes in the cluster
```
# kubectl get nodes
```
Get all namespaces
```
# kubectl get namespaces
```

Get all deployments in the current namespace
```
# kubectl get deployments
```

Get all replica sets in the current namespace
```
# kubectl get replicasets
```

Get all pods in the current namespace
```
# kubectl get pods
```

Get all services in the current namespace
```
# kubectl get services
```

### READ OPERATION WITH MORE DETAILS

Get all nodes in the cluster with more details
```
# kubectl describe nodes <NODE_NAME>
```

Get all deployments in the current namespace with more details
```
# kubectl describe deployments <DEPLOYMENT_NAME>
```

Get all replica sets in the current namespace with more details
```
# kubectl describe replicasets <REPLICASET_NAME>
```

Get all pods in the current namespace with more details
```
# kubectl describe pods <POD_NAME>
```

Get all services in the current namespace with more details
```
# kubectl describe services <SERVICE_NAME>
```


## UPDATE OPERATION (Same as CREATE operation but now you are updating the existing object by using new configuration file)

Update a deployment
```
# kubectl apply -f deployment.yaml
```

Update a service
```
# kubectl apply -f service.yaml
```

## DELETE OPERATION

Delete a deployment
```
# kubectl delete deployment <DEPLOYMENT_NAME>
```

Delete a service
```
# kubectl delete service <SERVICE_NAME>
```

Delete a pod
```
# kubectl delete pod <POD_NAME>
```

Delete a namespace
```
# kubectl delete namespace <NAMESPACE_NAME>
```

