---
title: "Kubernetes Cluster"
date: 2025-06-01 00:00:00 +0800
categories: [DevOps, Container Orchestration]
image: assets/images/kubernetes.jpg
tags: [kubernetes, k8s, containers, orchestration, cluster]
---

# Kubernetes Cluster

**The Control Plane is responsible for managing the cluster.**

**The Control Plane is responsible for managing the cluster.**

**Node-level components, such as the kubelet, communicate with the control plane using the [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)**, which the control plane exposes.


## COMPONENTS

- Pod 
	-  Generally one pod should have only one application
	- pods are epihermal
	- each pod has its own ip address
	
- Services 
	- permanent IP that can be attached to the pod
	- the life cycle of pod and services are not linked
	- Load balancer
	
- Ingress 
	- port forwarding
	- for domain linking
	
- Configmap
	  If you try to change the settings of anything say like the database pod then there arises a tedious task to build everything from the scratch. in order to avoid this config map is used as an alternative.
	  for example if you change the name of the database then you just change the settings in the configmap.
	  
- Secrets 
	- similar to configmap but to protect the things like password from getting exposed
	- encoded in base64 and encrypted
	
- Volumes

- Deployments - replica or blue prints
	-  DB cant be replicated via blueprint
	
- Stateful Set - for database replicas
	- very tough
	- so better to host the database outside the environment and connect them to the pod
basically a stateful set tracks in which database pods is the data written to avoid the inconsistencies of the data storage.

- Daemon Set
	- It automatically deploys hoe many number of replicas must be run in the nodes
	- it also guarantees only one replica per pod
	




## K8S Architecture

- KUBELET - interacts with both  container runtime and the machine itself
- Container Runtime  (Containerd , Docker )
- KUBE PROXY - communication via services
		- i.e.  instead of sending the data or the request tot he replica the kube proxy send it to the node from which it was requested




#### MASTER SERVICES  (Control Plane Nodes)

- API Server - cluster gateway
	- request forwarding
		- single entry point or user validation 
		
- Scheduler - start app in the worker node
	- where the pod will be scheduled
	- scheduler only decides the node where the app has to be started 
	- it is the function of kubelet to schedule a and start the pod
	
- Controller Manager - detect state changes
	- recover crash state
	- it request scheduler to reschedule the pods
- etcd - Key_Value store
	- every mechanism is stored here as data


MINIKUBE - both master and worker run in the same machine for testing purposes.


## Create a Service

Service
		- internal
		- external

- Basically service is a permanent IP address that can be attached to each pod.
- So that, even if the pod dies the service will not lose its configurations.

By default, the Pod is only accessible by its internal IP address within the Kubernetes cluster. To make the `hello-node` Container accessible from outside the Kubernetes virtual network, you have to expose the Pod as a External Kubernetes Service


pods
replica sets
deployments
services


1. Create a POD
```
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
```

4. View cluster events:
```
kubectl get events
```

5. View the `kubectl` configuration:
```
kubectl config view
```

Expose the Pod to the public internet using the `kubectl expose` command:

```
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

The `--type=LoadBalancer` flag indicates that you want to expose your Service outside of the cluster.

# Crud Commands

Create  deployment 
```
kubectl create deployment [name]
```

Edit  deployment 
```
kubectl edit deployment [name]
```

Delete deployment 
```
kubectl delete deployment [name]
```

# Status of different K8s components

```
kubectl get nodes 
```

```
kubectl get pod -o wide
 ```

```
kubectl get services
```

```
kubectl get replicaset
```

```
kubectl get deployment
```

# Debugging pods

1. Log to Console
```
kubectl logs [pod name]
```

2. Get Interactive Terminal
```
kubectl exec -it [pod name] --bin/bash
```

3. Get info about pod
```
kubectl describe pod [pod name]
```

4. Get info about service
```
kubectl describe service [service name]
```
# Use configuration file for CRUD

1. Apply a configuration file
```
kubectl apply -f [file name]
```

structure of config file 
```
apiVersion: apps/vl
kind: Deployment
metadata :
	name: nginx—deployment
	labels:
		app: nginx
spec:
	replicas: 1
	selector:
		matchLabeIs:
			app: nginx
	template:
		metadata :
			labels:
				app: nginx
		spec:
			containers:
			— name: nginx
			  image: nginx:1.16
			  ports:
			  — containerPort: 80
```

2. Delete with configuration file
```
kubectl delete -f [file name]
 ```

# 3 PARTS of K8s configuration file

1. Metadata 
2. Specification
3. Status - automatically generated
Kubernetes always compares between desired state and actual state and this is the basis of the healing feature. for ex: if your yaml has 2 replica sets then it is the desired state and if your deployment has only one pod running the the actual state does not match with the desired state. Then Kubernetes repairs this issue.

```
kubectl get deployment [deployment name] -o yaml
```



Inside a service if you specify type as a LoadBalancer then it acts as an external service. Although the internal service also is given a tag name of LoadBalancer


note that the node port for exposing the kubernetes service must be inside a range in between 32000 and 32767

to assingn an external ip to the service 
```
minikube service service-name
```


# Namespaces

1. Structure your components
2. Avoid conflicts between teams
3. Share services between different environments
4. Access and Resource Limits on Namespaces Level
5. Each NS must define own ConfigMap and secret

*Components, which can't be created within a Namespace*
- Volumes
- Node

```
kubectl get namespace
```
```
kubectl create namespace my-namespace
```
```
kubectl api-resources --namespaced=false
```
```
kubectl api-resources --namespaced=true
```


