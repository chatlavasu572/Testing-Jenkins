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

## create an instance in the aws console.
Instance name with Dummy-Jenkins-Server 
![Screenshot from 2025-06-20 18-15-35](https://github.com/user-attachments/assets/e88a1a7c-af6b-4e4a-a6f1-5f3fa37db800)

with type "t2.micro" and with key pair named "jenkinsKeyPair" 
![Screenshot from 2025-06-20 18-15-57](https://github.com/user-attachments/assets/cde3058e-b4bf-4296-9164-cda1b7e94008)

with 8GB of Volume
![Screenshot from 2025-06-20 18-16-39](https://github.com/user-attachments/assets/3e38f2eb-b5c7-4d06-af24-8870059eaa19)

then click on launch instance

## you will see the instances which is running like this
![Screenshot from 2025-06-20 18-20-11](https://github.com/user-attachments/assets/7969c7ce-7386-4366-bd8e-95377c46464f)

## connect ec2-instance in the local machine using SSH-client 
copy that code 
``` 
"ssh -i "jenkinskeypair.pem" ubuntu@ec2-13-204-87-116.ap-south-1.compute.amazonaws.com"
````
![Screenshot from 2025-06-20 18-24-35](https://github.com/user-attachments/assets/59eae808-400f-46aa-93c8-1131374ac1e3)
Getting error because of no permission for our public_key.. need to pass the command for providing permissions to the public key .
 ```
chmod 400 "jenkinskeypair.pem"
'''

![Screenshot from 2025-06-20 18-20-24](https://github.com/user-attachments/assets/2faa3675-96e6-4169-96df-44b759484fa4)

## Terminal from local machine 


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
click on save and finish

![Screenshot from 2025-06-20 17-44-39](https://github.com/user-attachments/assets/a3323b09-3f39-4324-be80-cf469dc5ba17)

## start using jenkins

![Screenshot from 2025-06-20 17-44-48](https://github.com/user-attachments/assets/08c49901-2ac0-4bf0-b5a5-8e0f8b1faa83)

## welcome to the dashboard 
![Screenshot from 2025-06-20 17-45-24](https://github.com/user-attachments/assets/4e568762-4413-49b5-8855-9c5ea34222b6)

 Now u can do your work in jenkins like create a job, creating nodes and building jobs .

 Now you pass the values like API_KEY,USER,Local_Host etc.. in the Environmental variables in the monitor pythonScript.
 click on save and check how the monitor is working whether it is working or giving any failed issues.

 ## If the monitor runs successful you will see the output as like this ...

 ![Screenshot from 2025-06-08 12-33-42](https://github.com/user-attachments/assets/738227a1-c150-4d68-85d3-6fe784b7b4ca)



