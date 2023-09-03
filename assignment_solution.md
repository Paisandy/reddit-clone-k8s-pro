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

