# TechX AWS Load Balancing Assignment

This project sets up two Ubuntu EC2 instances running `nginx` behind an Application Load Balancer.

## What I Built

- `web-1` returns `Server 1`
- `web-2` returns `Server 2`
- an internet-facing ALB routes traffic to both instances
- the target group health check path is `/`

## Basic Setup

1. Launched 2 Ubuntu EC2 instances.
2. Installed `nginx` on both instances.
3. Updated each server's `index.html` with different content.
4. Created a target group on port `80` with health check path `/`.
5. Created an Application Load Balancer and attached both instances through the target group.
6. Stopped one instance to confirm the ALB still served traffic through the healthy instance.

## Files

- `commands.txt` contains the commands used during setup

## Submission Info

- ALB DNS: `http://web-alb-1125866445.us-east-2.elb.amazonaws.com`
- GitHub repo: `https://github.com/agc000/techx-ec2.git`
