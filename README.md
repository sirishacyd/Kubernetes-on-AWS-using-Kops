# Kubernetes on AWS using Kops

Deploying and managing a Kubernetes cluster on AWS using Kops. Kops is a tool for creating, destroying, upgrading, and maintaining production-grade, highly available, Kubernetes clusters on AWS.

## Prerequisites

- An AWS account
- The AWS CLI installed and configured on your local machine
- The kops and kubectl command-line tools installed on your local machine

## Instructions

1. Launch Linux EC2 instance in AWS (Kubernetes Client)

2. Create and attach an IAM role to the EC2 Instance. Kops needs permissions to access:
   - S3
   - EC2
   - VPC
   - Route53
   - Autoscaling
   - etc..

3. Install Kops on EC2

   ```bash
   curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
   chmod +x kops-linux-amd64
   sudo mv kops-linux-amd64 /usr/local/bin/kops
   
4. Install kubectl
 ```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
 ```
