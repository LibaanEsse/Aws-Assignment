# AWS Assignment

## Overview
This assignment demonstrates the design and implementation of a fully custom AWS network built from the ground up. The objective is to gain hands-on experience with core cloud networking concepts, including VPC design, subnetting, routing, controlled internet access, and instance monitoring.In addition, the assignment showcases a common DevOps architecture pattern in which multiple EC2 instances are deployed behind an Application Load Balancer (ALB). The goal is to understand how load balancing, health checks, security group isolation, HTTPS termination, DNS integration, and automated scaling work together to create a highly available application architecture.
All inbound traffic is routed through the ALB, ensuring that backend EC2 instances are not directly accessible from the public internet. Two EC2 instances are deployed behind the ALB, each configured to serve different content to validate proper load balancing behavior. 

## Assignment 1- AWS Custom VPC with Secure Public & Private Subnet with Internet and Internal EC2 Access
## 📚 What Was Learned
A fully custom network environment was deployed using AWS VPC.  
Core knowledge gained:

- Designing public and private subnets  

-  Internet access using IGW and NAT Gateway  

- Route table management for correct traffic flow  

- Secure internal-only communication for private servers 

- Importance of network segmentation in cloud architecture

- How a bastion host enables secure administrative access

- How CloudWatch can be used to validate infrastructure behaviour

## Architecture
The architecture is built around a single VPC with both public and private subnets. An Internet Gateway allows inbound access to resources in the public subnet, while a NAT Gateway provides outbound-only internet access for instances in the private subnet.

A public EC2 instance serves as a bastion host, enabling secure SSH access to the private EC2 instance.
<img src="Images/vpc-architecture.png"></img> 

### 1. VPC Creation
- Created a custom VPC with CIDR block `10.0.0.0/16`
- Created one public subnet
- Created one private subnet

A VPC (Virtual Private Cloud) gives you complete control over your cloud network, including IP addressing, routing, and security. It acts as your own private network in which you can deploy and manage instances, similar to how networks are structured in real-world architectures.

- Using a /16 CIDR block provides more than 65,000 private IP addresses, allowing your network to scale in the future without redesign. This ensures high scalability for growth or additional resources.

- A Public Subnet hosts resources that need to be accessible from the internet. Examples include EC2 instances running web services or acting as bastion hosts for SSH access. Resources in the public subnet connect to the internet via an Internet Gateway, enabling secure inbound and outbound communication.

- A Private Subnet contains resources that should remain hidden from the public internet, such as internal applications or sensitive backend services. Instances here can access the internet only through a NAT Gateway for tasks like software updates, while preventing any direct inbound connections from outside AWS. This provides a secure environment for private workloads.

- ### 2. Internet Access
- Created and attached an Internet Gateway to the VPC
- Allocated an Elastic IP
- Created a NAT Gateway in the public subnet using the Elastic IP

The Internet Gateway allows public instances to communicate with the internet, while the NAT Gateway lets private instances make outbound requests without exposing them to inbound traffic. This design keeps public resources accessible for management and testing, while private instances remain secure and isolated. An Elastic IP provides a static public IP for resources like NAT Gateways or public EC2 instances. It ensures consistent connectivity, allowing private instances to access the internet reliably and public instances to be reachable for management or users. 

### 3. Route Tables
- Public route table configured with a default route (`0.0.0.0/0`) to the Internet Gateway
- Private route table configured with a default route (`0.0.0.0/0`) to the NAT Gateway
- Each route table associated with its respective subnet

Route tables control how traffic moves inside and outside a VPC.  
Using two route tables ensures:

- Public instances can be reached from the internet when needed  
- Private instances stay isolated from direct external access  
- Private instances can still make outbound requests safely  

This setup maintains strong security while supporting necessary cloud operations.

### EC2 Instances
- Launched a public EC2 instance in the public subnet with a public IPv4 address
- Launched a private EC2 instance in the private subnet without a public IPv4 address
- EC2 user data scripts were used to automate instance bootstrapping
 

- Public User Date
```
     #!/bin/bash

yum update -y
yum install -y httpd

systemctl start httpd
systemctl enable httpd 

echo "<h1>Hello from public EC2v1 from $(hostname -f)</h1>" > /var/www/html/index.html
```
- Private user data 
```
#!/bin/bash

yum update -y
yum install -y httpd

systemctl start httpd
systemctl enable httpd 

echo "<h1>Hello from private EC2v1 from $(hostname -f)</h1>" > /var/www/html/index.html
```
Deploying instances in public and private subnets shows how secure multi-tier cloud architectures work. Public systems are accessible but controlled, while private systems stay protected from external threats.

 ### 5. Security
- Public EC2 security group allows SSH and HTTP access only from my IP
- Private EC2 security group allows inbound access only from within the VPC (via the bastion host)
- No direct internet access to the private EC2

  ### 6. Bastion Host
The public EC2 instance serves a dual purpose: it functions not only as a standard compute instance but also as a bastion host, providing a secure and controlled gateway for accessing the private EC2 instance. By acting as an intermediary, the bastion host allows administrators and authorized users to establish SSH connections to the private instance without requiring a direct public IP or exposing the private instance to the internet. This setup enhances security by restricting direct external access, ensuring that all management and maintenance activities pass through a single, monitored point of entry. In addition, it enables logging and auditing of SSH connections, providing greater visibility and control over who accesses the private instance and when.

### 7. CloudWatch Monitoring
Detailed CloudWatch monitoring was activated on both EC2 instances to track and validate resource usage and network activity.

This included:

CPU utilization metrics to monitor processing load and performance

Network in/out metrics to measure the volume of data transmitted and received

Packet count metrics to assess the number of network packets flowing to and from the instances

CloudWatch metrics were analyzed to confirm:

Inbound traffic reaching the public EC2 instance, ensuring the bastion host was accessible

Outbound traffic from the private EC2 instance through the NAT Gateway, verifying internet connectivity for the private subnet

Normal instance behavior after deployment, indicating that both instances were operating within expected performance and network parameters

  ## Assignment Images 
 You can view all project images which includes a detailed step by tutorial here: [View all images](https://github.com/LibaanEsse/Aws-Assignment/tree/main/Images)






