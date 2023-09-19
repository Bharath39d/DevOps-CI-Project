## Continuous Integration Project using Amazon Ubuntu

#### Step 1 

Create the Ec2 instance in AWS account with these parameters
- EC2 type – Ubuntu t2.medium
- EBS volume – 30 GB
- Region – US-EAST-1

#### Step 2 

Connect to EC2 and Install all tools in that system as root user. To login as root user use 'sudo su'

#### Step 3 Install Jenkins on Ubuntu

- #!/bin/bash
- sudo apt update -y
- sudo apt upgrade -y
- sudo apt install openjdk-17-jre -y
- curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
 /usr/share/keyrings/jenkins-keyring.asc > /dev/null
- echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
 https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
 /etc/apt/sources.list.d/jenkins.list > /dev/null
- sudo apt-get update -y
- sudo apt-get install jenkins -y

#### Step 4 -Change the security group of ec2 instance

- EC2 Instance >> Security >> Security groups >> Edit Inbound Rules >> All traffic and anywhere from IPV4 >> Add rule

#### Step 5 – Sign Into Jenkins console

- http://<EC2_PUBLIC \_IP>:8080/

#### Step 6 – Get the Administrator password by hitting the below command in EC2

- cat /var/lib/jenkins/secrets/initialAdminPassword

#### Step 7 – Install all suggested plugins

#### Step 8 – Create first user

#### Step 9 – Create a pipeline Job

#### Step 10

Under configure >> Add pipeline script as SCM
- SCM >> Git
- Repositories >> Repository URL >> https://github.com/Bharath39d/Java_app_3.0.git  and Credentials >> none
- Branches to build >> Branch Specifier >> */main
save the pipeline

#### Step 11 

Add the Plugins >> Dashboard -> Manage Jenkins -> Plugins -> Available plugins

Plugins for Sonar/Jfrog –
- Sonar Gerrit
- SonarQube Scanner
- SonarQube Generic Coverage
- Sonar Quality Gates
- Quality Gates
- Artifactory
- Jfrog

#### Step 12 – Setup Docker

- #!/bin/bash
- sudo apt update -y
- sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
- curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
- sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" -y
- sudo apt update -y
- apt-cache policy docker-ce -y
- sudo apt install docker-ce -y
- sudo chmod 777 /var/run/docker.sock
- docker -v

#### Step 13- Install SonarQube

-  docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube

Start docker container if it's not up

- docker ps -a [ Get the container ID ]
- docker start <containerID>

#### Step 13.1 - Goto http://<EC2_PUBLIC \_IP>:9000/
#### Step 13.2 -> Login into sonar dashboard
- Username – admin
- Password – admin
#### Step 13.3 -> Create Sonar token for Jenkins
- Sonar Dashboard -> Administration -> My Account -> Security -> Create token -> Name: <Any Name> Type: Global Analysis Token Expires in: 30 days -> Generate [Save the token to some text file]

#### Step 13.4 -> Integrate Sonar to Jenkins
- Sonar Dashboard -> Administration -> Configuration -> webhooks -> Add the below name and url and save
- Name: Jenkins URL: http://<EC2_IP>:8080/sonarqube-webhook/

#### Step 14 – Install Maven
- sudo apt update -y
- sudo apt install maven -y
- mvn -version

#### Step 15 – Install TRIVY for docker image scan

- # A Simple and Comprehensive Vulnerability Scanner for Containers and other Artifacts, Suitable for CI.
- sudo apt-get install wget apt-transport-https gnupg lsb-release
- wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
- echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
- sudo apt-get update
- sudo apt-get install trivy

## Integrate All tools with Jenkins

#### Step 16 - Jenkins Dashboard -> Manage Jenkins ->configure system 
