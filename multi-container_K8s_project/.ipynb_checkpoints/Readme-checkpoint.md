# Project overview
[This project is a copy of Multi-container-App deployed on AWS](../Multi-container-App-deployed_on_AWS_BS) that will be adapted to work in the K8s.

<img src="../multi-container-app-complex/photos/1.png">  

will be   
<img src="photos/1.png">   
PVC: Persistent Volume Claim
for this project we will not need the following folder: 
- nginx : as the routing will be done Ingress Service with the outside world

also we don't need the following files :

> docker-compose.yml  
> Dockerrun.aws.json  
> .travis.yml  


# Objects Used in this project:
<img src="photos/2.png">

we will use deployments to create the needed pods

## 1. Services used
**Note** a service is needed when we have a request into a set of pods (or a single pod), so for the worker there is no need to have a ClusterIP neither a port setting  
there is no information inside the worker than need to be accessible from anything else inside our cluster
### 1. ClusterIP
the ClusterIP exposes a set of pods to other objects in the cluster  
<img src="photos/3.png">

### 2. Ingress Service


## creating object
> 1. Create a folder K8s where you store all the config yaml files
> 2. open the shell and go to the project directory an type :  
shell > kubectl apply -f K8s

## 2. Databases pods 
### Postgres and necessity of Volumes
<img src="photos/4.png"> <img src="photos/5.png">
the necessity of volumes comes from the fact that when a postgress pod is created, it will contain a postgress container which by itself contain the data stored, so if the container crushes the data will be lost
<img src="photos/7.png"> 
here comes the volumes :  
<img src="photos/6.png">  

when a container crushes depoyment>pod will create a new one that will be connected  to the volume to get the data.  
<img src="photos/8.png">  

# Types Volumes in Kubernetes
<img src="photos/10.png">


## 1. volumes in K8s
<img src="photos/9.png">   
<img src="photos/11.png">
> A volume still at the pod level, so if the pod crushes we lose our data !  

## 2. Persistent volumes
A persistent volume is a long term durable storage, so if a pod crushes we will not lose data  
<img src="photos/12.png">
### Persistent volume claim
is a request for storage by a user. It is similar to a Pod. Pods consume node resources and PVCs consume PV resources. Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., they can be mounted ReadWriteOnce, ReadOnlyMany or ReadWriteMany, [see AccessModes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes))

<img src="photos/13.png">

PVC is like saying to kubernetes : I want a storage that meets the requirment explained in the PVC yaml file (see example below) :  

```YAML
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: database-persistent-volume-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
      # storageClassName needed to be specified if we use a cloud provider
```

<img src="photos/14.png">
<img src="photos/15.png">


### Designating a PVC in a pod template
As described before we want a persistent storage to be dedicated to postgress pod, for this we need to update the yaml config file of the postgress deployment:  

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: postgres
  template:
    metadata:
      labels:
        component: postgres
    spec:
      volumes: # here we specify that we need a persistent storage as described in the claim
        - name: postgres-storage
          persistentVolumeClaim:
            claimName: database-persistent-volume-claim # we give the claim name
      containers:
        - name: postgres
          image: postgres
          ports:
            - containerPort: 5432
          volumeMounts: # describe how the storage will be used inside the container (like docker volume)
            - name: postgres-storage #we specify the volumename that must be used-see volumes section
              mountPath: /var/lib/postgresql/data # here the path inside the container from where we  store the content inside the persistent storage (the data that we want to backup )
              subPath: postgres # the name of the folder inside the persistent storage where this data will be stored
```

for more details please visit : https://kubernetes.io/docs/concepts/storage/persistent-volumes/
