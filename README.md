# Testing-Jenkins
## what is jenkins?
Jenkins is an open-source automation server used to automate building, testing, and deploying
software. It helps DevOps teams manage CI/CD pipelines efficiently.

## why we need jenkins?
Jenkins is used to automate repetitive tasks in the software development lifecycle, such as 
code
integration, testing, and deployment. It helps detect bugs early, speed up delivery, and ensures
consistent builds.

## Installation Process :

## create an instance in the aws console.
Instance name with Dummy-Jenkins-Server 

![Screenshot from 2025-06-20 18-15-35](https://github.com/user-attachments/assets/e88a1a7c-af6b-4e4a-a6f1-5f3fa37db800)

with type "t2.micro" and with key pair named "jenkinsKeyPair" 

![Screenshot from 2025-06-20 18-15-57](https://github.com/user-attachments/assets/cde3058e-b4bf-4296-9164-cda1b7e94008)

with 8GB of Configure-Volume

![Screenshot from 2025-06-20 18-16-39](https://github.com/user-attachments/assets/3e38f2eb-b5c7-4d06-af24-8870059eaa19)

then click on launch instance

## you will see the instances which is running like this

![Screenshot from 2025-06-20 18-20-11](https://github.com/user-attachments/assets/7969c7ce-7386-4366-bd8e-95377c46464f)

## connect ec2-instance in the local machine using SSH-client 

copy that code 

![Screenshot from 2025-06-20 18-20-24](https://github.com/user-attachments/assets/b708fa64-ad27-44dc-9747-9ce8ac4058df)


``` 
"ssh -i "jenkinskeypair.pem" ubuntu@ec2-13-204-87-116.ap-south-1.compute.amazonaws.com"
````
![Screenshot from 2025-06-20 18-24-35](https://github.com/user-attachments/assets/59eae808-400f-46aa-93c8-1131374ac1e3)

Getting error because of no permission for our public_key.. need to pass the command for providing permissions to the public key .
 ```
chmod 400 "jenkinskeypair.pem"
```

![Screenshot from 2025-06-20 18-20-24](https://github.com/user-attachments/assets/2faa3675-96e6-4169-96df-44b759484fa4)

## Terminal from local machine 


## first we need  java so install java.
```
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
```
openjdk version "21.0.3" 2024-04-16
OpenJDK Runtime Environment (build 21.0.3+11-Debian-2)
OpenJDK 64-Bit Server VM (build 21.0.3+11-Debian-2, mixed mode, sharing)


## installation of jenkins :
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
if your a first access jenkins controller you will output like this :
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

 ![Screenshot from 2025-06-20 18-44-47](https://github.com/user-attachments/assets/2218d92b-ef30-4ac9-96b3-117a2759c119)


 Now you pass the values like API_KEY,USER,Local_Host etc.. in the Environmental variables in the monitor pythonScript.
 click on save and check how the monitor is working whether it is working or giving any failed issues.
 ## The Four monitors are :
 ### 1. connectivity check:
 This is used to check the endpoints are reachable to the jenkins.

```
#!/usr/bin/env python3
"""
Jenkins Connectivity Check

This script checks if Jenkins is accessible via its REST API.

Environment Variables:
    JENKINS_URL - Jenkins base URL (default: http://localhost:8080)
    JENKINS_USER - Jenkins username (if required)
    JENKINS_API_TOKEN - API token for authentication (if required)

Exit Codes:
    0 - Success (Jenkins is reachable)
    1 - Failure (Jenkins is unreachable)
"""

import logging
import os
import sys

import requests
from requests.auth import HTTPBasicAuth

# Configure logging
logging.basicConfig(
    level=logging.INFO, format="%(asctime)s - %(levelname)s - %(message)s"
)

# Environment variables
JENKINS_URL = os.getenv("JENKINS_URL", "http://13.204.87.116:8080/")
JENKINS_USER = os.getenv("JENKINS_USER","vasu")
JENKINS_API_TOKEN = os.getenv("JENKINS_API_TOKEN","1188868b6b04fc85b3c")
def check_connectivity():
    """Check if Jenkins is accessible via API."""
    try:
        auth = (
            HTTPBasicAuth(JENKINS_USER, JENKINS_API_TOKEN)
            if JENKINS_USER and JENKINS_API_TOKEN
            else None
        )
        response = requests.get(f"{JENKINS_URL}/api/json", auth=auth, timeout=5)
        response.raise_for_status()
        logging.info("Jenkins is reachable.")
        return True
    except requests.exceptions.RequestException as e:
        logging.error(f"Failed to connect to Jenkins: {e}")
        return False


if __name__ == "__main__":
    sys.exit(0 if check_connectivity() else 1)

 ```
![Screenshot from 2025-06-23 13-58-25](https://github.com/user-attachments/assets/6f8c4f44-c6a8-4088-9eba-1a21e93b86bc)

### 2. Failed job check :
```
"""
Jenkins Failed Jobs Monitor

This script checks for failed Jenkins jobs in the last N builds.

Environment Variables:
    JENKINS_URL - Jenkins base URL (default: http://localhost:8080)
    JENKINS_USER - Jenkins username (if required)
    JENKINS_API_TOKEN - API token for authentication (if required)
    MAX_FAILED_JOBS - Maximum acceptable failed jobs in recent builds (default: 3)

Exit Codes:
    0 - Success (Failed jobs within limits)
    1 - Failure (Too many failed jobs detected)
"""

import logging
import os
import sys

import requests
from requests.auth import HTTPBasicAuth

# Configure logging
logging.basicConfig(
    level=logging.INFO, format="%(asctime)s - %(levelname)s - %(message)s"
)


# Environment variables
JENKINS_URL = os.getenv("JENKINS_URL", "http://3.108.220.114:8080")
JENKINS_USER = os.getenv("JENKINS_USER","admin")
JENKINS_API_TOKEN = os.getenv("JENKINS_API_TOKEN","11b499e45f5be51f661a783afd0c8ec917")
MAX_FAILED_JOBS = int(os.getenv("MAX_FAILED_JOBS", 3))


def check_failed_jobs():
    """Check for failed Jenkins jobs in recent builds."""
    try:
        auth = (
            HTTPBasicAuth(JENKINS_USER, JENKINS_API_TOKEN)
            if JENKINS_USER and JENKINS_API_TOKEN
            else None
        )
        response = requests.get(
            f"{JENKINS_URL}/api/json?tree=jobs[name,lastBuild[result]]",
            auth=auth,
            timeout=5,
        )
        response.raise_for_status()
        jobs = response.json().get("jobs", [])

        failed_jobs = [
            job["name"]
            for job in jobs
            if job.get("lastBuild", {}).get("result") == "FAILURE"
        ]
        failed_count = len(failed_jobs)

        logging.info(f"Number of failed jobs: {failed_count}")
        if failed_count > MAX_FAILED_JOBS:
            logging.warning(
                f"Failed jobs exceeded limit: {failed_count} > {MAX_FAILED_JOBS}"
            )
            return False

        return True
    except requests.exceptions.RequestException as e:
        logging.error(f"Failed to fetch Jenkins jobs: {e}")
        return False


if __name__ == "__main__":
    sys.exit(0 if check_failed_jobs() else 1)
```

![Screenshot from 2025-06-23 13-58-46](https://github.com/user-attachments/assets/62790750-a48e-4812-93a6-f5fc2f9ae4cb)

### 3.Job Queue Check:
```
#!/usr/bin/env python3
"""
Jenkins Job Queue Check

This script checks the number of pending jobs in Jenkins queue.

Environment Variables:
    JENKINS_URL - Jenkins base URL (default: http://localhost:8080)
    JENKINS_USER - Jenkins username (if required)
    JENKINS_API_TOKEN - API token for authentication (if required)
    MAX_QUEUE_SIZE - Maximum acceptable pending jobs (default: 5)

Exit Codes:
    0 - Success (Queue size within limits)
    1 - Failure (Queue size exceeded)
"""

import logging
import os
import sys

import requests
from requests.auth import HTTPBasicAuth

logging.basicConfig(
    level=logging.INFO, format="%(asctime)s - %(levelname)s - %(message)s"
)

# Environment variables
JENKINS_URL = os.getenv("JENKINS_URL", "http://3.108.220.114:8080")
JENKINS_USER = os.getenv("JENKINS_USER","admin")
JENKINS_API_TOKEN = os.getenv("JENKINS_API_TOKEN","11b499e45f5be51f661a783afd0c8ec917")


MAX_QUEUE_SIZE = int(os.getenv("MAX_QUEUE_SIZE", 5))


def check_job_queue():
    """Check the number of jobs in Jenkins queue."""
    try:
        auth = (
            HTTPBasicAuth(JENKINS_USER, JENKINS_API_TOKEN)
            if JENKINS_USER and JENKINS_API_TOKEN
            else None
        )
        response = requests.get(f"{JENKINS_URL}/queue/api/json", auth=auth, timeout=5)
        response.raise_for_status()
        queue = response.json()

        queue_size = len(queue.get("items", []))
        logging.info(f"Jenkins job queue size: {queue_size}")

        if queue_size > MAX_QUEUE_SIZE:
            logging.warning(
                f"Job queue exceeded threshold: {queue_size} > {MAX_QUEUE_SIZE}"
            )
            return False

        return True
    except requests.exceptions.RequestException as e:
        logging.error(f"Failed to fetch Jenkins job queue: {e}")
        return False


if __name__ == "__main__":
    sys.exit(0 if check_job_queue() else 1)
```

![Screenshot from 2025-06-23 13-58-38](https://github.com/user-attachments/assets/c17e05c5-91d2-4312-adf2-7ccadb9c549c)

### 4.Node Health Check :

```
"""
Jenkins Node Health Monitor

This script checks if any Jenkins nodes are offline or failing.

Environment Variables:
    JENKINS_URL - Jenkins base URL (default: http://localhost:8080)
    JENKINS_USER - Jenkins username (if required)
    JENKINS_API_TOKEN - API token for authentication (if required)

Exit Codes:
    0 - Success (All nodes healthy)
    1 - Failure (Some nodes are offline)
"""

import logging
import os
import sys

import requests

from requests.auth import HTTPBasicAuth



# Configure logging
logging.basicConfig(
    level=logging.INFO, format="%(asctime)s - %(levelname)s - %(message)s"
)

# Environment variables
JENKINS_URL = os.getenv("JENKINS_URL", "http://3.108.220.114:8080")
JENKINS_USER = os.getenv("JENKINS_USER","admin")
JENKINS_API_TOKEN = os.getenv("JENKINS_API_TOKEN","11b499e45f5be51f661a783afd0c8ec917")


def check_node_health():
    """Check the health of Jenkins nodes."""
    try:
        auth = (
            HTTPBasicAuth(JENKINS_USER, JENKINS_API_TOKEN)
            if JENKINS_USER and JENKINS_API_TOKEN
            else None
        )
        response = requests.get(
            f"{JENKINS_URL}/computer/api/json", auth=auth, timeout=5
        )
        response.raise_for_status()
        nodes = response.json().get("computer", [])

        offline_nodes = [node["displayName"] for node in nodes if node.get("offline")]
        if offline_nodes:
            logging.warning(f"Offline Jenkins nodes detected: {offline_nodes}")
            return False

        logging.info("All Jenkins nodes are online and healthy.")
        return True
    except requests.exceptions.RequestException as e:
        logging.error(f"Failed to fetch Jenkins node health: {e}")
        return False


if __name__ == "__main__":
    sys.exit(0 if check_node_health() else 1)
```
![Screenshot from 2025-06-23 13-59-00](https://github.com/user-attachments/assets/5bd809a2-14d3-472a-bb13-e4da03c5e822)


 ## If the monitor runs successful you will see the output as like this ...

 ![Screenshot from 2025-06-08 12-33-42](https://github.com/user-attachments/assets/738227a1-c150-4d68-85d3-6fe784b7b4ca)

 ## If any monitor failed you can see the red dot blink in the application monitor itself.
 ![Screenshot from 2025-06-20 19-39-35](https://github.com/user-attachments/assets/db44204d-4ffa-416b-9cf5-48fa6d97c5c1)

 ## Conclusion:
All four monitors are working successfully, indicating that the system's health checks are functioning as expected. Each monitor is actively running, returning valid results, and confirming that all critical components of the application are stable, responsive, and under proper observation. Monitoring is healthy and no issues were detected.
 




