# Welcome to Kubernetes !

## 1. Why Kubernetes ?
the problem is all about scaling our application, let's see the example of AWS Beanstalk when we start to hit a lot users in the same time on our application, the AWS BS scale horizontally by creating a copy of the whole architecture as showed below :  
<img src="/photos/12.png">  
app the scaled :  
<img src="/photos/13.png">

> In this example we want only to scale the worker app (container)! here it comes Kubernetes !!!

with Kubernetes the approach is as follow:  
<img src="/photos/14.png">

## 2. What is Kubernetes
<img src="/photos/15.png">  
Figure : Kubernetes Cluster

a cluster in the world of kubernetes is an assembly of **Master** and **one or more nodes**.  
A node is Virtual Machine VM or phyisical compute rthat will be used to run some number of different containers.  
> Remark:each node can run different number of containers!  
> **MASTER** controls what each node is running at any given time (as a developper we interact with the master)  
> **Load Balancer** is **OUTSIDE** of the Kubernetes cluster, it takes some amount of outside traffic in the form of network request and relay all thoses requests into each of our different nodes  

## 3. How nodes work ?

## 4. Kubernetes in development and production
> in development we use both Minikube and Kubectl as shown below :  
<img src="/photos/16.png">

> In production (see table below):  
<img src="/photos/17.png">

## 5. Analogy between Docker and Kubernetes
<img src="/photos/18.png">

## 6. Hands on - Making a simple project - one node one container
**Prequesties**:  
> Docke image hosted in docker hub  
> Make config file(yaml) to create the container (object)  
> Make config file(yaml) to set up networking (object)  

**what is an object:**
<img src="/photos/19.png">  

<img src="/photos/21.png">  

**POD**: the philosophy of the pod is a grouping of containers with similar purpose, meaning containers with tight coupled integration (if one container is gone the other is worthless) see example below:    
<img src="/photos/20.png">

**Service**: there are 4 types of services :  
<img src="/photos/22.png">  
the nodePort role is to expose the container to the outside world, but in between we have the kube proxy as shown below :  
<img src="/photos/23.png">  

for this project we created 2 config files (one Pod - one Service)  
**client.pod.yaml**
```YAML
apiVersion: v1 # each version give access to a set of object we can use
kind: Pod # defining the object type Pod used to run a container
metadata:
  name: client-pod # name of the pod created
  labels:
    component: web # we could change component by another name we are free
spec:
  containers: # list of the containers
    - name: client # we give a name
      image: goahead19/multi-client-09-08 # image from the docker hub
      ports: #
        - containerPort: 3000 # from this container Expose port 3000 to the outside world
```
**client-node-port.yaml**
```YAML
apiVersion: v1 # each version give access to a set of object we can use
kind: Service # defining the object type Service used to set up networking
metadata:
  name: client-node-port
spec:
  type: NodePort
  ports: # an array
    - port: 3050 # used by other pods to get access to the targetport
      targetPort: 3000 # the container we want to reach
      NodePort: 31515 # port in the outside world (if not specified random assign btw 30000-32767)) used in our browser
  selector:
    component: web # we could change component by another name we are free
```

Explanation here below :  
<img src="/photos/24.png">  

more details  

<img src="/photos/25.png"> 

Start Kuberntes cluster with config: 
<img src="/photos/26.png">  
> kubectl apply -f client.pod.yaml  
> kubectl apply -f client-node-port.yaml  

> when we run this commands this file is taken and passed to the master  

**checking the pods and services command and deployments**

> kubectl get pods  

NAME         READY   STATUS    RESTARTS   AGE  
client-pod   1/1     Running   0          4m49s  

> kubectl get services  

NAME               TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE  
client-node-port   NodePort    10.100.71.34     none       3050:31515/TCP   2m59s  
kubernetes         ClusterIP   10.96.0.1        none        443/TCP          4h32m  

> kubectl get deployments

**deleting a pod**
> kubectl delete - f filepath  
<img src="/photos/36.png"> 

**to get access to your app**  
if you are using docker/kubernetes : localhost:port
if you are using Minikube :  
- you type in the shell minikube ip (to get the ip of your node VM)
- in the browser you type ip:port

## 7. Deployment workflow

<img src="/photos/27.png">  
Note: in the master there are 3 or 4 programs that controls the Kubernetes cluster, here only one represented
<img src="/photos/28.png">  

## 8. Imperative VS Declarative approach
<img src="/photos/29.png">  

**the declarative approach is the one used in the real production deployment**  

<img src="/photos/30.png">  


## 9. Updating a Kubernetes cluster
<img src="/photos/31.png">  
when we change the yaml file you run the command:  
> kubectl apply -f file.yaml  

if you want to check the details of an object you type :  
<img src="/photos/32.png">  
kubectl describe pod client-pod

### Limitation in the updating a Pod
the updating process is limited because we cannot change everything (containerPort for example)  

kubectl apply -f client.pod.yaml  
pod updates may not change fields other than `spec.containers[*].image`, `spec.initContainers[*].image`, `spec.activeDeadlineSeconds` or `spec.tolerations` (only additions to existing tolerations)

> So avoid this issue, we will introduce a new Object called Deployment  
<img src="/photos/33.png">  
<img src="/photos/34.png">  

the deployment runs a set of identical pods (on or more), every pod will have the exact same set of containers with identical configuration  
Deployment use what is called a Pod template to create pods as shown below :  
<img src="/photos/35.png">

When updating the pod created is not possible, Deployment will kill the Pod and create a new one.

## Why use Service ? 
Service alow to route traffic the right pod using the component selector, this layer is very important as the pod IP may change for whatever the cause (re-run, shutdown, error)  
> kubectl get pods -o wide  

NAME                   |              READY  | STATUS  |  RESTARTS  | AGE  |   IP   |      NODE             NOMINATED NODE  | READINESS GATES|  
client-deployment-5dfb6bf966-ctwgd |  1/1  |   Running |  0   |       7m45s |  10.1.0.7  | docker-desktop  | <none>     |      <none>|  

<img src="/photos/37.png">