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


Labels 
- Key-value pairs attached to Kubernetes objects (e.g. Pods, ReplicaSets). 
- Used to organize and select a subset of objects, based on the requirements in place. Many objects can have the same Label(s). 
- Do not provide uniqueness to objects. 
- Controllers use Labels to logically group together decoupled objects, rather than using objects' names or IDs.

ReplicaSets
-  ReplicaSets support both equality- and set-based selectors. We can scale the number of Pods running a specific container application image. Scaling can be accomplished manually or through the use of an autoscaler.
- The ReplicaSet will detect that the current state is no longer matching the desired state. The ReplicaSet will create an additional Pod, thus ensuring that the current state matches the desired state.
- ReplicaSets can be used independently as Pod controllers but they only offer a limited set of features. A set of complementary features are provided by Deployments, the recommended controllers for the orchestration of Pods. Deployments manage the creation, deletion, and updates of Pods. A Deployment automatically creates a ReplicaSet, which then creates a Pod. There is no need to manage ReplicaSets and Pods separately, the Deployment will manage them on our behalf.

Deployments: represent a set of multiple, identical Pods with no unique identities. A Deployment runs multiple replicas of your application and automatically replaces any instances that fail or become unresponsive. ... Deployments are managed by the Kubernetes Deployment controller.
- Deployment objects provide declarative updates to Pods and ReplicaSets. The DeploymentController is part of the master node's controller manager, and it ensures that the current state always matches the desired state. It allows for seamless application updates and downgrades through rollouts and rollbacks, and it directly manages its ReplicaSets for application scaling. 
- In Deployment, we change the Pods' Template and we update the container image from nginx:1.7.9 to nginx:1.9.1. The Deployment triggers a new ReplicaSet B for the new container image versioned 1.9.1 and this association represents a new recorded state of the Deployment, Revision 2. The seamless transition between the two ReplicaSets, from ReplicaSet A with 3 Pods versioned 1.7.9 to the new ReplicaSet B with 3 new Pods versioned 1.9.1, or from Revision 1 to Revision 2, is a Deployment rolling update. A rolling update is triggered when we update the Pods Template for a deployment. Operations like scaling or labeling the deployment do not trigger a rolling update, thus do not change the Revision number. Once the rolling update has completed, the Deployment will show both ReplicaSets A and B, where A is scaled to 0 (zero) Pods, and B is scaled to 3 Pods. This is how the Deployment records its prior state configuration settings, as Revisions. 
- Once ReplicaSet B and its 3 Pods versioned 1.9.1 are ready, the Deployment starts actively managing them. However, the Deployment keeps its prior configuration states saved as Revisions which play a key factor in the rollback capability of the Deployment - returning to a prior known configuration state. In our example, if the performance of the new nginx:1.9.1 is not satisfactory, the Deployment can be rolled back to a prior Revision, in this case from Revision 2 back to Revision 1 running nginx:1.7.9. 

Namespaces: Namespaces are a way to divide cluster resources between multiple users (via resource quota). In future versions of Kubernetes, objects in the same namespace will have the same access control policies by default.
- Generally, Kubernetes creates four default Namespaces: kube-system, kube-public, kube-node-lease, and default. The kube-system Namespace contains the objects created by the Kubernetes system, mostly the control plane agents. The default Namespace contains the objects and resources created by administrators and developers. By default, we connect to the default Namespace. kube-public is a special Namespace, which is unsecured and readable by anyone, used for special purposes such as exposing public (non-sensitive) information about the cluster. The newest Namespace is kube-node-lease which holds node lease objects used for node heartbeat data. Good practice, however, is to create more Namespaces to virtualize the cluster for users and developer teams.

Authentication, Authorization, and Admission Control: To access and manage any Kubernetes resource or object in the cluster, we need to access a specific API endpoint on the API server. Each access request goes through the following three stages:
Authentication: Logs in a user. Kubernetes does not have an object called user, nor does it store usernames or other related details in its object store. However, even without that, Kubernetes can use usernames for access control and request logging
- Two kinds of users: Normal Users-They are managed outside of the Kubernetes cluster via independent services like User/Client Certificates, a file listing usernames/passwords, Google accounts, etc. Service Accounts- With Service Account users, in-cluster processes communicate with the API server to perform different operations. Most of the Service Account users are created automatically via the API server, but they can also be created manually. The Service Account users are tied to a given Namespace and mount the respective credentials to communicate with the API server as Secrets.
For authentication, Kubernetes uses different authentication modules:
Client Certificates
To enable client certificate authentication, we need to reference a file containing one or more certificate authorities by passing the --client-ca-file=SOMEFILE option to the API server. The certificate authorities mentioned in the file would validate the client certificates presented to the API server. A demonstration video covering this topic is also available at the end of this chapter.
Static Token File
We can pass a file containing pre-defined bearer tokens with the --token-auth-file=SOMEFILE option to the API server. Currently, these tokens would last indefinitely, and they cannot be changed without restarting the API server.
Bootstrap Tokens
This feature is currently in beta status and is mostly used for bootstrapping a new Kubernetes cluster.
Static Password File
It is similar to Static Token File. We can pass a file containing basic authentication details with the --basic-auth-file=SOMEFILE option. These credentials would last indefinitely, and passwords cannot be changed without restarting the API server.
Service Account Tokens
This is an automatically enabled authenticator that uses signed bearer tokens to verify the requests. These tokens get attached to Pods using the ServiceAccount Admission Controller, which allows in-cluster processes to talk to the API server.
OpenID Connect Tokens
OpenID Connect helps us connect with OAuth2 providers, such as Azure Active Directory, Salesforce, Google, etc., to offload the authentication to external services.
Webhook Token Authentication
With Webhook-based authentication, verification of bearer tokens can be offloaded to a remote service.
Authenticating Proxy
If we want to program additional authentication logic, we can use an authenticating proxy.
Authorization: Authorizes the API requests added by the logged-in user.

Admission Control: Software modules that can modify or reject the requests based on some additional checks, like a pre-set Quota.

 

