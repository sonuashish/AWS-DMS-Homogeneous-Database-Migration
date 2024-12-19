# AWS-DMS-Homogeneous-Database-Migration
To learn how to implement homogeneous database migration across VPCs in AWS.To learn how to implement homogeneous database migration across VPCs in AWS.routing between them using private IPv4 or IPv6 addresses.
# Lab Guide on DMS: Homogeneous Database Migration

## Objective
To learn how to implement homogeneous database migration across VPCs in AWS.

## Background
This exercise demonstrates how to set up and migrate data between two MySQL databases (source: AWS EC2 MySQL DB server, target: AWS RDS MySQL DB server) in separate VPCs using AWS Database Migration Service (DMS). By default, VPCs cannot communicate with each other, so VPC peering will be used to enable routing between them using private IPv4 or IPv6 addresses.

## Problem Description
- The source endpoint (AWS EC2 MySQL DB server) and target endpoint (AWS RDS MySQL DB server) are in two separate VPCs.
- These VPCs cannot communicate with each other by default.
- VPC peering will be used to enable communication.

## Solution
We will create two VPCs and configure them for homogeneous database migration.

---

### Step 1: Set Up the VPCs and Internet Gateway
1. Go to the AWS Management Console and click on **VPC**.
2. In the VPC Dashboard, click **Start VPC Wizard**.
3. In the **VPC Configuration** window, select the **VPC with a Single Public Subnet** option.
4. Provide a name for the VPC, select **Availability Zone a**, and click **Create VPC**.
5. Once the VPC is successfully created, you will see a confirmation message.

#### Verify VPC Components
- In the navigation pane, click **Your VPCs** to view the details of the created VPC.
- Check the **Internet Gateways** section for the internet gateways associated with the default VPC and the newly created VPC.

#### Verify Routing Tables
- Navigate to **Route Tables** in the VPC dashboard.
- Select the relevant route table and click the **Routes** tab to verify the details.

### Step 2: Create the Second VPC
1. Create another VPC with a public subnet.
2. Use a non-overlapping IPv4 CIDR block range (e.g., 11.0.0.0/16) to avoid conflicts.
3. Select the same **Availability Zone a** as the first VPC.
4. Create a new subnet in this VPC for the RDS instance, ensuring a non-overlapping IPv4 CIDR block range (e.g., 11.0.1.0/24).
5. Verify the route tables for the second VPC.

---

### Step 3: Launch the Source Endpoint (AWS EC2 MySQL DB Server)
1. Navigate to the **EC2 Dashboard**.
2. Launch an instance, selecting **MySQL-compatible AMI**.
3. In the **Step 3: Configure Instance** section, choose the VPC and subnet associated with the first VPC.
4. Complete the instance launch process.

### Step 4: Launch the Target Endpoint (AWS RDS MySQL DB Server)
1. Navigate to the **RDS Dashboard**.
2. Create an RDS instance with MySQL engine.
3. Configure the RDS instance to use the second VPC and its associated subnet group.
4. Ensure the RDS instance is in **Availability Zone a**.
5. Launch the RDS instance.

---

### Step 5: Set Up VPC Peering
1. Navigate to **Peering Connections** in the VPC dashboard.
2. Click **Create Peering Connection**.
   - Provide a name tag.
   - Choose the first VPC as the **Requester VPC** and the second VPC as the **Accepter VPC**.
3. Click **Create Peering Connection**.

#### Accept the Peering Request
1. Go to the **Peering Connections** page.
2. Select the created peering connection and click **Actions** > **Accept Request**.
3. Confirm by clicking **Yes, Accept**.

---

### Step 6: Update Route Tables
1. Update the route table for the first VPC:
   - Add a route for the IPv4 CIDR block of the second VPC.
   - Set the target to the VPC peering connection.
2. Update the route table for the second VPC:
   - Add a route for the IPv4 CIDR block of the first VPC.
   - Set the target to the VPC peering connection.

---

### Step 7: Configure Security Groups
1. Update the security group for the EC2 instance in the first VPC:
   - Allow inbound traffic from the CIDR block of the second VPC for MySQL (port 3306).
2. Update the security group for the RDS instance in the second VPC:
   - Allow inbound traffic from the CIDR block of the first VPC for MySQL (port 3306).

---

### Step 8: Configure AWS DMS
1. Navigate to the **DMS Dashboard**.
2. Create a replication instance in one of the VPCs.
3. Create source and target endpoints for the EC2 MySQL server and RDS MySQL server, respectively.
4. Configure and start the migration task.

---

### Final Verification
- Test connectivity between the EC2 instance and RDS instance.
- Verify that data has been migrated successfully using AWS DMS.

This completes the lab guide for homogeneous database migration using AWS DMS across VPCs.

Here are the key skills gained that I can list from this project:

**AWS Networking Skills:**
VPC configuration and management.
Setting up and configuring VPC Peering.
Route table updates for cross-VPC traffic.
Subnet creation and management.
Security group configuration for database communication.

**AWS Database Skills:**
Deploying and managing RDS MySQL instances.
Configuring MySQL on EC2 instances.
Understanding and creating DB Subnet Groups.

**AWS Migration Skills:**
Using AWS Database Migration Service (DMS) for database replication.
Configuring source and target database endpoints.
Setting up and managing DMS replication instances and migration tasks.

**General Cloud Skills:**
Managing IPv4 CIDR blocks to avoid conflicts.
Troubleshooting connectivity issues in cloud environments.
Hands-on experience with AWS EC2, RDS, and DMS services.

**Problem-Solving and Implementation Skills:**
Enabling secure cross-VPC communication.
End-to-end database migration planning and execution.
Resolving challenges related to routing and private IP-based communication.

These skills demonstrate expertise in AWS services, networking, database management, and cloud migrations, making them highly relevant for cloud-based roles like Data Engineer or Cloud Architect.

