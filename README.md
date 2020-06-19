# kubernetes
kubernetes and its components

Pod: Smallest unit of kubernetes and a single instance or application run in a single pod in a node. multiple pods can run in a node but multiple same applications can't do run in a single pod. 

kubectl: kubectl is a client object of kubernetes communicate to kuster and get valueable information for pods, replicationcontroller, deployment and so on. 

Minikube: Minikube is single node cluster for testing purposes making local cluster on personal computer. Installing minikube following by official sites and run the follwoing command.

$ minikube start --force --driver=virtualbox 

This above command will download minikube vm and create a VM on your computer. To check status for minikube

$ minikube status 

# output: 

m01

host: Running

kubelet: Running

apiserver: Running

kubeconfig: Configured

# POD with YML

$ kubectl run mynginx --image=nginx [This will create a pod with latest version of nginx]

$ kubectl get pods [To see available pods on node]

But with YML file we can create a pod or multiple pods at a time through replicationcontroller or replicaset and if we want ot add additional information like labels name, metadata, replication number i.e replicaset, matchlabels, selectors an so on. 

# Scalling application up or down for load sharing

In this context replicationcontroller or replicaset comes on it. If you have a single node with a sinlge pod along with a single instance or application but increasing users to access that application are facing problem or slower performance you need to increase pod with that instance or application on that same node or different node.

# Deployment: 

Deployment is an object of kubernetes is used for creating multiple pods with an application using replicationcontroller or replicaset and can do update with version of application when newer version comes on. Rollout and undo activities of applications can perform with deployent object. Rolling update is a type of application update among running applications. It will up newer version and down older version one by one. If any wrong is happend can undo to previous state. 

# Kubernetes Networking:

Minikube is single node cluster and while set up on my laptop will get an IP as like virtualbox setup. And each pod created on a node get an internal privage network assigned by kubernetes like 10.244.0.0 series. Multiple pods will get individual IP with this series and accessible among all pods on same node. But when on different node, pods are not able to communicate each others. There have many network solutioins like cisco, calico, vmware NSX-T, flannel and so on which will provide different internal private network block to each node and can able to communicate with each pod among all nodes. 
