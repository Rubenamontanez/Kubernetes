# Kubernetes

What is kubernetes?
Kubernetes is a container orchestration tool that means it is a tool for the automated management of containerized applications.

Kubernetes offers a very rich set of features for container orchestration. Some of its fully supported features are:

- Automatic bin packing
Kubernetes automatically schedules containers based on resource needs and constraints, to maximize utilization without sacrificing availability.
- Self-healing
Kubernetes automatically replaces and reschedules containers from failed nodes. It kills and restarts containers unresponsive to health checks, based on existing rules/policy. It also prevents traffic from being routed to unresponsive containers.
- Horizontal scaling
With Kubernetes applications are scaled manually or automatically based on CPU or custom metrics utilization.
- Service discovery and Load balancing
Containers receive their own IP addresses from Kubernetes, while it assigns a single Domain Name System (DNS) name to a set of containers to aid in load-balancing requests across the containers of the set.

Kubeadm- the tool which automates a large portion of the process of setting up a cluster. 
Kubelet- The essential component of Kubernetes that handles running containers on a node.
Kubectl- Command-line tool for interacting with the cluster once it is up. Used to manage the cluster
Orchestration- one of the things we can do is we can  

Nodes and Pods
- Nodes-Kubernetes runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine, depending on the cluster. Each node contains the services necessary to run Pods, managed by the control plane.
- Pods- the smallest and the most basic buiding block of the Kubernetes model. A pod consist of more containers, storage resources, and a unique IP address in the Kubernetes cluster network.

Master node has three components
- The API Server - allows you to interact with the Kubernetes API. Its the front end control plane. 
- Scheduler - watches created Pods, who do not hae a Node design yet, and designs the pod to run on a specific Node. 
-Controller Manager- runs the controllers. These are background threads that run task in a cluster. Roles include Node Controller, responsible for the worker states. the Repication Controller, responsible for maintaning the correct number of Pods for the replicated controller. End-Point Controller joins the services and Pods together.
-etcd - simple distributed key value stored. Kubernetes uses etcd as its database and stores all cluster data here. 

kubeadm join 172.31.29.189:6443 --token 6e7syc.mc5ltw57x66z4rlu --discovery-token-ca-cert-hash sha256:67ea0f26bbf999c08fc41997acb78cb18ca5a7f1ebe2b356c153ea5af6d2fe99 
