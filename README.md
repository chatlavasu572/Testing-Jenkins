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

## Unlock jenkins 
 open browser and paste the link : http://localhost:8080 please  provide your PUBLIC_IP in place of localhost.
if your a first accessjenkins controller you will output like this :
![image](https://github.com/user-attachments/assets/b390b591-a8c6-4a26-88be-082f1970e9fc)
 
 ## Initial Admin Password
 if you run this command you will get password in your ec2-instance terminal 
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
copy the password and paste it your Administrator Password.then click on continue....


 
