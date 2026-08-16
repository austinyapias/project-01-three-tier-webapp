# Project 1: Production Three-Tier Web Application on AWS

## Architecture

![Architecture Diagram](docs/architecture.svg)

Multi-AZ deployment across us-east-1a and us-east-1b with layered security.

## What I Built

- VPC with 6 subnets (2 public, 4 private) across 2 Availability Zones
- Application Load Balancer (internet-facing, HTTP)
- Auto Scaling Group (min 2, max 6 instances, CPU-based scaling)
- RDS PostgreSQL in private subnet — zero public access
- Layered security groups enforcing defense in depth
- Session Manager access only — no SSH, no exposed ports

## High Availability Proof

I terminated a running instance to simulate failure. Auto Scaling detected it and launched a replacement in 5 seconds. Zero downtime for end users.

## Security Controls

| Control | Implementation |
|---------|---------------|
| No public DB access | RDS in private subnet, publicly accessible = No |
| Least-privilege networking | Each tier only accepts traffic from tier above |
| No SSH | Port 22 closed, SSM Session Manager only |
| Security group references | Dynamic rules, no hardcoded IPs |

## Services Used

VPC, EC2, ALB, Auto Scaling, RDS PostgreSQL, IAM, Systems Manager, Route Tables, NAT Gateway, Internet Gateway

## Cost

~\$3/day when running | Full teardown in 5 minutes

## Screenshots

See [docs/screenshots/](docs/screenshots/) for:
- VPC and subnet layout
- Security group rules
- ALB with healthy targets
- Auto Scaling activity (self-healing)
- Session Manager connection
- Live application

## Lessons Learned

1. NAT Gateways cost more than the EC2 instances (~\$32/mo) — cost awareness matters
2. Security group references > CIDR blocks — dynamic and resilient
3. Building manually before automating teaches you what Terraform is actually doing
4. RDS takes 10+ minutes to provision — start it first, build other components while waiting
5. Always check which VPC the console defaults to — it burned me multiple times

## Resume Bullet

> Architected and deployed a production-grade three-tier web application on AWS featuring Multi-AZ high availability, Auto Scaling (2–6 instances), ALB traffic distribution, and private-subnet RDS — validated self-healing by simulating instance failure with 5-second recovery and zero user-facing downtime.

## Next

Project 2: Rebuild this entire stack in Terraform with a single command.
