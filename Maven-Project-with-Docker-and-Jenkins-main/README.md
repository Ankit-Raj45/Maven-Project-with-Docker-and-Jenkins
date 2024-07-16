# Maven Project with Docker and Jenkins
 

![11](https://github.com/user-attachments/assets/55bf107e-feba-46a0-84d0-ae1e30c05523)


1. Launch an EC2 instance for Docker host

2. Install docker on EC2 instance and start services 
  ```sh 
  yum install docker
  service docker start
  ```

3. create a new user for Docker management and add him to Docker (default) group
```sh
useradd dockeradmin
passwd dockeradmin
usermod -aG docker dockeradmin
```

4. Write a Docker file under /opt/docker

```sh
mkdir /opt/docker

### vi Dockerfile
# Pull base image 
From tomcat:9-jre9 

# Maintainer
MAINTAINER "DEVOPS with Kamlesh" 

# copy war file on to container 
COPY ./webapp.war /usr/local/tomcat/webapps
```

5. Login to Jenkins console and add Docker server to execute commands from Jenkins  
Manage Jenkins --> Configure system -->  Publish over SSH --> add Docker server and credentials

6. Create Jenkins job 

A) Source Code Management  
 Repository : https://github.com/Ankit-Raj45/Maven-Project-with-Docker-and-Jenkins/tree/main/Maven-Project-with-Docker-and-Jenkins-main
 Branches to build : */main  

B) Build
 Root POM: pom.xml  
 Goals and options : clean install package  
 
C) send files or execute commands over SSH
 Name: docker_host  
 Source files	: `webapp/target/*.war`
 Remove prefix	: `webapp/target`
 Remote directory	: `//opt//docker`  
 Exec command[s]	: 
  ```sh
  docker stop docker_demo;
  docker rm -f docker_demo;
  docker image rm -f docker_demo;
  cd /opt/docker;
  docker build -t docker_demo .
  ```

D) send files or execute commands over SSH  
  Name: `docker_host`  
  Exec command	: `docker run -d --name docker_demo -p 8090:8080 docker_demo`  

7. Login to Docker host and check images and containers. (no images and containers)

8. Execute Jenkins job

9. check images and containers again on Docker host on AWS EC2. This time an image and container get creates through Jenkins job

10. Access web application from browser which is running on container
```
<docker_host_Public_IP>:8081
```
![1](https://github.com/kamleshrawat/Maven-Project-with-Docker-and-Jenkins/assets/108130020/25a27858-fe94-4c34-9299-6335d9636a87)
