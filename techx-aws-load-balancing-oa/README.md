# Distributed Systems Basics - Load Balancing on AWS

## Minimal Repo Structure

This repo only needs:

- `README.md` - setup steps and submission notes
- `commands.txt` - exact commands used during setup

## Goal

Set up two Ubuntu EC2 instances running `nginx`, place them behind an AWS Application Load Balancer, and verify failover by stopping one instance while traffic continues through the ALB.

## Architecture

- EC2 Instance 1: Ubuntu + `nginx` + page content `Server 1`
- EC2 Instance 2: Ubuntu + `nginx` + page content `Server 2`
- Application Load Balancer: internet-facing
- Target Group: both EC2 instances registered
- Health Check Path: `/`

## AWS Resources Used

- 2 Ubuntu EC2 instances
- 1 Application Load Balancer
- 1 Target Group
- Security groups allowing:
  - `22` from my IP for SSH
  - `80` from anywhere for HTTP

## Setup Steps

### 1. Launch EC2 Instances

Create two Ubuntu EC2 instances in the AWS Console.

- AMI: Ubuntu Server
- Instance type: `t2.micro` or `t3.micro`
- Count: `2`
- Same VPC
- Different subnets if possible
- Attach a security group with:
  - inbound `SSH (22)` from my IP
  - inbound `HTTP (80)` from `0.0.0.0/0`

### 2. Install and Configure `nginx`

SSH into each instance and run the commands from `commands.txt`.

Instance 1 should return:

```html
<h1>Server 1</h1>
```

Instance 2 should return:

```html
<h1>Server 2</h1>
```

### 3. Verify Each Instance Directly

Open each EC2 public IP in the browser and confirm:

- Instance 1 shows `Server 1`
- Instance 2 shows `Server 2`

### 4. Create Target Group

In the AWS Console:

- Create a target group for instances
- Protocol: `HTTP`
- Port: `80`
- Health check path: `/`
- Register both EC2 instances

Wait until both targets show `healthy`.

### 5. Create the Application Load Balancer

In the AWS Console:

- Type: `Application Load Balancer`
- Scheme: `internet-facing`
- Listener: `HTTP` on port `80`
- Select at least two subnets / AZs
- Attach a security group allowing inbound `HTTP (80)`
- Forward traffic to the target group created above

### 6. Test Load Balancing

Open the ALB DNS name in the browser and refresh multiple times.

Expected result:

- You should see responses from both servers over repeated refreshes

Note:

- Depending on browser behavior, you may need a hard refresh or use `curl` repeatedly to observe alternating responses.

### 7. Reliability / Failover Check

Temporarily stop one EC2 instance in the AWS Console.

Then:

- wait for the stopped instance to fail the target group health check
- open the ALB DNS again
- confirm traffic still works through the healthy instance

Expected result:

- The ALB continues serving traffic from the remaining healthy server

## Commands Used

All commands used for setup are recorded in [commands.txt](/Users/ac/techx-aws-load-balancing-oa/commands.txt).

## Submission Details

### ALB DNS URL

`PASTE-ALB-DNS-HERE`

Example:

`http://my-load-balancer-1234567890.us-east-1.elb.amazonaws.com`

### GitHub Repository

`PASTE-GITHUB-REPO-LINK-HERE`

### Demo Recording

The demo should show:

- both EC2 instances configured and serving different pages
- the ALB serving traffic
- one instance being stopped
- the ALB continuing to serve traffic from the remaining healthy instance

## Result

This setup demonstrates basic AWS load balancing and fault tolerance using two `nginx` web servers behind an Application Load Balancer.
