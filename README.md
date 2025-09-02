# AWS EC2 + S3 + IAM + ALB Secure Web Hosting Lab

Securely host and serve static web content from S3 via EC2 instances (with Nginx), IAM instance roles, and an Application Load Balancer.

## Includes

- **S3 Bucket**: Private static files (logo, CSS). No public access.
- **IAM Role**: EC2 instance profile with fine-grained S3 read policy.
- **EC2 Instances**: Amazon Linux 2/2023, with Nginx auto-installed (via UserData), loads content from S3 at launch.
- **Application Load Balancer**: Public HTTP endpoint distributes requests across EC2 instances for HA & auto IP rotation.
- **Security Groups**: Only necessary ports (80, 22) open.
- **UserData Script**: Automates Nginx, fetches S3 assets, generates dynamic HTML with server identity.

## Steps

1. Provision a private S3 bucket. Upload `logo.png` and `style.css`. Block all public access.
2. Create IAM Role for EC2 with `AmazonS3ReadOnlyAccess`.
3. Launch two EC2 t3.micro instances with the above IAM Role and UserData (see `/scripts/userdata.sh` for automation). Assign proper security group.
4. Create an Application Load Balancer, register both EC2 instances, open port 80 to the public (for lab/demo).
5. Access your site via the ALB DNS. Each refresh should show hostname and static assets from S3, securely delivered.

## Directory Structure

- `scripts/`: UserData, deploy, and cleanup scripts.
- `s3-assets/`: Example `logo.png` and `style.css`.
- `docs/`: Markdown instructions and architecture diagram.

