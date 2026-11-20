# AWS Multi-Tier Web Application Deployment

A production-grade multi-tier web application deployed on AWS with automated CI/CD pipeline.

## Architecture Overview

**Frontend Layer**
- Application Load Balancer (HTTPS port 443, SSL certificate configured)
- Routes traffic to application servers in Auto Scaling Group

**Application Layer**
- Tomcat servers on EC2 instances
- Auto Scaling Group across 2 Availability Zones
- Scales out at CPU > 70%, scales in at CPU < 30%

**Backend Layer**
- MySQL on EC2 (private subnet) — primary database
- Memcached on EC2 — query result caching to reduce DB load
- RabbitMQ on EC2 — async message queue between app components

## CI/CD Pipeline (Jenkins)

```
Git Push → Webhook Trigger → Jenkins Pipeline
→ Stage 1: Git Checkout
→ Stage 2: Maven Build
→ Stage 3: Upload artifact to S3
→ Stage 4: SSH deploy to Tomcat servers
```

## AWS Services Used

| Service | Purpose |
|--------|---------|
| EC2 | App servers, DB, cache, MQ |
| ALB | Load balancing, SSL termination |
| Auto Scaling | High availability |
| Route 53 | Private DNS for backend services |
| S3 | Artifact storage |
| CloudWatch | Monitoring and alerts |
| VPC | Network isolation |
| Security Groups | Tier-level access control |

## Security Design

- Each backend service in its own Security Group
- App tier can only talk to backend — no direct internet access
- Private subnets for all backend services
- SSL/TLS on ALB

## Tech Stack

- **Cloud:** AWS
- **CI/CD:** Jenkins, Maven
- **Web Server:** Apache Tomcat
- **Database:** MySQL
- **Cache:** Memcached
- **Messaging:** RabbitMQ
- **DNS:** Route 53 Private Hosted Zone
- **OS:** Linux (Ubuntu)
- **Scripting:** Shell Script
