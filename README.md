# AWS High Availability Web Architecture

## Project Overview

This project demonstrates the design and deployment of a highly available AWS web architecture using:

- Custom VPC
- Public and private subnets
- Internet Gateway
- NAT Gateway
- Route tables
- Security groups
- EC2 application servers
- Application Load Balancer
- Target groups
- Multi-AZ deployment

The project also includes failover and resilience testing by intentionally simulating infrastructure failures.

---

## Architecture

```text
Internet
   ↓
Application Load Balancer (Public Subnets)
   ↓
EC2 Application Servers (Private Subnets)
```

---

## AWS Services Used

- Amazon VPC
- Amazon EC2
- Application Load Balancer (ALB)
- Target Groups
- Security Groups
- Route Tables
- NAT Gateway
- Internet Gateway

---

## Networking Design

### Public Subnets
Used for:
- Application Load Balancer
- NAT Gateway

### Private Subnets
Used for:
- EC2 application servers

### Internet Gateway
Allows public internet access for public resources.

### NAT Gateway
Allows private resources to access the internet outbound while remaining private.

---

## Security Design

### LoadBalancer-SG
- Allowed inbound HTTP traffic from the internet
- Forwarded traffic to application servers

### AppServer-SG
- Allowed inbound HTTP traffic only from the load balancer security group
- Prevented direct public access to application servers

---

## High Availability Design

- Deployed EC2 instances across multiple Availability Zones
- Configured an Application Load Balancer
- Used health checks for automatic failover

---

## Experiments Performed

### Experiment 1 — Single Server Failure
Stopped one application server and observed:
- Target health changes
- Automatic traffic redistribution

### Experiment 2 — Security Group Failure
Removed HTTP access from the application server security group and observed:
- Failed health checks
- Targets marked unhealthy

### Experiment 3 — NAT Route Removal
Removed the NAT Gateway route from the private route table and observed:
- Loss of outbound internet access from private resources
- Internal VPC communication remained functional

### Experiment 4 — Total Backend Failure
Stopped both application servers and observed:
- Load balancer inability to route traffic
- Service becoming unavailable

---

## Key Concepts Learned

- Public vs private subnet architecture
- VPC routing behavior
- NAT Gateway functionality
- Load balancing
- Health checks
- High availability
- Traffic flow through AWS infrastructure
- Security group behavior
- Failover mechanisms

---

## Future Improvements

- Add Amazon RDS database layer
- Implement HTTPS with SSL/TLS certificates
- Configure Auto Scaling
- Add monitoring with CloudWatch
- Introduce Infrastructure as Code (Terraform)

## Infrastructure as Code

After manually building and testing the architecture, I recreated the core infrastructure using Terraform.

Terraform manages:

- VPC
- Public and private subnets
- Internet Gateway
- NAT Gateway
- Route tables
- Security groups
- Application Load Balancer
- Target group
- Launch template
- Auto Scaling Group

This demonstrates the ability to define repeatable cloud infrastructure as code rather than relying only on manual AWS Console configuration.