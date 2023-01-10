# EKS Cluster with Teams to a new VPC

This example deploys a new EKS Cluster with Teams to a new VPC. It's based one of the examples from https://github.com/aws-ia/terraform-aws-eks-blueprints

- Creates a new sample VPC, 3 Private Subnets and 3 Public Subnets
- Creates an Internet gateway for the Public Subnets and a NAT Gateway for the Private Subnets
- Creates an EKS Cluster Control plane with public endpoint with one managed node group
- Creates two application teams - blue and red and deploys team manifests to the cluster
- Creates a single platform admin team - you will need to provide your own IAM user/role first, see the example for more details
- All resources are tagged with Environment, Service, and Owner (tag inputs).

Several other features are also configured.

- Limits/Quotas for application teams
- Gatekeeper is installed and configured for Pod security, and a simple constraint is created
- An example deployment in violation of the PSP (Pod Security Policy) is created to make it easy to verify functionality

Additional services/addons include:

- Cluster Autoscaler
- Kubernetes Dashboard
- Cloudwatch Metrics
- VPC CNI
- CoreDNS
- EKS kubeproxy
- AWS EBS CSI driver

It's easy to add other services/addons that are avaialble from the upstream source https://github.com/aws-ia/terraform-aws-eks-blueprints. This module uses a fork as a submodule so that changes can be picked up as desired but there's also a layer where customization is easy if it's needed.

## How to Deploy

### Prerequisites:

Ensure that you have installed the following tools in your Mac or Windows Laptop before start working with this module and run Terraform Plan and Apply

1. [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
2. [Kubectl](https://Kubernetes.io/docs/tasks/tools/)
3. [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

### Deployment Steps

#### Step 1: Clone the repo using the command below

```sh
git clone https://github.com/csuttles/aws-eks-cluster-terraform
```

#### Step 2: Run `terraform init`

to initialize a working directory with configuration files

```sh
cd aws-eks-cluster-terraform/examples/hackerrank
terraform init
```

#### Step 3: Run `terraform plan`

to verify the resources created by this execution

```sh
export AWS_REGION=<enter-your-region>   # Select your own region
terraform plan
```

#### Step 4: Finally, `terraform apply`

to create resources

```sh
terraform apply
```

Enter `yes` to apply

### Configure kubectl and test cluster

EKS Cluster details can be extracted from terraform output or from AWS Console to get the name of cluster. This following command used to update the `kubeconfig` in your local machine where you run kubectl commands to interact with your EKS Cluster.

#### Step 5: Run update-kubeconfig command.

`~/.kube/config` file gets updated with cluster details and certificate from the below command

    $ aws eks --region <enter-your-region> update-kubeconfig --name <cluster-name>

#### Step 6: List all the worker nodes by running the command below

    $ kubectl get nodes

#### Step 7: List all the pods running in kube-system namespace

    $ kubectl get pods -n kube-system

## How to Destroy

```sh
terraform destroy -auto-approve
```
