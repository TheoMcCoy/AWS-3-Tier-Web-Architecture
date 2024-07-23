# AWS 3-Tier Web Architecture

This repository contains a comprehensive guide and implementation of a 3-tier web architecture on Amazon Web Services (AWS). The architecture is designed to provide scalability, high availability, and security for web applications.

## Architecture Overview

The 3-tier architecture consists of:

1. **Web Tier**: Handles incoming HTTP/HTTPS requests, serves static content, and forwards requests to the application tier.
2. **App Tier**: Processes business logic and interacts with the database tier for data operations.
3. **Database Tier**: Stores and manages application data with high availability and read scalability.

![AWS 3-Tier Architecture](https://github.com/TheoMcCoy/AWS-3-Tier-Web-Architecture/blob/main/3TierArch.png)

### Components

- **Virtual Private Cloud (VPC)**: Network isolation for the entire architecture.
- **Internet Gateway**: Enables internet access to the VPC.
- **Elastic Load Balancer (ELB)**: Distributes incoming traffic across web servers.
- **Amazon EC2 Instances**: Hosts for web and application servers.
- **Amazon Aurora**: Managed relational database service with high availability and read replicas.
- **Public Subnets**: Hosts web servers accessible from the internet.
- **Private Subnets**: Hosts application and database servers, isolated from direct internet access.

## Detailed Architecture Workflow

1. **Client Requests**: Clients send HTTP/HTTPS requests through the internet, which are routed to the Internet Gateway.
2. **Load Balancing**: The Internet Gateway forwards the traffic to the Elastic Load Balancer, which distributes the requests across the web servers in the public subnets.
3. **Web Servers**: The web servers handle the requests and, if necessary, forward them to the application servers in the private subnets.
4. **Application Servers**: The application servers process the business logic and interact with the database instances in the private subnets.
5. **Database Operations**: The application servers perform read/write operations on the Aurora Primary DB, while read requests can also be served by the Aurora Read Replica to enhance performance.
6. **Response**: The processed data is sent back through the application and web servers to the clients.

## Prerequisites

- AWS Account
- Basic knowledge of AWS services (EC2, ELB, VPC, RDS)
- AWS CLI configured with appropriate permissions

## Deployment Instructions

1. Clone this repository to your local machine:
   ```bash
   git clone https://github.com/yourusername/aws-3-tier-architecture.git
   cd aws-3-tier-architecture

2. Configure your AWS environment using the AWS CLI:
   ```bash
   aws configure
3. Deploy the CloudFormation template to set up the architecture:
   ```bash
   aws cloudformation create-stack --stack-name my-3tier-architecture --template-body file://cloudformation-template.yaml

4. Monitor the deployment process and wait for the stack creation to complete:
   ```bash
   aws cloudformation describe-stacks --stack-name my-3tier-architecture

5. Access your web application via the ELB DNS name provided in the CloudFormation stack outputs.

## Cleanup
   To clean up the resources created by this architecture, delete the CloudFormation stack:
```bash
aws cloudformation delete-stack --stack-name my-3tier-architecture



