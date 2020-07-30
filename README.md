# Kubernetes

What is kubernetes?
Kubernetes is a container orchestration tool that means it is a tool for the automated management of containerized applications.

Kubeadm- the tool which automates a large portion of the process of setting up a cluster. 
Kubelet- The essential component of Kubernetes that handles running containers on a node.
Kubectl- Command-line tool for interacting with the cluster once it is up. Used to manage the cluster
Orchestration- one of the things we can do is we can  

Nodes and Pods
- Nodes-Kubernetes runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine, depending on the cluster. Each node contains the services necessary to run Pods, managed by the control plane.
- Pods- the smallest and the most basic buiding block of the Kubernetes model. A pod consist of more containers, storage resources, and a unique IP address in the Kubernetes cluster network.

Master is responsible for 

kubeadm join 172.31.29.189:6443 --token 6e7syc.mc5ltw57x66z4rlu --discovery-token-ca-cert-hash sha256:67ea0f26bbf999c08fc41997acb78cb18ca5a7f1ebe2b356c153ea5af6d2fe99 
