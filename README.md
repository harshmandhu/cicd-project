# DevOps Project CICD End-To-End Pipeline With Kubernetes

This is a Django Todo CICD App made in Django framework. It allows users to easily add, delete, and update events and tasks. With this App, users can manage their tasks and events.


## In this project I have created Continuous Integration and Continous Deployment End-To-End Pipeline using Git, Github, Jenkins, Docker, Dockerhub, Kubernetes by Jenkins Declarative Pipeline.




## Steps to follow

1. I created an AWS EC2 instance (Ubuntu + t2-micro) and key pair to securely connect to my instance and enabled SSH, HTTP ports in security group

2. Next I opened a terminal using instance public IP in PuTTY and login as ubuntu 

3. Updated a list of packages in Ubuntu's package manager
```bash
  sudo apt update
```

4. Then I installed Java and checked its version
```bash
  sudo apt install openjdk-11-jre
```
```bash
  java -version
```

5. installed Jenkins 
```bash
 curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

6. Enabled and started the Jenkins
```bash
  sudo systemctl enable jenkins
```
 ```bash
  sudo systemctl start jenkins
```

7. Checked Jenkins status
 ```bash
  sudo systemctl status jenkins
```

8. I know that jenkins opened on port 8080 So I updated the EC2 instance's inbound security added rule customTCP, port 8080 and allow anywhereIpv4

9. Then I opened jenkins with  http://publicIP:8080/

10. (A) In jenkins first of all I created my account and installed necessary jenkins plugins. 
Then in jenkins followed these steps to create a pipeline =  New Item -> type project name (cicd-project) -> pipeline project -> Pipeline (here in definition select Pipeline script from scm) -> SCM(Git) -> Repository URL (Paste the url of github repository) -> credentials -> kind = Username with Password, Scope = Global, username = my github username, password = paste that personal access token, ID = github-token, description = this is for jenkins and github integration. Then add that credentials -> branches to build = */main , Script Path = Jenkinsfile -> apply -> save

  (B1) To generate Personal Access Token in Github, Open your github account -> Settings -> Developer settings -> Peronal access token -> Tokens(classic) -> Generate new token(classic) -> give the permissions -> generate token -> copy that and use that token in jenkins credentials 

  (B2) To push the Docker images into DockerHub, create DockerHub access token and add that token in Jenkins credentials.
  To generate DockerHub Access Token, Open your DockerHub account -> Account Settings -> security -> New access token -> add description and generate token -> copy that token.
  Now open Jenkins -> Manage Jenkins -> Credentials -> global -> add credentials -> kind = Username with Password, Scope = Global, username = my dockerhub username, password = paste that access token, ID = docker-cred, description = this is for jenkins and dockerhub integration
  
11. Then I installed Docker in terminal because I want to build images of my application.
 
```bash
 sudo apt install docker.io
```
12. Then I know that Docker opened on port 8000 So I updated the EC2 instance's inbound security added rule customTCP, port 8000 and allow anywhereIpv4



13. Run Build Now in jenkins 
  
   After this build command, the jenkins pipeline pull that code from github, build a docker image from that code, push that docker image to DockerHub regisry and update the kubernetes manifest repository in github ( https://github.com/harshmandhu/manifest.git ) with latest deployment yaml file 


14. To install Kubernetes Minikube, I created an AWS EC2 instance (Ubuntu + t2-medium) and key pair to securely connect to my instance and enabled SSH, HTTP ports in security group.

15. Next i opened a terminal using instance public ip in PuTTY and login as Ubuntu

16. Updated a list of packages in Ubuntu's package manager
```bash
  sudo apt update
```
17. Then I installed Docker in terminal because minikube run kubernetes cluster in Docker
```bash
 sudo apt install docker.io
```
18. Added current user to docker group (To use docker without root)
```bash
 sudo usermod -aG docker $USER && newgrp docker
```
Exit

19. Install Minikube for linux ubuntu
```bash
 curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
20. Now install kubectl , command line tool of kubernetes
```bash
 sudo snap install kubectl --classic
```
21. Now start the Minikube 
```bash
 minikube start --driver=docker
```
22. check Minikube cluster status 
```bash
 minikube status 
```
23. Now clone those manifest files from the repository 
```bash
 git clone https://github.com/harshmandhu/manifest.git
```
24. Now apply the pod.yaml file to run the pod 
```bash
 kubectl apply -f pod.yaml
```
25. check the pod is running or not 
```bash
 kubectl get pods
```
26. Now apply the deployment file to deploy the application on kubernetes cluster
```bash
 kubectl apply -f deploy.yaml
```
27. Now apply the service file to open the clusters NodePort port for outside connection 
```bash
 kubectl apply -f service.yaml
```
28. you can all these pods deployments and service pods  
```bash
 kubectl get all
```
29. you can check the minikubeip and connect the cluster from inside using this ip and port
```bash
 minikube ip
```


![a](https://github.com/harshmandhu/cicd-project/assets/95110861/ca925ec5-e027-41c7-b716-85f3b15611e1)


![b](https://github.com/harshmandhu/cicd-project/assets/95110861/a7c403f5-7a5d-46a1-92bb-2993e465381c)


![c](https://github.com/harshmandhu/cicd-project/assets/95110861/90c8055d-55f4-4d7d-b25b-aefa3cf66f65)


![d](https://github.com/harshmandhu/cicd-project/assets/95110861/9bbb7247-239a-410e-aa1c-406d5e61e809)


![e](https://github.com/harshmandhu/cicd-project/assets/95110861/0372fb04-d609-4d63-ac8f-efdda2e54e4c)


![f](https://github.com/harshmandhu/cicd-project/assets/95110861/4ec2a59d-3421-4885-bead-0bd71326a287)


![g](https://github.com/harshmandhu/cicd-project/assets/95110861/29277644-1908-4ecf-a84b-c4e6b04465b8)


