# Launching-eks-ec2
# EKS Cluster Launch Guide

This guide provides step-by-step instructions to set up an Elastic Kubernetes Service (EKS) cluster on AWS, deploy an Nginx application, and test high availability features.

## Prerequisites

- AWS account credentials
- Access to the AWS Management Console
- AWS CLI installed
- Git installed

## Step 1: Create an IAM User with Admin Permissions

1. Navigate to IAM > Users.
2. Click **Add users**.
3. Enter "k8-admin" in the User name field.
4. Select **Attach policies directly**.
5. Select **AdministratorAccess**.
6. Click **Create user**.
7. Copy the access key and secret access key.

## Step 2: Launch an EC2 Instance and Configure Command Line Tools

1. Navigate to EC2 > Instances.
2. Launch an instance with Amazon Linux 2 AMI and t2.micro type.
3. Connect to the instance and update AWS CLI to v2.

```bash
aws --version
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install --bin-dir /usr/bin --install-dir /usr/bin/aws-cli --update
aws --version

aws configure
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.16.8/2020-04-16/bin/linux/amd64/kubectl
chmod +x ./kubectl
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
kubectl version --short --client
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/bin
eksctl version
```

## Step 3: Provision an EKS Cluster
  - Provision an EKS cluster with the specified configuration

```bash
eksctl create cluster --name dev --region us-east-1 --nodegroup-name standard-workers --node-type t3.medium --nodes 3 --nodes-min 1 --nodes-max 4 --managed
```
- Monitor CloudFormation stacks in the AWS Management Console.
- Inspect the EKS cluster details and EC2 instances.

## Step 4: Create a Deployment on Your EKS Cluster
- Install Git and download course files.
  
```
sudo yum install -y git
git clone https://github.com/ACloudGuru-Resources/Course_EKS-Basics
cd Course_EKS-Basics
```
## Review and apply the Nginx service and deployment.
  
``` bash
cat nginx-svc.yaml
kubectl apply -f ./nginx-svc.yaml
kubectl get service
cat nginx-deployment.yaml
kubectl apply -f ./nginx-deployment.yaml
kubectl get deployment
kubectl get pod
```
- Access the application using the LoadBalancer DNS hostname.

```
curl "<LOAD_BALANCER_DNS_HOSTNAME>"
```

## Step 5: Test High Availability Features of Your EKS Cluster
  -  Stop one worker node instance in the AWS console.
  -  Check the status of nodes and pods using kubectl.
    
```
kubectl get node
kubectl get pod
```

## Wait for EKS to launch new instances and restart pods.
-  Access the application using the LoadBalancer DNS hostname.

``` 
curl "<LOAD_BALANCER_DNS_HOSTNAME>"
Delete the EKS cluster using eksctl.
```
```
eksctl delete cluster dev --region us-east-1
```
Conclusion
Congratulations! You have successfully launched an EKS cluster, deployed an application, and tested high availability features. If you encountered any issues, refer to the AWS and Kubernetes documentation for troubleshooting.

arduino
Copy code

Save this content in a file named `README.md` in the root of your project directory.




