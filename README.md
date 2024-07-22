## Project Overview ##

This project demonstrates how to create a Deployment in Kubernetes and authorize it to access secrets from the `debezium-example` namespace where Debezium resides. 
It prints the secret for each pod running in that namespace. This project is intended for educational purposes to explore the functionality of the Kubernetes library in Python.

## Run the deployment on minikube ##

Follow these steps:

1. Start minikube:
```
minikube start
```

2. Configure your shell to use Minikube's Docker daemon. 
Minikube has it own Docker daemon. To build Docker images directly within Minikube, you need to point your shell to 
Minikube's Docker daemon.
Check the Docker environment for Minikube:
```
minikube docker-env
```
Point your shell to minikube's docker-daemon, run:
```
eval $(minikube docker-env)
```
To make sure that your Docker CLI is now configured to use the Docker daemon inside your Minikube environment, run:
```
docker info | grep -i "name:"
```
If the commands runs okay, it confirms that Docker daemon is running inside the Minikube VM.



3. Build your Docker image
Navigate go to the directory containing your Dockerfile and build your Docker image. using the provided Dockerfile:
```
docker build -t gprocida6g/print-secrets:1.0 .
```


## Create a Role Resource ##

Use an imperative command:

```
kubectl create role pod-listing-role \
  --verb=get,list \
  --resource=pods,secrets \
  --namespace=kafka \
  --dry-run=client -o yaml > my-role.yaml

```

Or just use the provided `my-role.yaml` file 

The Role defined in the `my-role.yaml` file grants permissions to perform get and list operations on the pods and secrets resources within the `debezium-example` namespace. 
This means any ServiceAccount, User, or Group that this Role is bound to can view and list the pods and secrets in that namespace. Apply the role:
```
kubectl apply -f my-role.yaml
```


## Create a rolebinding resource that binds the pod-listing-role to the default ServiceAccount

Use an imperative command:

```
kubectl create rolebinding pod-listing-binding \
  --role=pod-listing-role \
  --serviceaccount=kafka:default \
  --dry-run=client \
  --namespace=kafka \
  -o yaml > my-role-binding.yaml

```

or just use the provided `my-role-binding.yaml` file. 



The RoleBinding defined in the file `my-role-binding.yaml` grants the permissions defined in the `pod-listing-role` Role to the default ServiceAccount in the debezium-example namespace.
This means that the default ServiceAccount in the debezium-example namespace will have the permissions to get and list pods and secrets within that namespace. Apply the role:
```
kubectl apply -f my-role-binding.yaml
```



## Create the deployment

kubectl create deploy print-secrets \
  --image=gprocida6g/print-secrets:1.0 \
  --namespace=debezium-example\
  --dry-run=client -o yaml > print-secrets.yaml

# Modify the pod configuration by adding

imagePullPolicy: Never 

within the 'containers' field










# Apply all the newly created resources.





You could push the image to your Docker Hub or use the field imagePullPolicy: Never (image 
already present locally).


# Create a role resource named 'pod-listing-role' with specific permission

kubectl create role pod-listing-role \
  --verb=get,list \
  --resource=pods,secrets \
  --namespace=kafka \
  --dry-run=client -o yaml > my-role.yaml


## Create a rolebinding resource that binds the pod-listing-role to the default ServiceAccount

kubectl create rolebinding pod-listing-binding \
  --role=pod-listing-role \
  --serviceaccount=kafka:default \
  --dry-run=client \
  --namespace=kafka \
  -o yaml > my-role-binding.yaml


## Create a pod using the image giprocida/axual-debug:1.0 

kubectl run debug-pod \
  --image=giprocida/axual-debug:1.0 \
  --namespace=kafka \
  --dry-run=client -o yaml -- sleep 4000 > debug-pod.yaml

## Modify the pod configuration by adding

imagePullPolicy: Never 

within the 'containers' field

Apply all the newly created resources.


The previously described procedure is automated through the use of the scripts: create-resources.sh and run-debug.sh.

Run the script create-resource.sh to create all the necessary resources.
Run the script run-debug.sh to apply all the newly created resources.



## How to run it ##

Follow these steps:


## Build the docker image



You could push the image to your Docker Hub or use the field imagePullPolicy: Never (image 
already present locally).


# Create a role resource named 'pod-listing-role' with specific permission

kubectl create role pod-listing-role \
  --verb=get,list \
  --resource=pods,secrets \
  --namespace=kafka \
  --dry-run=client -o yaml > my-role.yaml


## Create a rolebinding resource that binds the pod-listing-role to the default ServiceAccount

kubectl create rolebinding pod-listing-binding \
  --role=pod-listing-role \
  --serviceaccount=kafka:default \
  --dry-run=client \
  --namespace=kafka \
  -o yaml > my-role-binding.yaml


## Create a pod using the image giprocida/axual-debug:1.0 

kubectl run debug-pod \
  --image=giprocida/axual-debug:1.0 \
  --namespace=kafka \
  --dry-run=client -o yaml -- sleep 4000 > debug-pod.yaml

## Modify the pod configuration by adding

imagePullPolicy: Never 

within the 'containers' field

Apply all the newly created resources.


The previously described procedure is automated through the use of the scripts: create-resources.sh and run-debug.sh.

Run the script create-resource.sh to create all the necessary resources.
Run the script run-debug.sh to apply all the newly created resources.

