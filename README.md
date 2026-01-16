# Aws-Assignment

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






