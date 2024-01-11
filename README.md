# aws-dev-docs
## What is Amazon EKS?
Amazon Elastic Kubernetes Service (Amazon EKS) is a managed service that **eliminates the need to install, operate, and maintain your own Kubernetes control plane on Amazon Web Services (AWS).** Kubernetes is an `open-source system that automates the management, scaling, and deployment of containerized applications`.

## Features of Amazon EKS

The following are key features of Amazon EKS:

### Secure networking and authentication
> Amazon EKS integrates your Kubernetes workloads with AWS networking and security services. It also integrates with `AWS Identity and Access Management (IAM)` to provide authentication for your Kubernetes clusters.
### Easy cluster scaling
> Amazon EKS enables you to scale your Kubernetes clusters up and down easily based `on the demand` of your workloads. Amazon EKS supports horizontal Pod autoscaling based on CPU or custom metrics, and cluster autoscaling based on the demand of the entire workload.

### Managed Kubernetes experience
> You can make changes to your Kubernetes clusters using **eksctl**, AWS Management Console, AWS Command Line Interface (AWS CLI), the API, kubectl, and Terraform

### High availability
> Amazon EKS provides high availability for your control plane across multiple Availability Zones.
### Integration with AWS services
> Amazon EKS integrates with other AWS services, providing a comprehensive platform for `deploying and managing your containerized applications`. You can also more easily **troubleshoot your Kubernetes workloads with various observability tools**.

![Alt text](image.png)

## Common use cases in Amazon EKS
- **Deploying high-availability applications:**
Using Elastic Load Balancing, you can make sure that your applications are highly available across multiple Availability Zones

- **Building microservices architectures:**
Use Kubernetes service discovery features with [AWS Cloud Map](https://aws.amazon.com/cloud-map/)
or [Amazon VPC Lattice](https://aws.amazon.com/vpc/lattice/) to build resilient systems.
    >  AWS Cloud Map = Service discovery for cloud resources or IT asset discovery


- **Automating software release process:**
Manage continuous integration and continuous deployment `(CICD) pipelines` that simplify the process of automated `building, testing, and deployment of applications`.
- **Running serverless applications:** Use AWS Fargate
with Amazon EKS to run serverless applications. This means you can focus solely on application development, while Amazon EKS and Fargate handle the underlying infrastructure.
- **Executing machine learning workloads:**
Amazon EKS is compatible with popular machine learning frameworks such as **TensorFlow**, **MXNet, and PyTorch**
. With GPU support, you can handle even complex machine learning tasks effectively.

- **Deploying consistently on premises and in the cloud:**
Use Amazon EKS Anywhere to operate Kubernetes clusters on your own infrastructure using tools that are consistent with Amazon EKS in the cloud.
- **Running cost-effective batch processing and big data workloads:**
Utilize Spot Instances to run your batch processing and big data workloads such as `Apache Hadoop` and `Spark`, at a fraction of the cost. This lets you take advantage of unused Amazon EC2 capacity at discounted prices.
- **Securing application and ensuring compliance:**
Implement strong security practices and maintain compliance with Amazon EKS, which integrates with AWS security services such as AWS Identity and Access Management
(`IAM`), Amazon Virtual Private Cloud (`Amazon VPC`), and AWS Key Management Service (`AWS KMS`). This ensures data privacy and protection as per industry standards.

## Amazon EKS nodes
A Kubernetes node is a machine that runs containerized applications. Each node has the following components:

- **[Container runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)**

    Software that's responsible for running the containers.

- **[kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)**

    Makes sure that containers are healthy and running within their associated Pod.

- **[kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/)**

    Maintains network rules that allow communication to your Pods.

    ![Alt text](image-1.png)
    ## Managed node groups
    Amazon EKS managed node groups automate the provisioning and lifecycle management of nodes (Amazon EC2 instances) for Amazon EKS Kubernetes clusters.

    `With Amazon EKS managed node groups, you don't need to separately provision or register the Amazon EC2 instances that provide compute capacity to run your Kubernetes applications`. You can create, automatically update, or terminate nodes for your cluster with a single operation. Node updates and terminations automatically drain nodes to ensure that your applications stay available.

    Every managed node is provisioned as part of an Amazon EC2 Auto Scaling group that's managed for you by Amazon EKS. Every resource including the instances and Auto Scaling groups runs within your AWS account. `Each node group runs across multiple Availability Zones that you define`.

    You can add a managed node group to new or existing clusters using the Amazon EKS console, `eksctl`, AWS CLI; AWS API, or infrastructure as code tools including AWS CloudFormation. Nodes launched as part of a managed node group are automatically tagged for auto-discovery by the Kubernetes cluster autoscaler. You can use the node group to apply Kubernetes labels to nodes and update them at any time.

    There are no additional costs to use Amazon EKS managed node groups, you only pay for the AWS resources you provision. These include Amazon EC2 instances, Amazon EBS volumes, Amazon EKS cluster hours, and any other AWS infrastructure. There are no minimum fees and no upfront commitments.

    ## Self-managed nodes

    A cluster contains one or more Amazon EC2 nodes that `Pods` are scheduled on. Amazon EKS nodes run in your AWS account and connect to the control plane of your cluster through the cluster API server endpoint. You're billed for them based on Amazon EC2 prices. For more information, see *Amazon EC2 pricing*

    A cluster can contain several node groups. Each node group contains one or more nodes that are deployed in an Amazon EC2 Auto Scaling group. The instance type of the nodes within the group can vary, such as when using *attribute-based instance* type selection with [Karpenter](https://karpenter.sh/). All instances in a node group must use the Amazon EKS node IAM role.

    Amazon EKS provides specialized Amazon Machine Images (AMIs) that are called Amazon EKS optimized AMIs. The AMIs are configured to work with Amazon EKS. Their components include `containerd`, `kubelet`, and the AWS IAM Authenticator. The AMIs also contain a specialized [bootstrap script](https://github.com/awslabs/amazon-eks-ami/blob/master/files/bootstrap.sh) that allows it to discover and connect to your cluster's control plane automatically.

    If you restrict access to the public endpoint of your cluster using CIDR blocks, we recommend that you also enable private endpoint access. This is so that nodes can communicate with the cluster. Without the private endpoint enabled, the CIDR blocks that you specify for public access must include the egress sources from your VPC. For more information, see *Amazon EKS cluster endpoint access control*. 

    ## AWS Fargate
    This topic discusses using Amazon EKS to run Kubernetes Pods on AWS Fargate. Fargate is a technology that provides on-demand, right-sized compute capacity for [containers](https://aws.amazon.com/containers/). With Fargate, `you don't have to provision, configure, or scale groups of virtual machines on your own to run containers. You also don't need to choose server types, decide when to scale your node groups, or optimize cluster packing`. For more information, see [What is AWS Fargate?](https://docs.aws.amazon.com/AmazonECS/latest/userguide/what-is-fargate.html) in the Amazon *Elastic Container Service User Guide for AWS Fargate*.

    You can control which Pods start on Fargate and how they run with Fargate profiles. [Fargate profiles](https://docs.aws.amazon.com/eks/latest/userguide/fargate-profile.html) are defined as part of your Amazon EKS cluster. Amazon EKS integrates Kubernetes with Fargate by using controllers that are built by AWS using the upstream, extensible model provided by Kubernetes. These controllers run as part of the Amazon EKS managed Kubernetes control plane and are responsible for scheduling native Kubernetes Pods onto Fargate. The Fargate controllers include a new scheduler that runs alongside the default Kubernetes scheduler in addition to several mutating and validating admission controllers. When you start a Pod that meets the criteria for running on Fargate, the Fargate controllers that are running in the cluster recognize, update, and schedule the Pod onto Fargate

## Storage

- Amazon EBS CSI driver
- Amazon EFS CSI driver
- Amazon FSx for Lustre CSI driver
- Amazon FSx for NetApp ONTAP CSI driver
- Amazon FSx for OpenZFS CSI driver
- Amazon File Cache CSI driver
- Mountpoint for Amazon S3 CSI driver
- CSI snapshot controller

