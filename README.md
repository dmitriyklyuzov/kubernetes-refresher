# Kubernetes

## Resources
* [Kubernetes Full Course](https://youtu.be/X48VuDVv0do)

## Main components

**Node** - physical server
* each Node has multiple Pods on it
* **Worker nodes**
    * 3 processes run on every worker node
        * container runtime - Docker
        * Kubelet - K8s process that interacts with both the container and the node. Starts and runs the pods with a container inside.
        * Kube Proxy - forwards the requests from Services to Pods. Ensures performant communication with low overhead
* **Master nodes** - control the cluster state and the worker nodes
    * 4 processes run on every master node
        * API Server - cluster gateway. receives the initial requests to update the cluster. Gatekeeper for authentication
        * Scheduler - decides on which Node to put a new Pod (doesn’t actually run it, Kubelet does that on the worker node itself)
        * Controller manager - detect state changes (like when nodes die) and reschedules/recovers via scheduler
        * etcd - cluster brain, key/value store for changes. 
    * There are usually multiple Master nodes. The API service is load balanced and etcd stores a distributed storage across all of the master nodes.  

**Pod** - smallest unit of K8, abstraction over a container.
* Pods are ephemeral
* Creates a layer on top of a container so that the container is abstracted away.
* Runs 1 application container per pod.
* Each pod gets its own IP address.

**Deployment** - blueprint for a Pod, abstraction of a Pod
* used for stateLESS applications
* you create Deployments rather than working with Pods directly
* you can scale the number of replicas you need
* databases can’t be replicated using Deployments

**StatefulSet** - component for stateFULL Applications
* should be used over Deployments for databases (MySql, MongoDB, ElasticSearch)
* takes care of replicating and scaling stateful Pods
* deploying DBs using Stateful Sets is difficult, so DBs are often hosted outside of K8 clusters

**Volumes** - attach a physical storage to a pod so that data lifecycle is not tied to a pod itself
* K8 doesn’t manage data persistance
* data can be on local machine
* can be remote, outside of the K8 cluster
* when a Pod is restarted, data persists

**Service** - permanent IP address that’s attached to a Pod.
* Serves as a permanent IP for a Pod and as a Load Balancer between pods.
* Pods communicate with each other using a service.
* Lifecycles of pods and services are not connected (if pod dies, service stays)
* External service - service that opens communication from external service.
* Internal service - for db, etc
* `protocol://ip-address:port`

**Ingress** - pretty url, forwards the request to a service

**ConfigMap** - external configuration of your application
* contains URLs of dbs, etc
* connected to a Pod so the Pod gets the data from it
* don’t use it for credentials in plain text

**Secret** - used to store secret data (passwords, certificates)
* stored in base64, not plaintext
* the built-in security mechanism is not enabled by default

## minikube
* Creates a Virtual Box on your local machine
* A single Node runs both Master and Worker processes
* minicube CLI is used mostly for starting up or deleting the cluster
* everything else will be done with `kubectl`

```bash
minikkube start --vm-driver=hyperkit
minikube status
```

## kubectl
* CLI tool for Kubernetes clusters (minikube or Cloud)
* way to interact with a cluster

### CRUD commands

```bash
# create deployment
kubectl create deployment [name]

# edit deployment
kubectl edit deployment [name]

# delete deployment
kubectl delete deployment [name]
```

### Status of different K8s components

```bash
kubectl get nodes | pod | services | replicaset | deployment
```

### Debugging pods

```bash
# view logs
kubectl logs [pod name]

# get interactive terminal
kubectl exec -it [pod name] --bin/bash
```
