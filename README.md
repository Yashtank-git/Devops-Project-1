# django-docker-jenkins-terraform-aws
## Project Description
This project is to create Infrastructure in AWS as infrastructure as Code  for a Django todoApp application forked from shreys7/django-todo:develop. This will setup a Jenkins server as well for configuring **CI**(Continious Integration) and **CD**(Continious Deployment) for the Application to Build and Test. Deployment of the application is done with docker-compose on App Server.

Technologies Used:
- Cloud Provider               - **AWS**
- IoC(Infrastructure as Code)  - **Terraform**
- CICD Tool                    - **Jenkins**
- Scripting                    - **Shell Scripting/Python**
- Containerization             - **Docker**

## Setup

Download AWS CLI as per Appropriate OS: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
Download Terraform as per Appropriate OS: https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli

Create a IAM User from AWS Console for Terraform to create/modify/destroy AWS Resources

Configure aws IAM User for using Terraform as Ioc
Create a key pair to login to ec2 instances named as "todoapp-key" and download private key

```bash
aws configure 
$ aws --version
aws-cli/2.15.27 Python/3.11.8 Linux/5.15.0-1055-aws exe/x86_64.ubuntu.20 prompt/off
aws configure
AWS Access Key ID [None]: <access key id>
AWS Secret Access Key [****************1OuQ]: <secret access key>
Default region name [None]: <default region>
Default output format [None]: 
```

To get this repository, run the following command inside your git enabled terminal
```bash
$ git clone https://github.com/Yashtank-git/django-docker-jenkins-terraform-aws
```
Update the aws config directory file path to **terraform/01-AWS-DOCKER-JENKINS/variables.tf** variable file

## Running IoC Commands

change directory to **terraform/01-AWS-DOCKER-JENKINS** and run below commands in shell

```bash
terraform init
terraform validate
terraform plan
terraform apply
```

## Starting Jenkins - Login and Configuration

Get the public ip of jenkins server from AWS Console and login as 
```bash
http://<jenkins-ip>:8080
```
Please follow the documentation to configure and login Jenkins for first time - https://www.jenkins.io/doc/book/installing/linux/#setup-wizard


## Setting up the Agent and Job in Jenkins
### Setting up the Agent 
The Application web server needs to be setup as Jenkins Agent to run the pipeline on the web server.

- Create a Jenkins credentials for logging into todoapp web server as "ubuntu" as username and "todoapp-ec2" private key as secret key.
- Provide the Agent label as "todoapp-agent"
- Get the public IP of the todoapp webserver by going to AWS Console and provide as host for Jenkins agent by connecting as it SSH.
- home directory should be **/home/ubuntu**
- Once Agent setup is done, Check the Agent log to make use the Agent should be connected and running.

## Setting up Job Pipeline

- Create a new job as "pipeline" type and name
- Setup the Pipepine Definition as "Pipeline script with SCM" SCM as "Git"
- Paste the Repository URL as "https://github.com/Yashtank-git/django-docker-jenkins-terraform-aws"
- Branch Specifier as "main" and Script Path as "Jenkinsfile"

**The job is ready to build and run**

Finally the Application can be accessed with:

```bash
http://<web-sever-public-ip>:8000
```







