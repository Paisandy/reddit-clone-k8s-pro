# Reddit-Application-Assignment using Kubernetes(k8s)
## Requriements:-
### 1. AWS ec2 instance t2 micro
### 2. Docker Hub Profile Create it
### 3. GitHub
### 4. Follow my steps!!

## Setup CI Server
### Launch an AWS ec2 instance with Ubuntu-t2.micro AMI type
```
sudo apt-get update
sudo apt-get install docker.to -y
sudo usermod -aG docker $USER && newgrp docker
```
### After that do docker ps
```
docker  ps
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/6f2035cc-6b45-4542-9022-f92e4ac0ec02)
### Clone the project using Github
```
mkdir project
cd project/
git clone https://github.com/Paisandy/reddit-clone-k8s-pro.git
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/d58acbb9-46e3-4878-9d7f-a46a9ea9ef7f)
### do cat Dockerfile
```
cat Dockerfile
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/1912a0da-81a8-42f3-b825-676e430746dd)
### build the docker image
```
docker build . -t sandypaia320/reddit-k8s:latest
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/2f15c5d2-8adf-4272-be63-5b22669011f3)
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/36457c00-8c77-484d-9fd5-85be6fffa5dc)

#### sandypaia320 ---> DockerHub user name.. So change it.!
#### reddit-k8s:latest ---> Docker child image

### Push to Dockerhub
```
docker login
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/7c200215-b76f-4d07-a0e9-32ff825e8765)
```
docker push sandypaia320/reddit-k8s:latest
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/5c4f0c59-2192-4059-8683-534545c3f761)
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/9ff6f0ff-8baf-4648-8f1c-dc1776d8d6f9)

## Setup Deployment
```
sudo apt-get update
sudo apt-get install docker.io -y
sudo usermod -aG docker $USER && newgrp docker
```
## Installation of Minikube & Kubectl
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
sudo snap install kubectl --classic
minikube start --driver=docker
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/92399583-ff6c-4989-b7f6-e7349d4c1ef0)
### Create namespace in a project folder
```
kubectl create namespace reddit-ns
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/e18f36ac-58e7-43e3-83c1-e052a1ca0c05)
### Check the manifest file using 'cat' command
```
cat deployment.yml
```
### deployment.yml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reddit-clone-deployment
  labels:
    app: reddit-clone
spec:
  replicas: 2
  selector:
    matchLabels:
      app: reddit-clone
  template:
    metadata:
      labels:
        app: reddit-clone
    spec:
      containers:
      - name: reddit-clone
        image: sandypaia320/reddit-k8s:latest
        ports:
        - containerPort: 3000
```
### Run the deployment.yml file
```
kubectl apply -f deployment.yml -n reddit-ns
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/672cce82-2540-4a6d-ac2a-f223e35bd596)

### after saving it.. Check service.yml file using 'cat' command
```
cat service.yml
```
### Run the service.yml file
```
apiVersion: v1
# Indicates this as a service
kind: Service
metadata:
  # Service name
  name: reddit-clone-service
spec:
  selector:
    # Selector for Pods
    app: reddit-clone
  ports:
    # Port Map
  - port: 3000
    targetPort: 3000
    protocol: TCP
  type: LoadBalancer
```
### Run it
```
kubectl apply -f service.yml -n reddit-ns
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/b1fed116-9987-45aa-9f68-d86a758c9af4)

## To get URL for your app:
```
minikube service reddit-clone-service -n reddit-ns created --url
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/6b04167b-55f3-4bdd-8f45-9cf97f8cbb30)
### make sure that add this port 3000 & last port number which appears for ex:- 192.168.49.2:30808 so use 30808 add on the AWS Security group

## Expose the App Deployment:
```
kubectl expose deployment reddit-clone-deployment -n reddit-ns --type=NodePort
```
## Expose the APP Service:
```
kubectl port-forward svc/reddit-clone-service -n reddit-ns 3000:3000 --address 0.0.0.0
```
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/10678c63-f55c-44be-b588-f9840559796d)
![image](https://github.com/Paisandy/reddit-clone-k8s-pro/assets/115485972/fcc7d13d-5ca6-41c7-aaf2-83caa56c58c0)

### Check ingress.yml file
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-reddit-app
spec:
  rules:
  - host: "domain.com"
    http:
      paths:
      - pathType: Prefix
        path: "/test"
        backend:
          service:
            name: reddit-clone-service
            port:
              number: 3000
  - host: "*.domain.com"
    http:
      paths:
      - pathType: Prefix
        path: "/test"
        backend:
          service:
            name: reddit-clone-service
            port:
              number: 3000
```

## Enable minikube ingress;
```
minikube addons enable ingress
```
### Check the current setting of addons in Minikube;
```
minikube addons list
```
### Run ingress file:
```
kubectl apply -f ingress.yml -n reddit-ns
```
### Check the created ingress file:
```
kubectl get ingress
```

