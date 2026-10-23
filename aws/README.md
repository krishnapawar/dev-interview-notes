# ☁️ AWS (Amazon Web Services) — Interview Questions & Answers

A comprehensive, production-oriented AWS interview preparation guide covering cloud compute, storage, IAM security, networking, VPC architecture, serverless, and deployment strategies for Full-Stack & Backend Developers.

---

## 📋 Table of Contents

1. [What Is the Difference Between EC2 and AWS Lambda?](#1-what-is-the-difference-between-ec2-and-aws-lambda)
2. [What Is Amazon S3 and What Are Its Storage Classes?](#2-what-is-amazon-s3-and-what-are-its-storage-classes)
3. [How Does IAM Work (Users, Roles, and Policies)?](#3-how-does-iam-work-users-roles-and-policies)
4. [Difference Between Application Load Balancer (ALB) and Network Load Balancer (NLB)](#4-difference-between-application-load-balancer-alb-and-network-load-balancer-nlb)
5. [How Does Auto Scaling Work in AWS?](#5-how-does-auto-scaling-work-in-aws)
6. [What Is the Difference Between Amazon CloudWatch and AWS CloudTrail?](#6-what-is-the-difference-between-amazon-cloudwatch-and-aws-cloudtrail)
7. [What Is Amazon CloudFront and How Does CDN Caching Work?](#7-what-is-amazon-cloudfront-and-how-does-cdn-caching-work)
8. [Difference Between Amazon RDS and Amazon DynamoDB](#8-difference-between-amazon-rds-and-amazon-dynamodb)
9. [What Is an AWS VPC and How Do Public vs Private Subnets Work?](#9-what-is-an-aws-vpc-and-how-do-public-vs-private-subnets-work)
10. [How Do You Securely Deploy a Production Node.js and React Application on AWS?](#10-how-do-you-securely-deploy-a-production-nodejs-and-react-application-on-aws)

---

## 1. What Is the Difference Between EC2 and AWS Lambda?

> **Interview-Ready Answer:**  
> Amazon EC2 provides resizable virtual server instances with full OS-level control where you manage scaling, patching, and runtime environments; AWS Lambda is a serverless event-driven compute engine that runs code automatically in response to triggers and scales to zero, where you pay strictly per millisecond of execution.

| Feature | Amazon EC2 | AWS Lambda |
| :--- | :--- | :--- |
| **Model** | Infrastructure as a Service (IaaS) | Function as a Service (FaaS) / Serverless |
| **Server Management** | You manage OS, patching, packages, scaling | Fully managed by AWS (no OS access) |
| **Scaling** | Auto Scaling Groups (takes minutes to boot) | Instant automatic concurrency (milliseconds) |
| **Pricing** | Paid per hour/second while running | Paid strictly per execution invocation & duration |
| **Execution Limit** | None (runs continuously 24/7) | Maximum 15 minutes timeout per invocation |
| **State** | Stateful (local disk EBS/instance store) | Stateless (ephemeral container storage) |

**When to Choose Which:**
- Use **EC2** for long-running processes, WebSocket servers, persistent TCP connections, heavy monolithic apps, and legacy software.
- Use **Lambda** for sporadic workloads, webhooks, asynchronous background image processing, S3 file triggers, and lightweight REST microservices.

---

## 2. What Is Amazon S3 and What Are Its Storage Classes?

> **Interview-Ready Answer:**  
> Amazon Simple Storage Service (S3) is an object storage service offering industry-leading 99.999999999% (11 9s) durability, designed to store and protect any amount of unstructured data (media files, backups, static web assets) using REST APIs.

**Core S3 Storage Classes:**

| Storage Class | Durability / Availability | Access Frequency | Retrieval Fee | Typical Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **S3 Standard** | 99.999999999% / 99.99% | Frequent | None | Active user uploads, static website assets |
| **S3 Intelligent-Tiering** | 99.999999999% / 99.9% | Unknown / Dynamic | None | Automatic cost optimization without access delays |
| **S3 Standard-IA** | 99.999999999% / 99.9% | Infrequent | Yes (per GB) | Monthly backups, disaster recovery data |
| **S3 Glacier Flexible** | 99.999999999% / 99.99% | Rare (archives) | Yes | Long-term compliance logs (retrieval: minutes to hours) |
| **S3 Glacier Deep Archive** | 99.999999999% / 99.99% | Rare (years) | Yes (lowest storage cost) | Multi-year regulatory compliance archives (retrieval: 12h) |

**Generating a Presigned URL for Secure S3 Uploads (Node.js SDK v3):**
```javascript
import { S3Client, PutObjectCommand } from "@aws-sdk/client-s3";
import { getSignedUrl } from "@aws-sdk/s3-request-presigner";

const s3 = new S3Client({ region: "us-east-1" });

export async function getUploadUrl(key, contentType) {
  const command = new PutObjectCommand({
    Bucket: "my-app-uploads",
    Key: key,
    ContentType: contentType,
  });

  // Client uploads directly to S3 without burdening backend bandwidth
  return await getSignedUrl(s3, command, { expiresIn: 300 });
}
```

---

## 3. How Does IAM Work (Users, Roles, and Policies)?

> **Interview-Ready Answer:**  
> AWS Identity and Access Management (IAM) controls authentication and authorization across AWS resources using **Users** (permanent credentials for people), **Roles** (temporary credentials for AWS services or federated identities), and **Policies** (JSON documents defining explicit Allow/Deny permissions).

**The 3 Core IAM Building Blocks:**
1. **IAM User:** Represents an individual person or service account with long-term credentials (password, access key/secret key).
2. **IAM Role:** An identity with temporary security tokens granted via AWS Security Token Service (STS). Used by EC2 instances, Lambda functions, or ECS tasks without hardcoding secret keys.
3. **IAM Policy:** A JSON document following the principle of least privilege:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowUserS3UploadsOnly",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::my-production-bucket/avatars/*"
    }
  ]
}
```

> ⚠️ **Best Practice:** Never hardcode AWS Access Keys in codebase or EC2 instances. Attach an **IAM Instance Profile Role** directly to the EC2 server or Lambda function.

---

## 4. Difference Between Application Load Balancer (ALB) and Network Load Balancer (NLB)

> **Interview-Ready Answer:**  
> Application Load Balancer (ALB) operates at Layer 7 (HTTP/HTTPS) and routes traffic intelligently based on URL path, host header, or query parameters; Network Load Balancer (NLB) operates at Layer 4 (TCP/UDP) and handles tens of millions of requests per second with ultra-low latency and static IP support.

| Feature | Application Load Balancer (ALB) | Network Load Balancer (NLB) |
| :--- | :--- | :--- |
| **OSI Layer** | Layer 7 (Application) | Layer 4 (Transport) |
| **Protocols** | HTTP, HTTPS, HTTP/2, WebSockets, gRPC | TCP, UDP, TLS |
| **Routing Decisions** | Path-based (`/api`), Host-based (`admin.app.com`) | IP address and TCP/UDP port |
| **Performance** | High throughput with rich inspection | Ultra-high performance, ultra-low sub-millisecond latency |
| **Static IP Address** | ❌ No (assigned dynamic DNS name) | ✅ Yes (assigns Elastic IPs per subnet) |
| **Typical Use Case** | Microservices, REST APIs, WebSockets | Gaming servers, IoT data ingestion, high-scale TCP proxies |

---

## 5. How Does Auto Scaling Work in AWS?

> **Interview-Ready Answer:**  
> AWS Auto Scaling monitors application metrics (CPU utilization, request count per target) and automatically adjusts the number of EC2 instances or container tasks between configured minimum, desired, and maximum limits to maintain steady performance at lowest cost.

**Core Components of EC2 Auto Scaling:**
1. **Launch Template:** Defines what to launch (AMI ID, instance type, security groups, IAM role, user-data shell scripts).
2. **Auto Scaling Group (ASG):** Defines where and how many to launch (VPC subnets, Min: 2, Desired: 4, Max: 10).
3. **Scaling Policies:**
   - **Target Tracking:** Maintains average CPU utilization at 65%.
   - **Step Scaling:** Adds 2 instances if CPU > 75%, adds 4 if CPU > 90%.
   - **Scheduled Scaling:** Pre-scales before known peak hours (e.g. Black Friday sale).

```text
Traffic Surge → CloudWatch Alarm Triggers → ASG Launches New EC2 → ALB Health Checks Pass → Traffic Distributed
```

---

## 6. What Is the Difference Between Amazon CloudWatch and AWS CloudTrail?

> **Interview-Ready Answer:**  
> Amazon CloudWatch monitors application and infrastructure performance, metrics, alarms, and logs (CPU, RAM, API latencies); AWS CloudTrail audits account governance and compliance by recording all API calls, console actions, and CLI commands (who did what, when, and from where).

| Feature | Amazon CloudWatch | AWS CloudTrail |
| :--- | :--- | :--- |
| **Primary Purpose** | Performance monitoring & real-time observability | Security auditing, governance & compliance |
| **Tracks** | CPU %, disk I/O, network traffic, application logs | API calls, user logins, SDK actions, resource changes |
| **Questions Answered** | *"Is my EC2 instance healthy? Why is API latency spiking?"* | *"Who deleted the production S3 bucket or modified IAM role?"* |
| **Actionable Triggers** | Alarms trigger Auto Scaling or SNS alerts | EventBridge triggers on suspicious unauthorized API calls |

---

## 7. What Is Amazon CloudFront and How Does CDN Caching Work?

> **Interview-Ready Answer:**  
> Amazon CloudFront is a globally distributed Content Delivery Network (CDN) that caches static and dynamic web content at hundreds of Edge Locations worldwide, reducing latency by serving responses physically closer to end users.

**How CloudFront Caching Works:**
1. User in London requests `https://app.com/bundle.js`.
2. DNS routes user to the nearest London CloudFront Edge Location.
3. **Cache Hit:** If the file is in Edge cache, CloudFront delivers it instantly (~10ms).
4. **Cache Miss:** CloudFront fetches the asset from the **Origin** (S3 bucket or ALB EC2 in `us-east-1`), returns it to the user, and caches it locally for the duration defined by `Cache-Control: max-age=31536000, immutable`.

**Invalidating Caches on New Deployments:**
```bash
# Invalidate index.html to force users to fetch the new React build instantly
aws cloudfront create-invalidation --distribution-id E1A2B3C4D5E6F7 --paths "/index.html"
```

---

## 8. Difference Between Amazon RDS and Amazon DynamoDB

> **Interview-Ready Answer:**  
> Amazon RDS is a managed relational database service supporting MySQL, PostgreSQL, and MariaDB with ACID transactions, foreign keys, and complex SQL joins; Amazon DynamoDB is a fully managed NoSQL key-value and document database that provides single-digit millisecond latency at virtually any scale without provisioning servers.

| Feature | Amazon RDS | Amazon DynamoDB |
| :--- | :--- | :--- |
| **Database Type** | Relational SQL (Postgres, MySQL) | NoSQL Key-Value & Document |
| **Schema** | Rigid schema with relational constraints | Schema-less (only Partition Key & Sort Key required) |
| **Scaling** | Vertical scaling (bigger instance) + Read Replicas | Horizontal partition scaling automatically |
| **Complex Joins** | ✅ Native SQL joins and aggregations | ❌ No joins (requires denormalized data design) |
| **Transactions** | Full ACID transactions | ACID support via `TransactWriteItems` |
| **Best For** | Financial ledgers, complex reporting, ERP | High-scale user sessions, shopping carts, gaming leaderboards |

---

## 9. What Is an AWS VPC and How Do Public vs Private Subnets Work?

> **Interview-Ready Answer:**  
> An Amazon Virtual Private Cloud (VPC) is a logically isolated virtual network dedicated to your AWS account. **Public subnets** have a direct route to an Internet Gateway (IGW) for public-facing load balancers, while **Private subnets** isolate backend databases and servers from the internet, accessing external APIs only via a one-way NAT Gateway.

```text
[ Internet ]
     │
[ Internet Gateway ]
     │
┌────▼──────────────────────────────────────────────────┐
│ Public Subnet                                          │
│   [ Application Load Balancer ]   [ NAT Gateway ]      │
└────┬───────────────────────────────────┬───────────────┘
     │ Internal Traffic                  │ Outbound Only
┌────▼───────────────────────────────────▼──────────────┐
│ Private Subnet                                         │
│   [ Node.js EC2 / ECS Tasks ] ──► [ RDS Database ]    │
└───────────────────────────────────────────────────────┘
```

**Security Best Practice:**
- Place databases (RDS, DynamoDB endpoints, Redis) strictly inside **Private Subnets** with zero public IP addresses.
- Configure Security Groups so that the RDS database only accepts inbound traffic on port 5432/3306 originating from the App Server Security Group.

---

## 10. How Do You Securely Deploy a Production Node.js and React Application on AWS?

> **Interview-Ready Answer:**  
> Deploy a production full-stack application by hosting the React frontend on Amazon S3 distributed via Amazon CloudFront with SSL/TLS certificates from AWS Certificate Manager (ACM), while running the Node.js backend on EC2 or ECS containers behind an Application Load Balancer inside private subnets connected to an RDS database with automated backups and CloudWatch alarms.

**Production Deployment Architecture Blueprint:**

1. **Frontend Hosting (React/Vite):**
   - Build assets (`npm run build`) and sync to an **Amazon S3** private bucket.
   - Configure **CloudFront CDN** with Origin Access Control (OAC) so S3 is inaccessible directly.
   - Attach free wildcard SSL certificate via **AWS Certificate Manager (ACM)**.
   - Point Route 53 DNS alias record to CloudFront distribution.

2. **Backend API Hosting (Node.js/Express):**
   - Containerize app with **Docker** and push images to **Amazon ECR**.
   - Run containers via **Amazon ECS (Fargate)** or **EC2 Auto Scaling** across 2 Availability Zones.
   - Place servers in **Private Subnets** behind an **Application Load Balancer (ALB)** in Public Subnets.
   - Manage secrets and database passwords securely using **AWS Secrets Manager** or **SSM Parameter Store** instead of `.env` files on disk.

3. **Database & Caching:**
   - **Amazon RDS (Multi-AZ PostgreSQL/MySQL)** with automated daily snapshots and encrypted storage.
   - **Amazon ElastiCache (Redis)** for user session tokens and hot API caching.

4. **Continuous Integration & Deployment (CI/CD):**
   - GitHub Actions runner uses OIDC (IAM Role without permanent keys) to push build artifacts to S3 and trigger zero-downtime rolling ECS deployments.
