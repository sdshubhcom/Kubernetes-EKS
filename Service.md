# Service: Nodeport. 

```
kubectl apply -f https://raw.githubusercontent.com/cloudnloud/Kubernetes_Admin_Training/main/class6-service/service/nodeport-facebook.yaml
```

```
kubectl describe pod/webserver -n facebook
```

```
kubectl get all -n facebook -o wide
```

 - now access master,node1,node2 public ip and check from outside browser

<Master Node Public IP>:32001
<Worker Node1 Public IP>:32001
<Worker Node2 Public IP>:32001

```
kubectl delete -f https://raw.githubusercontent.com/cloudnloud/Kubernetes_Admin_Training/main/class6-service/service/nodeport-facebook.yaml
```
# Load Balancer

- For this load balancer exercise
- Create EKS Cluster.  
```
kubectl create ns facebook
```
```  
kubectl apply -f https://raw.githubusercontent.com/cloudnloud/Kubernetes_Admin_Training/main/class6-service/service/loadbalancer-facebook.yaml
```
```  
kubectl get all -n facebook -o wide
```
```  
kubectl get pod,services -n facebook -o wide
```  
- 35.227.119.203:32001
- access like above from all node machine ip addresses.it should work from all client machine ip address.then the proxy cluster is working good.
```
kubectl get services
```
```  
kubectl describe services
```
# Ingress
  
# Introduction

Kubernetes Ingresses offer you a flexible way of routing traffic from beyond your cluster to internal Kubernetes Services. 
Ingress Resources are objects in Kubernetes that define rules for routing HTTP and HTTPS traffic to Services. 
For these to work, an Ingress Controller must be present; its role is to implement the rules by accepting traffic 
(most likely via a Load Balancer) and routing it to the appropriate Services. 

# Prerequisite
- Kubernetes Cluster up and running with master and node or GKE or EKS or any other type of k8s setup.
- Here we are using GCP as a cloud provider for LoadBalancer Service. In case if you do not want to use LoadBalancer you can skip the steps.


# Deploy hello-kubernetes-first.yaml
```
kubectl apply -f https://raw.githubusercontent.com/cloudnloud/Kubernetes_Admin_Training/main/class14-ingress/hello-kubernetes-first.yaml
```
```  
kubectl get all -o wide
```
- To verify the Service’s creation, run the following command:
```
kubectl get service hello-kubernetes-first
```
- You’ll find that the newly created Service has a ClusterIP assigned, which means that it is working properly. All traffic sent to it will be forwarded to the selected Deployment on port 8080. Now that you have deployed the first variant of the hello-kubernetes app, you’ll work on the second one.


# Deploy hello-kubernetes-second.yaml
```
kubectl apply -f https://raw.githubusercontent.com/cloudnloud/Kubernetes_Admin_Training/main/class14-ingress/hello-kubernetes-second.yaml
```
```  
kubectl get all -o wide
```
- To verify the Service’s creation, run the following command:
```
kubectl get service hello-kubernetes-second
```

# Installing the Kubernetes Nginx Ingress Controller
 
- To install the Nginx Ingress Controller to your cluster, you’ll first need to add its repository to Helm by running:
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```
- Update to let Helm know what it contains:
```
helm repo update
```
- Finally, run the following command to install the Nginx ingress:
```
helm install nginx-ingress ingress-nginx/ingress-nginx --set controller.publishService.enabled=true
```
- This command installs the Nginx Ingress Controller from the stable charts repository, names the Helm release nginx-ingress, and sets the publishService parameter to true.

- You can watch the Load Balancer become available by running:
```
kubectl --namespace default get services -o wide -w nginx-ingress-ingress-nginx-controller
```

# Exposing the App Using an Ingress
```
kubectl apply -f https://raw.githubusercontent.com/cloudnloud/Kubernetes_Admin_Training/main/class14-ingress/hello-kubernetes-ingress.yaml
```
- You define an Ingress Resource with the name hello-kubernetes-ingress. Then, you specify two host rules, so that hw1.your_domain is routed to the hello-kubernetes-first Service, and hw2.your_domain is routed to the Service from the second deployment (hello-kubernetes-second).
```
kubectl get all -o wide
```
34.69.167.205	www.example.com
34.69.167.205	hw1.example.com
34.69.167.205	hw2.example.com

access in broswer now you can easily understand ingress
