---
title: "Complete AWS VPC Setup Guide"
date: 2025-08-06
categories: [AWS, Networking, Cloud]
tags: [aws, vpc, networking, cloud, guide]
image: assets/images/images.png
---

# Complete AWS VPC Setup Guide

## Table of Contents
1. Introduction
2. Prerequisites
3. VPC Architecture Overview
4. Step-by-Step VPC Setup
5. Advanced Configurations
6. Best Practices
7. Troubleshooting

## Introduction

Amazon Virtual Private Cloud (VPC) allows you to create an isolated network within AWS where you can launch AWS resources in a virtual network that you define. This guide will walk you through creating a production-ready VPC with public and private subnets across multiple availability zones.

## Prerequisites

- AWS Account with appropriate permissions
- Basic understanding of networking concepts (CIDR, subnets, routing)
- AWS CLI installed (optional but recommended)
- IAM permissions for VPC operations

## VPC Architecture Overview

We'll create a VPC with the following architecture:
- **VPC CIDR**: 10.0.0.0/16 (65,536 IP addresses)
- **2 Availability Zones** for high availability
- **2 Public Subnets** (10.0.1.0/24 and 10.0.2.0/24)
- **2 Private Subnets** (10.0.11.0/24 and 10.0.12.0/24)
- **Internet Gateway** for public subnet internet access
- **NAT Gateways** for private subnet internet access
- **Route Tables** for traffic routing


## Step-by-Step VPC Setup

### Step 1: Create the VPC

#### Using AWS Console:
1. Navigate to **VPC Dashboard** in AWS Console
2. Click **"Create VPC"**
3. Configure:
   - **Name tag**: MyProductionVPC
   - **IPv4 CIDR block**: 10.0.0.0/16
   - **IPv6 CIDR block**: No IPv6 CIDR block (unless needed)
   - **Tenancy**: Default
4. Click **"Create VPC"**

#### Using AWS CLI:
```bash
aws ec2 create-vpc \
    --cidr-block 10.0.0.0/16 \
    --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=MyProductionVPC}]'
```

### Step 2: Enable DNS Resolution and DNS Hostnames

#### Using AWS Console:
1. Select your VPC
2. Click **"Actions"** → **"Edit DNS resolution"**
3. Enable **"DNS resolution"**
4. Click **"Actions"** → **"Edit DNS hostnames"**
5. Enable **"DNS hostnames"**

#### Using AWS CLI:
```bash
# Get VPC ID from previous command
VPC_ID="vpc-xxxxxxxxx"

aws ec2 modify-vpc-attribute \
    --vpc-id $VPC_ID \
    --enable-dns-support

aws ec2 modify-vpc-attribute \
    --vpc-id $VPC_ID \
    --enable-dns-hostnames
```

### Step 3: Create Subnets

#### Create Public Subnets:

**Public Subnet 1 (AZ-1a):**
```bash
aws ec2 create-subnet \
    --vpc-id $VPC_ID \
    --cidr-block 10.0.1.0/24 \
    --availability-zone us-east-1a \
    --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=Public-Subnet-1a}]'
```

**Public Subnet 2 (AZ-1b):**
```bash
aws ec2 create-subnet \
    --vpc-id $VPC_ID \
    --cidr-block 10.0.2.0/24 \
    --availability-zone us-east-1b \
    --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=Public-Subnet-1b}]'
```

#### Create Private Subnets:

**Private Subnet 1 (AZ-1a):**
```bash
aws ec2 create-subnet \
    --vpc-id $VPC_ID \
    --cidr-block 10.0.11.0/24 \
    --availability-zone us-east-1a \
    --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=Private-Subnet-1a}]'
```

**Private Subnet 2 (AZ-1b):**
```bash
aws ec2 create-subnet \
    --vpc-id $VPC_ID \
    --cidr-block 10.0.12.0/24 \
    --availability-zone us-east-1b \
    --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=Private-Subnet-1b}]'
```

### Step 4: Create and Attach Internet Gateway

#### Create Internet Gateway:
```bash
aws ec2 create-internet-gateway \
    --tag-specifications 'ResourceType=internet-gateway,Tags=[{Key=Name,Value=MyVPC-IGW}]'
```

#### Attach to VPC:
```bash
IGW_ID="igw-xxxxxxxxx"

aws ec2 attach-internet-gateway \
    --vpc-id $VPC_ID \
    --internet-gateway-id $IGW_ID
```

### Step 5: Create NAT Gateways

#### Allocate Elastic IPs:
```bash
# For NAT Gateway 1
aws ec2 allocate-address \
    --domain vpc \
    --tag-specifications 'ResourceType=elastic-ip,Tags=[{Key=Name,Value=NAT-GW-EIP-1}]'

# For NAT Gateway 2
aws ec2 allocate-address \
    --domain vpc \
    --tag-specifications 'ResourceType=elastic-ip,Tags=[{Key=Name,Value=NAT-GW-EIP-2}]'
```

#### Create NAT Gateways:
```bash
# NAT Gateway in Public Subnet 1
aws ec2 create-nat-gateway \
    --subnet-id $PUBLIC_SUBNET_1_ID \
    --allocation-id $EIP_ALLOCATION_ID_1 \
    --tag-specifications 'ResourceType=nat-gateway,Tags=[{Key=Name,Value=NAT-GW-1a}]'

# NAT Gateway in Public Subnet 2
aws ec2 create-nat-gateway \
    --subnet-id $PUBLIC_SUBNET_2_ID \
    --allocation-id $EIP_ALLOCATION_ID_2 \
    --tag-specifications 'ResourceType=nat-gateway,Tags=[{Key=Name,Value=NAT-GW-1b}]'
```

### Step 6: Configure Route Tables

#### Create Route Tables:

**Public Route Table:**
```bash
aws ec2 create-route-table \
    --vpc-id $VPC_ID \
    --tag-specifications 'ResourceType=route-table,Tags=[{Key=Name,Value=Public-RT}]'
```

**Private Route Tables:**
```bash
# Private Route Table 1
aws ec2 create-route-table \
    --vpc-id $VPC_ID \
    --tag-specifications 'ResourceType=route-table,Tags=[{Key=Name,Value=Private-RT-1a}]'

# Private Route Table 2
aws ec2 create-route-table \
    --vpc-id $VPC_ID \
    --tag-specifications 'ResourceType=route-table,Tags=[{Key=Name,Value=Private-RT-1b}]'
```

#### Add Routes:

**Public Route Table - Route to Internet Gateway:**
```bash
aws ec2 create-route \
    --route-table-id $PUBLIC_RT_ID \
    --destination-cidr-block 0.0.0.0/0 \
    --gateway-id $IGW_ID
```

**Private Route Tables - Routes to NAT Gateways:**
```bash
# Private RT 1 to NAT GW 1
aws ec2 create-route \
    --route-table-id $PRIVATE_RT_1_ID \
    --destination-cidr-block 0.0.0.0/0 \
    --nat-gateway-id $NAT_GW_1_ID

# Private RT 2 to NAT GW 2
aws ec2 create-route \
    --route-table-id $PRIVATE_RT_2_ID \
    --destination-cidr-block 0.0.0.0/0 \
    --nat-gateway-id $NAT_GW_2_ID
```

#### Associate Route Tables with Subnets:

**Public Subnets:**
```bash
# Associate Public RT with Public Subnet 1
aws ec2 associate-route-table \
    --route-table-id $PUBLIC_RT_ID \
    --subnet-id $PUBLIC_SUBNET_1_ID

# Associate Public RT with Public Subnet 2
aws ec2 associate-route-table \
    --route-table-id $PUBLIC_RT_ID \
    --subnet-id $PUBLIC_SUBNET_2_ID
```

**Private Subnets:**
```bash
# Associate Private RT 1 with Private Subnet 1
aws ec2 associate-route-table \
    --route-table-id $PRIVATE_RT_1_ID \
    --subnet-id $PRIVATE_SUBNET_1_ID

# Associate Private RT 2 with Private Subnet 2
aws ec2 associate-route-table \
    --route-table-id $PRIVATE_RT_2_ID \
    --subnet-id $PRIVATE_SUBNET_2_ID
```

### Step 7: Configure Security Groups

#### Create Security Groups:

**Web Server Security Group (Public):**
```bash
aws ec2 create-security-group \
    --group-name WebServerSG \
    --description "Security group for web servers" \
    --vpc-id $VPC_ID

# Add rules
aws ec2 authorize-security-group-ingress \
    --group-id $WEB_SG_ID \
    --protocol tcp \
    --port 80 \
    --cidr 0.0.0.0/0

aws ec2 authorize-security-group-ingress \
    --group-id $WEB_SG_ID \
    --protocol tcp \
    --port 443 \
    --cidr 0.0.0.0/0

aws ec2 authorize-security-group-ingress \
    --group-id $WEB_SG_ID \
    --protocol tcp \
    --port 22 \
    --cidr YOUR_IP/32  # Replace with your IP
```

**Application Server Security Group (Private):**
```bash
aws ec2 create-security-group \
    --group-name AppServerSG \
    --description "Security group for application servers" \
    --vpc-id $VPC_ID

# Allow traffic from Web SG
aws ec2 authorize-security-group-ingress \
    --group-id $APP_SG_ID \
    --protocol tcp \
    --port 8080 \
    --source-group $WEB_SG_ID
```

**Database Security Group (Private):**
```bash
aws ec2 create-security-group \
    --group-name DatabaseSG \
    --description "Security group for databases" \
    --vpc-id $VPC_ID

# Allow traffic from App SG
aws ec2 authorize-security-group-ingress \
    --group-id $DB_SG_ID \
    --protocol tcp \
    --port 3306 \
    --source-group $APP_SG_ID
```

### Step 8: Create Network ACLs (Optional)

Network ACLs provide an additional layer of security at the subnet level:

```bash
# Create custom NACL for private subnets
aws ec2 create-network-acl \
    --vpc-id $VPC_ID \
    --tag-specifications 'ResourceType=network-acl,Tags=[{Key=Name,Value=Private-NACL}]'

# Add inbound rules
aws ec2 create-network-acl-entry \
    --network-acl-id $NACL_ID \
    --rule-number 100 \
    --protocol tcp \
    --rule-action allow \
    --cidr-block 10.0.0.0/16 \
    --port-range From=0,To=65535

# Add outbound rules
aws ec2 create-network-acl-entry \
    --network-acl-id $NACL_ID \
    --rule-number 100 \
    --protocol -1 \
    --rule-action allow \
    --cidr-block 0.0.0.0/0 \
    --egress
```

## Advanced Configurations

### VPC Peering
Connect two VPCs to allow resources to communicate:
```bash
aws ec2 create-vpc-peering-connection \
    --vpc-id $VPC_ID \
    --peer-vpc-id $PEER_VPC_ID \
    --peer-region us-west-2
```

### VPN Connection
Set up a Site-to-Site VPN for hybrid cloud connectivity:
1. Create Customer Gateway
2. Create Virtual Private Gateway
3. Attach to VPC
4. Create VPN Connection
5. Configure routing

### VPC Endpoints
Create endpoints for AWS services to avoid internet routing:
```bash
# S3 Gateway Endpoint
aws ec2 create-vpc-endpoint \
    --vpc-id $VPC_ID \
    --service-name com.amazonaws.us-east-1.s3 \
    --route-table-ids $PRIVATE_RT_1_ID $PRIVATE_RT_2_ID
```

### Flow Logs
Enable VPC Flow Logs for network monitoring:
```bash
aws ec2 create-flow-logs \
    --resource-type VPC \
    --resource-ids $VPC_ID \
    --traffic-type ALL \
    --log-destination-type cloud-watch-logs \
    --log-group-name /aws/vpc/flowlogs
```

## Best Practices

1. **CIDR Planning**:
   - Use RFC 1918 private IP ranges
   - Plan for future growth
   - Avoid overlapping CIDRs if planning VPC peering

2. **High Availability**:
   - Always use multiple Availability Zones
   - Deploy NAT Gateways in each AZ
   - Distribute resources across AZs

3. **Security**:
   - Follow the principle of least privilege
   - Use Security Groups as primary security layer
   - Implement NACLs for additional subnet-level security
   - Enable VPC Flow Logs

4. **Cost Optimization**:
   - Use NAT Instances instead of NAT Gateways for dev/test
   - Delete unused Elastic IPs
   - Right-size your subnets

5. **Naming Conventions**:
   - Use consistent, descriptive names
   - Include environment (prod/dev/test)
   - Include purpose and location

6. **Documentation**:
   - Document your network architecture
   - Maintain an IP address allocation spreadsheet
   - Keep track of security group rules

## Troubleshooting

### Common Issues:

1. **Cannot connect to EC2 instance**:
   - Check Security Group rules
   - Verify route table associations
   - Ensure instance has public IP (for public subnet)
   - Check NACL rules

2. **Private instances cannot reach internet**:
   - Verify NAT Gateway is running
   - Check private route table has route to NAT Gateway
   - Ensure NAT Gateway is in public subnet

3. **Cannot resolve DNS**:
   - Enable DNS resolution and DNS hostnames on VPC
   - Check DHCP options set

### Useful Commands for Debugging:
```bash
# List all VPC resources
aws ec2 describe-vpcs
aws ec2 describe-subnets --filters "Name=vpc-id,Values=$VPC_ID"
aws ec2 describe-route-tables --filters "Name=vpc-id,Values=$VPC_ID"
aws ec2 describe-security-groups --filters "Name=vpc-id,Values=$VPC_ID"

# Check connectivity
aws ec2 describe-vpc-peering-connections
aws ec2 describe-vpn-connections
```

## Cleanup

To avoid charges, delete resources in reverse order:
```bash
# 1. Terminate EC2 instances
# 2. Delete NAT Gateways
# 3. Release Elastic IPs
# 4. Detach and delete Internet Gateway
# 5. Delete subnets
# 6. Delete route tables
# 7. Delete security groups
# 8. Delete VPC
```

## Conclusion

This guide provides a comprehensive approach to setting up a production-ready VPC in AWS. Remember to adapt the configuration based on your specific requirements and always follow AWS best practices for security and cost optimization.
