# Cluster VPC Considerations<a name="network_reqs"></a>

When you create an Amazon EKS cluster, you specify the Amazon VPC subnets for your cluster to use\. Amazon EKS requires subnets in at least two Availability Zones\. We recommend a network architecture that uses private subnets for your worker nodes and public subnets for Kubernetes to create internet\-facing load balancers within\. When you create your cluster, specify all of the subnets that will host resources for your cluster \(such as worker nodes and load balancers\)\.

**Note**  
Internet\-facing load balancers require a public subnet in your cluster\.

The subnets that you pass when you create the cluster influence where Amazon EKS places elastic network interfaces that are used for the control plane to worker node communication\.

It is possible to specify only public or private subnets when you create your cluster, but there are some limitations associated with these configurations:
+ **Private\-only**: Everything runs in a private subnet and Kubernetes cannot create internet\-facing load balancers for your pods\.
+ **Public\-only**: Everything runs in a public subnet, including your worker nodes\.

Amazon EKS creates an elastic network interface in your private subnets to facilitate communication to your worker nodes\. This communication channel supports Kubernetes functionality such as kubectl exec and kubectl logs\. The security group that you specify when you create your cluster is applied to the elastic network interfaces that are created for your cluster control plane\.

## VPC Tagging Requirement<a name="vpc-tagging"></a>

When you create your Amazon EKS cluster, Amazon EKS tags the VPC containing the subnets you specify in the following way so that Kubernetes can discover it:


| Key | Value | 
| --- | --- | 
|  `kubernetes.io/cluster/<cluster-name>`  |  `shared`  | 
+ **Key**: The *<cluster\-name>* value matches your Amazon EKS cluster's name\. 
+ **Value**: The `shared` value allows more than one cluster to use this VPC\.

## Subnet Tagging Requirement<a name="vpc-subnet-tagging"></a>

When you create your Amazon EKS cluster, Amazon EKS tags the subnets you specify in the following way so that Kubernetes can discover them:


| Key | Value | 
| --- | --- | 
| `kubernetes.io/cluster/<cluster-name>` | `shared` | 
+ **Key**: The *<cluster\-name>* value matches your Amazon EKS cluster\. 
+ **Value**: The `shared` value allows more than one cluster to use this subnet\.