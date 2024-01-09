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
5. Create S3 Bucket in AWS
Create an S3 bucket that Kubernetes will use to persist cluster state. Make sure the bucket name is unique across all AWS accounts.

 ```
aws s3 mb s3://your-unique-bucket-name --region your-region
 ```

6. Create Private Hosted Zone in AWS Route53
Head over to AWS Route53 and create a private hosted zone for your cluster. Choose a name (e.g., mykubernetescluster.local) and type as a private hosted zone for VPC. Select the default VPC in the region you are setting up your cluster, and then hit create.

7. Configure Environment Variables
Open the .bashrc file on your EC2 instance:
 ```
vi ~/.bashrc
 ```
following content into .bashrc, using your chosen cluster name and the S3 bucket name created in the previous step:
```
export KOPS_CLUSTER_NAME=mykubernetescluster.local
export KOPS_STATE_STORE=s3://your-unique-bucket-name
```
Run the following command to reflect the variables added to .bashrc:
```
source ~/.bashrc
```
8. Create SSH Key Pair
Generate an SSH key pair that will be used for SSH access to the Kubernetes cluster:
```
ssh-keygen
```
9. Create a Kubernetes Cluster Definition
```
kops create cluster \
--state=${KOPS_STATE_STORE} \
--node-count=2 \
--master-size=t3.medium \
--node-size=t3.medium \
--zones=your-availability-zone \
--name=${KOPS_CLUSTER_NAME} \
--dns private \
--master-count 1
```
