# kubernetes
kubernetes and it's components

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

# Service of Kubernetes:

service of kubernetes is for accessing the application from outside of cluster. Created pods on a node get same IP address and every node has a internal IP address. To access the application on a node have to create service using same label when pod was created with a label.

$ kubectl create -f service-definition.yml

Here service-definition.yml file is defined with apiVersion: v1, kind: Service, metadata and spec section. And also have to mention nodePort, targetPort, port. Nodeport is a range of ports (30000-32767) on a node. Targetport is a port of pod which is mapped to a service port and service port is mapped to a nodeport of a node. 

kubectl get nodes -o yaml

# Init Container:

One or more initilization containers which must run to completion before any application container run. 

# Statefulset:

Manages deployments and scalling of a set of pods, with durable storage and persistent identifier for each pod.

# ServiceAccount with ClusterRole & ClusterRoleBinding: 

kubernetes doesn't allow to create user directly in a cluster like 'kubectl create user user1' To allow any user to perform any task across the cluster we need to create ClusterRole and ClusterRoleBinding. Besically ClusterRole is an object where is in define which tasks can do into which resources for a specific user or serviceaccount. And ClusterRoleBinding is where user or servicesaccount is specified or mentioned into a clusterrolebinding definition file. When pod will be created pod definition file will be written using this clusterrole, clusterrolebinding and serviceaccount. we can get more information about apiGroup and api-resources using followoing command.

$ kubectl api-resources & kubectl api-group

This is an example of clusterrole definition file below.

apiVersion: rbac.authorization.k8s.io/v1

kind: ClusterRole

metadata:

  name: pvviewer-role
  
rules:

- apiGroups: [""]

  resources: ["persistentvolumes"]
  
  verbs: ["list"]
  
Note: Here apiGroup option under rules section is defined as [""] because for persistentvolumes resource which is defined under resources section has 'false' as a apiGroup. we'll get it by using above command. If for any resource having 'true' as an apiGroup then apiGroup option will as apiGroup: "". Please have a below example.

apiVersion: rbac.authorization.k8s.io/v1

kind: ClusterRoleBinding

metadata:

  name: pvviewer-role-binding
  
subjects:

- kind: ServiceAccount

  name: pvviewer
  
  apiGroup: “”
  
  namespace: default
  
roleRef:

  kind: ClusterRole
  
  name: pvviewer-role
  
  apiGroup: rbac.authorization.k8s.io
  
  # Ingress & Ingress controller
  
  Ingress controller must have to remain in cluster becauser of ingress resources which is for traffic handling accross all pods in a specific namespace. Ingress resource and application must have to deploy in same namespace. But ingress controller may have in different namespace. Ingress is besically HTTP load balancing built-in configuration in kubernetes. For accessing into services in kubernetes cluster from outside network must have to deoploy Ingress controller. There are many ingress controller provider like nginx-ingress-controller(https://www.nginx.com/products/nginx/kubernetes-ingress-controller). see kubernetes documentation for more detail about ingress and ingress controller. 
  
  





