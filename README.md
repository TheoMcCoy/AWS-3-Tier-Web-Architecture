# AWS 3-Tier Web Architecture

This repository contains a comprehensive guide and implementation of a 3-tier web architecture on Amazon Web Services (AWS). The architecture is designed to provide scalability, high availability, and security for web applications.

## Architecture Overview

The 3-tier architecture consists of:

- In this architecture:
  - A public-facing Application Load Balancer forwards client traffic to the web tier EC2 instances.
  - The web tier:
    - Runs Nginx web servers configured to:
      - Serve a React.js website.
      - Redirect API calls to the application tier’s internal-facing load balancer.
  - The internal-facing load balancer:
    - Forwards traffic to the application tier.
  - The application tier:
    - Is written in Node.js.
    - Manipulates data in an Aurora MySQL multi-AZ database.
    - Returns data to the web tier.
  - Load balancing, health checks, and autoscaling groups are created at each layer to maintain the availability of this architecture.

![AWS 3-Tier Architecture](https://github.com/TheoMcCoy/AWS-3-Tier-Web-Architecture/blob/main/3TierArch.png)

## Components

- **Virtual Private Cloud (VPC)**: Network isolation for the entire architecture.
- **Internet Gateway**: Enables internet access to the VPC.
- **Public-facing Application Load Balancer**: Distributes incoming client traffic across web servers.
- **Amazon EC2 Instances**: Hosts for web servers running Nginx and application servers running Node.js.
- **Nginx Webservers**: Serve a React.js website and redirect API calls to the internal load balancer.
- **Internal-facing Load Balancer**: Distributes API traffic across application servers.
- **Amazon Aurora**: Managed relational database service with high availability and read replicas.
- **Public Subnets**: Host web servers accessible from the internet.
- **Private Subnets**: Host application and database servers, isolated from direct internet access.
- **Load Balancing, Health Checks, and Autoscaling Groups**: Implemented at each layer to maintain high availability and fault tolerance.

## Detailed Architecture Workflow

1. **Client Requests**: Clients send HTTP/HTTPS requests through the Internet, routed to the Internet Gateway.
2. **Public-facing Application Load Balancer**: The Internet Gateway forwards the traffic to the public-facing Application Load Balancer, which distributes the requests across the web servers in the public subnets.
3. **Web Servers (Nginx)**:
    - Handle the requests and serve static content for the React.js website.
    - Redirect API calls to the application tier’s internal-facing load balancer.
4. **Internal-facing Load Balancer**: Forwards the redirected API traffic to the application servers in the private subnets.
5. **Application Servers (Node.js)**:
    - Process the API requests and perform business logic.
    - Interact with the Aurora MySQL multi-AZ database for data manipulation.
6. **Database Operations**: The application servers perform read/write operations on the Aurora Primary DB, while read requests can also be served by the Aurora Read Replica to enhance performance.
7. **Response**: The processed data is sent back through the application servers to the web servers, returning the response to the clients.
8. **High Availability and Fault Tolerance**: Load balancing, health checks, and autoscaling groups are configured at each layer (web tier, application tier, and database tier) to ensure continuous availability and fault tolerance of the architecture.


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



