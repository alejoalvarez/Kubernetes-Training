# Kubernetes

<p>
<img height="60" src="https://user-images.githubusercontent.com/13514156/120713690-e108a000-c487-11eb-9c3f-fce7df817602.png">
</p>

## Generalities

- Started at google with **Borg** and **Omega** 
- Kubernetes shares same DNA og Borg and Omega and its **Open Source**
- Written in Golang
- Better know as K8s
- System that allows to carry out applications to the cloud in a simple way, allowing a faster and more efficient administration of the architecture
- Belongs to CNF (Cloud native Compunting Foundation)
- It Allows to orchestrate the containers
  ○ Deploy and manage applications (containers)
  ○ Scale up and down according demand
  ○ Zero downtime Deployment
  ○ Rollbacks
  ○ And more

## Competitors  K8s
- RedHat OpenShift 
- Docker Swarm 
- Amazon Elastic Container Service for Kubernetes 
- IBM Cloud Kubernetes Service 
- Azure Kubernetes Services AKS 
- Marathon 
- Mesosphere 

## Architecture

<p align="center">
<img height="700" src="https://user-images.githubusercontent.com/13514156/120700328-240e4780-c477-11eb-8127-51a7c9365d47.png">
</p>

### Cluster

- Cluster is a set of nodes
- Node - Virtual VM or Physical Machine!
- **Master node** and N **workers node**

### Master node (control plane)
- Is simply the brains of the cluster so this is where all decisions all are made
- is responsible for scheduling the work through the workers nodes
- Master runs all cluster's control plane services

<p align="center">
<img height="500" src="https://user-images.githubusercontent.com/13514156/120702932-4a81b200-c47a-11eb-8f26-8f8d3e971ad1.png">
</p>

#### ```Apiserver```

- Frontend to kubernetes Control Pane (Makes available the kubernetes API that is used to operate the kubernetes environment)
- All communications go through API server External and internal
- Exposes Restful API on port 443
- Authentication and Authorization checks
- Allows use kubectl apply -f FILE.YAML 

<p align="center">
<img height="200" src="https://user-images.githubusercontent.com/13514156/120706207-6d15ca00-c47e-11eb-85fe-6e1ae9b29496.png">
</p>

#### ```schedule```

- Watchers for new workloads/pods and assigns then to a node based on several scheduling factors
- Is the node healthy?
- Does it has enough resources?
- Is the port available?
- It is responsible for selecting the nodes in which the pods should be created

#### ```controller-manager```

- Provides various high-level abstractions to support or support pod replicas
- Daemon that manages the control loop Controller of Controllers
  - Node control
  - If for some reason a node falls, this component is responsible for uploading it again
- Other controllers
  - ReplicaSet
  - Endpoint
  - Namespace
  - Service Accounts
- Each controller watches the API Server for changes

#### ```etcd```
- Highly available database
- It is a distributed key-value component and is the primary communication component used by the master and worker nodes
- The status of critical information in your kubernetes environment is stored and replicated
- The performance and scalability characteristics of kubernetes depend on this mechanism etcd to be highly efficient
- Stores configuration and state of the entire cluster
- Single source of truth

<p align="center">
<img height="200" src="https://user-images.githubusercontent.com/13514156/120705943-0e505080-c47e-11eb-97fe-150237672d94.png">
</p>

#### ```Cloud control manager```

Responsible to intetact with underlying cloud provider (AWS, Google Cloud, Azure etc)

<p align="center">
<img height="450" src="https://user-images.githubusercontent.com/13514156/120706330-8e76b600-c47e-11eb-9d38-9ab891c2efb2.png">
</p>


#### ```kubectl```
- Kubernetes Command Line Toolds
- Run Commands gains your cluster
	- Deploy
	- Inspect
	- Edit resources
	- Debug
	-  View logs

#### ```Pod```
Smallest drop down unit that can be created
- It can contain 1 or more containers
- Application containers that are in the same pod have the following benefits:
	-  Share an IP address and port space
	-  They share the same host name
	-  Can communicate with each other through native interprocess communication (IPC)



### Worker Node 

- Machine within cluster (VM or physical Machine running often linux, usually in the microservices world they are EC2 instances on AWS)
- Privides running environment for you applications
- It has tools to be able to deploy applications
- Communicates with master node
- Runs Pods 

<p align="center">
<img height="500" src="https://user-images.githubusercontent.com/13514156/120708362-165dbf80-c481-11eb-858b-62cdd647ef4c.png">
</p>

Workers worker nodes are responsible for running the pods that are programmed in them. The main kubernetes components that exist on worker nodes are:
- kubelet
- kube-proxy
- container runtime

#### ```Kubelet```
- Main Agent that runs on every node
- Responsible for making sure the containers in each pod are up and running
- Receives pod definitions from API server
- Interacts with container runtime to run containers associated with the Pod
- Kubernetes users interact with the master node using the kubectl CLI
- Reports node and pod state to master
- The kubelet will restart the containers upon acknowledging that they have been unexpectedly terminated
	
#### ````Kube-proxy````
- One of the key strengths of kubernetes is the network support it provides for containers, kube-proxy provides network support in the form of connection forwarding, load balancing, and assigning a single IP address to a Pod
- Agent runs on everyone through DaemonSets
- Responsible for 
  - Local cluster networking 
  - Each node gets own unique IP address
  - Routing network traffic to load balanced services
	
#### ```Container runtime```
- Kubernetes supports several container runtime environment options, including Docker, rk, and containerd
- Is responsible for pulling images from container registries, such as dockerhub, GCR (Google), ECR (AWS), ACR (Azure)
- Responible for Running Containers and abstracts container management for kubernetes
- CRI - Container Runtime Interface 
  - Interface for 3rd party container runtime









