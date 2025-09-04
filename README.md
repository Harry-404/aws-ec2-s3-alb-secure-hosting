# AWS EC2 + S3 + ALB Secure Web Hosting Lab

A hands-on AWS lab for securely serving **static web content** using S3 (private assets), EC2, IAM Roles, and an Application Load Balancer (ALB). The setup is optimized for security, automation, and scalable web delivery for internal/private or public use cases.

***

## Architecture Overview

- **S3 Bucket**: Stores private static files (images, CSS)—no public access.
- **IAM Role**: Granted to EC2, allows controlled S3 bucket access.
- **EC2 Instances**: Auto-install and configure Nginx, fetch content from S3 at launch.
- **Application Load Balancer (ALB)**: Handles public HTTP requests, distributes to EC2.
- **Security Groups**: Only ports 80 (HTTP) and 22 (SSH) as required.
- **UserData Script**: Bootstraps content pulling and web server setup on instance creation.

***

## Step-by-Step Guide

### 1. Setup S3 Bucket (Private)

- Create a bucket and **block all public access**.
- Upload content files (e.g., `logo.png`, `style.css`).
- No static website hosting required unless explicitly needed.

### 2. Create an IAM Role for EC2

- Attach AmazonS3ReadOnlyAccess policy (or a custom, least-privilege policy).
- Assign this role when launching EC2 instances.

### 3. Launch EC2 Instances (Amazon Linux 2/2023)

- Use the UserData script (see `/scripts/userdata.sh`) to:
  - Install Nginx
  - Pull assets from S3 using AWS CLI
  - Place files in the Nginx web root
- Attach the IAM role created in the previous step.
- Use dedicated **security group** allowing inbound SSH (22) from your IP and HTTP (80) from ALB or trusted sources.

### 4. Deploy Application Load Balancer

- Configure **ALB** to forward requests on port 80 to your EC2 instances.
- Make ALB publicly accessible if needed.
- Register the EC2 instances as targets in the ALB's target group.
- ALB health checks can be set to `/` with protocol HTTP.

### 5. Access Your Website

- Use the **ALB DNS** endpoint in your browser.
- Static assets load securely from S3 via EC2 (not direct from S3).
- On each refresh, you may see different EC2 instance hostnames if scaling with more than one instance.

***

## Security & Best Practices

- **S3 Bucket**: Use bucket policies denying public access and requiring HTTPS (`aws:SecureTransport`).[1]
- **IAM Roles**: Grant only the necessary permissions (prefer custom over broad managed policies).
- **EC2**: No public access to S3, everything fetched using IAM role from inside VPC.
- **ALB & Security Groups**: Only allow needed ports. Restrict EC2 to only ALB (reference ALB’s security group in EC2 inbound rules).[2]
- **PrivateLink/VPC Endpoints** (optional for advanced use): For even tighter S3 access without internet routes.[3][4]
- **Logging & Monitoring**: Enable ALB access logs and monitor S3 server access logs.
- **Automation**: Use UserData scripts and CloudFormation/Terraform for repeatable infrastructure.

***



## Connect

<a href="https://www.linkedin.com/in/hiranmaya-biswas-505a1823a/" target="_blank">
  <img src="https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin" alt="LinkedIn" height="30">
</a>
<a href="https://github.com/Harry-404" target="_blank" style="margin-left:10px;">
  <img src="https://img.shields.io/badge/GitHub-Follow-black?logo=github" alt="GitHub" height="30">
</a>

