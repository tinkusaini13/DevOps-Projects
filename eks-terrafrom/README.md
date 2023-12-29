

# Provisioning AWS EKS Cluster with Terraform

> What is AWS EKS?
AWS EKS (Elastic Kubernetes Service) is a managed service from AWS that makes Kubernetes cluster management easier to implement and scale. It provides a reliable and scalable platform to run Kubernetes workloads, allowing engineers to focus on building applications while AWS takes care of managing the underlying Kubernetes infrastructure.

> Why Should You Use Terraform with AWS EKS?

Using Terraform with AWS EKS provides many benefits, from streamlining the process of provisioning to configuring and managing your Kubernetes clusters. Managing the lifecycle of a service through Infrastructure as Code, it’s usually a very good idea, as you will configure that service faster, while minimizing the potential for human error




## How to Provision an AWS EKS Cluster with Terraform

- Install Terraform  and kubectl  Locally
- Configure the AWS CLI
- Get the Code


## Configure the AWS CLI

Now, you’ll need to configure your AWS CLI with access credentials to your AWS account.

        aws configure
  

## Get the Code

You can now clone a repository which contains everything you need to set up EKS:

        https://github.com/tinkusaini13/DevOps-Projects.git

        cd DevOps-Projects/eks-terrafrom/

## Terraform Initial Setup Configuration
Create an AWS provider. It allows to interact with the AWS resources, such as VPC, EKS, S3, EC2, and many others.


- main.tf -  This is where EKS module is used. It consists of all the code for infra to provision.

- variables.tf -  File to declare variables to be used in main.tf

- output.tf - Containing the output that needs to be generated on successful completion of “apply” operation. like cluster name and End point name.

- terrafrom.tf - Could be used for configuring Terraform backend settings, provider configurations, or other global settings.


## Provisioning EKS Cluster

        terraform init
        terraform plan
        terraform apply


Once the terraform apply is completed successfully, it will show a set of terraform output values containing the details of the newly created cluster.

Run the below command to update the users kubeconfig file to start using the cluster.

        aws eks --region ap-south-1 update-kubeconfig --name <output.cluster_name>
