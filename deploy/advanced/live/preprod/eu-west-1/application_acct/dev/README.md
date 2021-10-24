# Application Account Dev Cluster Deployment
The following steps walks you through the deployment of this example

This example deploys the following Basic EKS Cluster with VPC

 - Creates a new sample VPC, 3 Private Subnets and 3 Public Subnets
 - Creates Internet gateway for Public Subnets and NAT Gateway for Private Subnets
 - Creates EKS Cluster Control plane with one managed node group, self managed and fargate profile
 - Deploys all available Helm Addons

# How to Deploy

## Prerequisites:
Ensure that you have installed the following tools in your Mac or Windows Laptop before start working with this module and run Terraform Plan and Apply

1. [aws cli](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
2. [aws-iam-authenticator](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html)
3. [kubectl](https://Kubernetes.io/docs/tasks/tools/)
4. [terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

## Deployment Steps

#### Step1: Clone the repo using the command below

```shell script
git clone https://gitlab.aws.dev/vabonthu/terraform-aws-eks-accelerator-patterns.git
```

#### Step2: Run Terraform INIT
to initialize a working directory with configuration files

```shell script
cd deploy/advanced/live/preprod/eu-west-1/application_acct/dev
terraform init
```

#### Step3: Run Terraform PLAN
to verify the resources created by this execution

```shell script
export AWS_REGION="eu-west-1"   # Select your own region
terraform plan
```

#### Step4: Finally, Terraform APPLY
to create resources

```shell script
terraform apply
```

Enter `yes` to apply

### Configure kubectl and test cluster
EKS Cluster details can be extracted from terraform output or from AWS Console to get the name of cluster. This following command used to update the `kubeconfig` in your local machine where you run kubectl commands to interact with your EKS Cluster.

#### Step5: Run update-kubeconfig command.

`~/.kube/config` file gets updated with cluster details and certificate from the below command

    $ aws eks --region eu-west-1 update-kubeconfig --name <cluster-name>

#### Step6: List all the worker nodes by running the command below

    $ kubectl get nodes

#### Step7: List all the pods running in kube-system namespace

    $ kubectl get pods -n kube-system

# How to Destroy
```shell script
cd deploy/advanced/live/preprod/eu-west-1/application_acct/dev
terraform destroy
```