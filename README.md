# Testing-Jenkins
## what is jenkins?
Jenkins is an open-source automation server used to automate building, testing, and deploying
software. It helps DevOps teams manage CI/CD pipelines efficiently.

## why we need jenkins?
Jenkins is used to automate repetitive tasks in the software development lifecycle, such as 
code
integration, testing, and deployment. It helps detect bugs early, speed up delivery, and ensures
consistent builds.

# Installation Process :
## first we need  java so install java.
```
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
openjdk version "21.0.3" 2024-04-16
OpenJDK Runtime Environment (build 21.0.3+11-Debian-2)
OpenJDK 64-Bit Server VM (build 21.0.3+11-Debian-2, mixed mode, sharing)

```
# installation of jenkins :
```
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

```
## enabling  jenkins:
```
sudo systemctl enable jenkins

```
## start jenkins:
```
sudo systemctl start jenkins
```
## status of jenkins
```
sudo systemctl status jenkins
```
## Allow port 8080 in security groups from ec2-instance 
![Screenshot from 2025-06-20 17-28-03](https://github.com/user-attachments/assets/baf69fb2-bd7b-436f-9606-8c326e8dbf02)


## Unlock jenkins 
 open browser and paste the link : http://localhost:8080 please  provide your PUBLIC_IP in place of localhost.
if your a first accessjenkins controller you will output like this :
![Screenshot from 2025-06-20 17-40-16](https://github.com/user-attachments/assets/9c4e6169-07c2-4e8d-92d9-786b9040949f)

 
 ## Initial Admin Password
 if you run this command you will get password in your ec2-instance terminal 
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
copy the password and paste it your Administrator Password.then click on continue....

## Install Suggested Plugins
Choose 'Install suggested plugins', then create admin user 
![Screenshot from 2025-06-20 17-41-40](https://github.com/user-attachments/assets/eb142955-8023-4bd0-a2e3-40d91ef75787)

create new user and password after installing suggested plugins

![Screenshot from 2025-06-20 17-44-39](https://github.com/user-attachments/assets/a3323b09-3f39-4324-be80-cf469dc5ba17)
![Screenshot from 2025-06-20 17-44-48](https://github.com/user-attachments/assets/08c49901-2ac0-4bf0-b5a5-8e0f8b1faa83)

## start using jenkins
![Screenshot from 2025-06-20 17-45-24](https://github.com/user-attachments/assets/4e568762-4413-49b5-8855-9c5ea34222b6)

 Now u can do your work in jenkins like create a job, creating nodes and building jobs .


