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

![Alt text](./imgOne/image.png)

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

    ![Alt text](./imgOne/image-1.png)
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
***
***

# AWS Elastic Beanstalk
Deploy and scale web applications
![Alt text](./imgOne/image-2.png)
AWS Elastic Beanstalk deploys web applications so that you can focus on your business.
![Alt text](./imgOne/image-3.png)
![Alt text](./imgOne/image-4.png)

To use Elastic Beanstalk, you create an application, upload an application version in the form of an **application source bundle** (for example, a Java .war file) to Elastic Beanstalk, and then provide some information about the application. Elastic Beanstalk automatically launches an environment and creates and configures the AWS resources needed to run your code. After your environment is launched, you can then manage your environment and deploy new `application versions`. The following diagram illustrates the workflow of Elastic Beanstalk.

![Alt text](./imgOne/clearbox-flow-00.png)

After you create and deploy your application, information about the application—including metrics, events, and environment status—is available through the Elastic Beanstalk console, APIs, or Command Line Interfaces, including the unified AWS CLI

## Elastic Beanstalk concepts
AWS Elastic Beanstalk enables you to manage all of the resources that run your application as environments. Here are some key Elastic Beanstalk concepts.
### Application

An Elastic Beanstalk application is a logical collection of Elastic *Beanstalk components, including environments, versions, and environment configurations*. In Elastic Beanstalk an application is conceptually similar to a <ins>folder</ins>.

### Application version
In Elastic Beanstalk, an application version refers to a specific, labeled iteration of deployable code for a web application. `An application version points to an Amazon Simple Storage Service (Amazon S3) object that contains the deployable code`, such as a Java WAR file. An application version is part of an application. Applications can have many versions and each application version is unique. In a running environment, you can deploy any application version you already uploaded to the application, or you can upload and immediately deploy a new application version. You might upload multiple application versions to test differences between one version of your web application and another.

### Environment
An environment is a collection of AWS resources running an application version. Each environment runs only one application version at a time, however, you can run the same application version or different application versions in many environments simultaneously. When you create an environment, Elastic Beanstalk provisions the resources needed to run the application version you specified.

### Environment tier
When you launch an Elastic Beanstalk environment, you first choose an environment tier. The environment tier designates the type of application that the environment runs, and determines what resources Elastic Beanstalk provisions to support it. An application that serves HTTP requests runs in a **web server environment tier**. A backend environment that pulls tasks from an Amazon Simple Queue Service (Amazon SQS) queue runs in a **worker environment tier**.

### Environment configuration

An environment configuration identifies a collection of parameters and settings that define how an environment and its associated resources behave. When you update an environment’s configuration settings, Elastic Beanstalk `automatically applies` the changes to existing resources or deletes and deploys new resources (depending on the type of change).

### Saved configuration

A saved configuration is a template that you can use as a **starting point** for creating unique environment configurations. You can create and modify saved configurations, and apply them to environments, using the Elastic Beanstalk console, EB CLI, AWS CLI, or API. The API and the AWS CLI refer to saved configurations as configuration templates.

### Platform

A platform is a combination of an operating system, programming language runtime, web server, application server, and Elastic Beanstalk components. You design and target your web application to a platform. Elastic Beanstalk provides a variety of platforms on which you can build your applications.

    
## Web server environments
The following diagram shows an example Elastic Beanstalk architecture for a web server environment tier, and shows how the components in that type of environment tier work together.

![webserver](./imgOne/webServere_aeb-architecture2.png)
The environment is the heart of the application. In the diagram, the environment is shown within the top-level solid line. When you create an environment, Elastic Beanstalk provisions the resources required to run your application. AWS resources created for an environment include one elastic load balancer (ELB in the diagram), an Auto Scaling group, and one or more Amazon Elastic Compute Cloud (Amazon EC2) instances.

Every environment has a CNAME (URL) that points to a load balancer. The environment has a URL, such as `myapp.us-west-2.elasticbeanstalk.com.` This URL is aliased in **Amazon Route 53** to an Elastic Load Balancing URL—something like `abcdef-123456.us-west-2.elb.amazonaws.com`—by using a CNAME record. Amazon Route 53 is a highly available and scalable Domain Name System (DNS) web service. It provides secure and reliable routing to your infrastructure. Your domain name that you registered with your DNS provider will forward requests to the CNAME.

The load balancer sits in front of the Amazon EC2 instances, which are part of an Auto Scaling group. Amazon EC2 Auto Scaling automatically starts additional Amazon EC2 instances to accommodate increasing load on your application. <ins>If the load on your application decreases, Amazon EC2 Auto Scaling stops instances, but always leaves at least one instance running</ins>

The host manager reports metrics, errors and events, and server instance status, which are available via the Elastic Beanstalk console, APIs, and CLIs.

The Amazon EC2 instances shown in the diagram are part of one security group. A security group defines the firewall rules for your instances. By default, Elastic Beanstalk defines a security group, which allows everyone to connect using port 80 (HTTP). You can define more than one security group. **For example**, you can define a security group for your database server. For more information about Amazon EC2 security groups and how to configure them for your Elastic Beanstalk application, see *Security groups*.

## Worker environments
AWS resources created for a worker environment tier include an Auto Scaling group, one or more Amazon EC2 instances, and an IAM role. For the worker environment tier, Elastic Beanstalk also creates and provisions an Amazon SQS queue if you don’t already have one. `When you launch a worker environment, Elastic Beanstalk installs the necessary support files for your programming language of choice and a daemon on each EC2 instance in the Auto Scaling group`. The daemon reads messages from an Amazon SQS queue. The daemon sends data from each message that it reads to the web application running in the worker environment for processing. If you have multiple instances in your worker environment, each instance has its own daemon, but they all read from the same Amazon SQS queue.

The following diagram shows the different components and their interactions across environments and AWS services.

![workerEnv](./imgOne/aeb-architecture_worker_env.png)

## Deploying applications to Elastic Beanstalk environments

### Choosing a deployment policy

Choosing the right deployment policy for your application is a tradeoff of a few considerations, and depends on your particular needs. The Deployment policies and settings page has more information about each policy, and a detailed description of the workings of some of them.

The following list provides summary information about the different deployment policies and adds related considerations.

- **All at once** – The quickest deployment method. Suitable if you can accept a short loss of service, and if `quick deployments are important to you`. With this method, Elastic Beanstalk deploys the new application version to each instance. Then, the web proxy or application server might need to restart. As a result, your application might be unavailable to users (or have low availability) for a short time.

- **Rolling** – <ins>Avoids downtime and minimizes reduced availability</ins>, at a cost of a longer deployment time. Suitable `if you can't accept any period of completely lost service`. With this method, your application is deployed to your environment one batch of instances at a time. Most bandwidth is retained throughout the deployment.



- **Rolling with additional batch** – <ins>Avoids any reduced availability, at a cost of an even longer deployment time compared to the Rolling method.</ins> Suitable if you must maintain the same bandwidth throughout the deployment. With this method, Elastic Beanstalk launches an extra batch of instances, then performs a rolling deployment. Launching the extra batch takes time, and ensures that the <ins>same bandwidth is retained throughout the deployment.</ins>

- **Immutable** – <ins>A slower deployment method,</ins> that ensures your new application version is always deployed to new instances, instead of updating existing instances. It also has the additional `advantage of a quick and safe rollback in case the deployment fails.` With this method, Elastic Beanstalk performs an immutable update to deploy your application. In an immutable update, a second Auto Scaling group is launched in your environment and the new version serves traffic alongside the old version until the new instances pass health checks.

- **Traffic splitting** – A `canary testing deployment method`. Suitable if you want to test the health of your new application version using a portion of incoming traffic, while keeping the rest of the traffic served by the old application version.


|     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Deployment methods                      |
| **Method** | **Impact of failed deployment** | **Deploy time** | **Zero downtime** | **No DNS change** | ****Rollback process**** | ******Code deployed to****** |
| **All at once** | Downtime | 🕛  | No  | Yes | Manual redeploy | Existing instances |
| **Rolling** | Single batch out of service; any successful batches before failure running new application version | 🕛🕛† | Yes | Yes | Manual redeploy | Existing instances |
| **Rolling with an additional batch** | Minimal if first batch fails; otherwise, similar to **Rolling** | 🕛🕛🕛<br><br>† | Yes | Yes | Manual redeploy | New and existing instances |
| **Immutable** | Minimal | 🕛🕛🕛🕛 | Yes | Yes | Terminate new instances | New instances |
| **Traffic splitting** | Percentage of client traffic routed to new version temporarily impacted | 🕛🕛🕛🕛<br><br>†† | Yes | Yes | Reroute traffic and terminate new instances | New instances |
| **Blue/green** | Minimal | 🕛🕛🕛🕛 | Yes | No  | Swap URL | New instances |

† Varies depending on batch size.

†† Varies depending on evaluation time option setting.


## How rolling deployments work

When processing a batch, Elastic Beanstalk detaches all instances in the batch from the load balancer, deploys the new application version, and then reattaches the instances. If you enable connection draining, Elastic Beanstalk drains existing connections from the Amazon EC2 instances in each batch before beginning the deployment.

After reattaching the instances in a batch to the load balancer, Elastic Load Balancing waits until they pass a minimum number of Elastic Load Balancing health checks (the Healthy check count threshold value), and then starts routing traffic to them. If no health check URL is configured, this can happen very quickly, because an instance will pass the health check as soon as it can accept a TCP connection. If a health check URL is configured, the load balancer doesn't route traffic to the updated instances until they return a `200 OK` status code in response to an `HTTP GET` request to the health check URL.

Elastic Beanstalk waits until all instances in a batch are healthy before moving on to the next batch. With basic health reporting, instance health depends on the Elastic Load Balancing health check status. When all instances in the batch pass enough health checks to be considered healthy by Elastic Load Balancing, the batch is complete. If enhanced health reporting is enabled, Elastic Beanstalk considers several other factors, including the result of incoming requests. With enhanced health reporting, all instances must pass 12 consecutive health checks with an OK status within two minutes for web server environments, and 18 health checks within three minutes for worker environments.

If a batch of instances does not become healthy within the command timeout, the deployment fails. After a failed deployment, check the health of the instances in your environment for information about the cause of the failure. Then perform another deployment with a fixed or known good version of your application to roll back.

`If a deployment fails after one or more batches completed successfully`, the completed batches run the new version of your application while any pending batches `continue to run the old version.` You can identify the version running on the instances in your environment on the health page in the console. This page displays the `deployment ID of the most recent deployment that executed on each instance in your environment`. If you terminate instances from the failed deployment, Elastic Beanstalk replaces them with instances running the application version from the most recent successful deployment.

## How traffic-splitting deployments work

Traffic-splitting deployments allow you to perform canary testing. You direct some incoming client traffic to your new application version to verify the application's health before committing to the new version and directing all traffic to it.

During a traffic-splitting deployment, Elastic Beanstalk creates a new set of instances in a separate temporary Auto Scaling group. Elastic Beanstalk then instructs the load balancer to direct a certain percentage of your environment's incoming traffic to the new instances. Then, for a configured amount of time, Elastic Beanstalk tracks the health of the new set of instances. If all is well, Elastic Beanstalk shifts remaining traffic to the new instances and attaches them to the environment's original Auto Scaling group, replacing the old instances. Then Elastic Beanstalk cleans up—terminates the old instances and removes the temporary Auto Scaling group.

> **Note**
>
>The environment's capacity doesn't change during a traffic-splitting deployment. Elastic Beanstalk launches the same number of instances in the temporary Auto Scaling group as there are in the original Auto Scaling group at the time the deployment starts. It then maintains a constant number of instances in both Auto Scaling groups for the deployment duration. Take this fact into account when configuring the environment's traffic splitting evaluation time.

Rolling back the deployment to the previous application version is quick and doesn't impact service to client traffic. If the new instances don't pass health checks, or if you choose to abort the deployment, Elastic Beanstalk moves traffic back to the old instances and terminates the new ones. You can abort any deployment by using the environment overview page in the Elastic Beanstalk console, and choosing **Abort current operation** in **Environment actions**. You can also call the `AbortEnvironmentUpdate` API or the equivalent AWS CLI command.

Traffic-splitting deployments require an Application Load Balancer. Elastic Beanstalk uses this load balancer type by default when you create your environment using the Elastic Beanstalk console or the EB CLI.

## Blue/Green deployments with Elastic Beanstalk
Because AWS Elastic Beanstalk performs an in-place update when you update your application versions, your application might become unavailable to users for a short period of time. To avoid this, perform a blue/green deployment. To do this, deploy the new version to a separate environment, and then swap the CNAMEs of the two environments to redirect traffic to the new version instantly.

A blue/green deployment is also required if you want to update an environment to an incompatible platform version. For more information, see [Updating your Elastic Beanstalk environment's platform version](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.platform.upgrade.html).

Blue/green deployments require that your environment runs independently of your production database, if your application uses one. If your environment includes a database that Elastic Beanstalk created on your behalf, the database and connection of the environment isn't preserved unless you take specific actions. If you have a database that you want to retain, use one of the Elastic Beanstalk [database lifecycle](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.managing.db.html#environments-cfg-rds-lifecycle) options. You can choose the Retain option to keep the database and environment operational after decoupling the database. For more information see Database lifecycle in the Configuring environments chapter of this guide. 

## Configuring application version lifecycle settings
Each time you `upload a new version of your application `with the Elastic Beanstalk console or the EB CLI, Elastic Beanstalk `creates an application version`. If you don't delete versions that you no longer use, you will eventually reach the **application version quota** and be unable to create new versions of that application.

You can avoid hitting the quota by applying an application version lifecycle policy to your applications. A lifecycle policy tells Elastic Beanstalk to delete application versions that are old, or to delete application versions when the total number of versions for an application exceeds a specified number.

Elastic Beanstalk applies an application's lifecycle policy each time you create a new application version, and deletes up to 100 versions each time the lifecycle policy is applied. Elastic Beanstalk deletes old versions after creating the new version, and does not count the new version towards the maximum number of versions defined in the policy.

![app-version](./imgOne/app-version-lifecycle.png)

Elastic Beanstalk does not delete application versions that are currently being used by an environment, or application versions deployed to environments that were terminated less than ten weeks before the policy was triggered.

The application version quota applies across all applications in a region. If you have several applications, configure each application with a lifecycle policy appropriate to avoid reaching the quota. For example, if you have 10 applications in a region and the quota is 1,000 application versions, consider setting a lifecycle policy with a quota of 99 application versions for all applications, or set other values in each application as long as the total is less than 1,000 application versions. Elastic Beanstalk only applies the policy if the application version creation succeeds, so if you have already reached the quota, you must delete some versions manually prior to creating a new version.

By default, Elastic Beanstalk leaves the application version's [source bundle](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/applications-sourcebundle.html) in Amazon S3 to prevent loss of data. You can delete the source bundle to save space.

You can set the lifecycle settings through the Elastic Beanstalk CLI and APIs. See [eb appversion](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-appversion.html), CreateApplication (using the `ResourceLifecycleConfig` parameter), and [UpdateApplicationResourceLifecycle](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateApplicationResourceLifecycle.html) for details.

## Advanced environment customization with configuration files (.ebextensions)

You can add AWS Elastic Beanstalk configuration files (`.ebextensions`) to your web application's source code to configure your environment and customize the AWS resources that it contains. Configuration files are YAML- or JSON-formatted documents with a `.config` file extension that you place in a folder named `.ebextensions` and deploy in your application source bundle.

>**Example**
>
>**.ebextensions/network-load-balancer.config**

This example makes a simple configuration change. It modifies a configuration option to set the type of your environment's load balancer to Network Load Balancer.

``` yaml
option_settings:
    aws:elasticbeanstalk:environment:
        LoadBalancerType: network
```

We recommend using YAML for your configuration files, because it's more readable than JSON. YAML supports comments, multi-line commands, several alternatives for using quotes, and more. However, you can make any configuration change in Elastic Beanstalk configuration files identically using either YAML or JSON.

>Tip
>
>When you are developing or testing new configuration files, launch a clean environment running the default application and deploy to that. Poorly formatted configuration files will cause a new environment launch to fail unrecoverably.

## Clone an Elastic Beanstalk environment

You can use an existing Elastic Beanstalk environment as the basis for a new environment by cloning the existing environment. For example, you might want to create a clone so that you can use a newer version of the platform branch used by the original environment's platform. Elastic Beanstalk configures the clone with the same environment settings used by the original environment. By cloning an existing environment instead of creating a new environment, you don't have to manually configure option settings, environment variables, and other settings. `Elastic Beanstalk also creates a copy of any AWS resource associated with the original environment. However, during the cloning process, Elastic Beanstalk doesn't copy data from Amazon RDS to the clone.` After you create the clone environment, you can modify environment configuration settings as needed.

You can only clone an environment to a different platform version of the same platform branch. A different platform branch isn't guaranteed to be compatible. To use a different platform branch, you have to manually create a new environment, deploy your application code, and make any necessary changes in code and options to ensure your application works correctly on the new platform branch.

> Note
>
>Elastic Beanstalk doesn't include any unmanaged changes to resources in the clone. Changes to AWS resources that you make using tools other than the Elastic Beanstalk console, command-line tools, or API are considered unmanaged changes.

## General migration process

When you're ready to go to production, Elastic Beanstalk requires a `blue/green deployment` to perform the upgrade. The following are the general best practice steps that we recommend for migration with a blue/green deployment procedure.

### Preparing to test for your migration

Before you deploy your application and start testing, review the information in the prior section [Differences and compatibility](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.generic.from-al2.html#using-features.migration-al.generic.from-al2.differences). Also review the reference cited in that section, [Comparing Amazon Linux 2 and Amazon Linux 2023](https://docs.aws.amazon.com/linux/al2023/ug/compare-with-al2.html) in the *Amazon Linux 2023 User Guide.* Make a note of the specific information from this content that applies or may apply to your application and configuration set up.

### High level migration steps

1. Create a new environment that's based on an AL2023 platform branch.

2. Deploy your application to the target AL2023 environment.

    Your existing production environment will remain active and unaffected, while you iterate through testing and making adjustments to the new environment.

3. Test your application thoroughly in the new environment.

4. When your destination AL2023 environment is ready to go to production, swap the CNAMEs of the two environments to redirect traffic to the new AL2023 environment.


## Using Elastic Beanstalk with Amazon RDS



- Create a new database in Amazon RDS.

- Start with a database that was previously [created by Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.managing.db.html) and subsequently [decoupled](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.managing.db.html#using-features.decoupling.db) from a Beanstalk environment. For more information, see [Adding a database to your Elastic Beanstalk environment](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.managing.db.html).


You can use either approach to run a database instance in Amazon RDS and configure your application to connect to it on launch. You can connect multiple environments to a database and also perform seamless updates with blue-green deployments.

***
***
