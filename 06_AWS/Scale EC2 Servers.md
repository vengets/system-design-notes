---
created: 2026-06-22 11:49
updated:
tags:
  - aws
status: active
service-category: EC2
source:
related:
---


> [!abstract]
> AWS services should be understood through architecture positioning and trade-offs, not memorization.

## Purpose

#### Scale EC2 Servers

1. Create Target Group under EC2 service, (TG1)
	1. Providing Instance type, Subnets that it operates
	2. Health check endpoints
2. Create Load Balancer (LB1)
	1. Select Application Load balancer
	2. Scheme, Ip address type selection
	3. Choose VPC and AZ mappings (should select at least 2 AZ, can select only 1 subnet in each AZ)
	4. Add Listeners and routing (add TG1 here)
3. Create AutoScaling group (ASG1)
	1. Choose launch template (create one and use it) that has instance type (t2.micro for ex)
	2. Select VPC and AZ subnets
	3. Choose Load Balancer (LB1)
	4. Next, Group size, desired capacity = 2
	5. Scaling min = 2, max = 4
	6. If you dont want static scaling, create Dynamic scaling policy
	7. policy type Target tracking scaling
	8. Metric can be CPU Utilization, N/w In, N/w out counts, Avg Load Balancer req counts
	9. Choose Target Value, instance warmup and create.
4. To check , Go to EC2 -> Load balancers, copy DNS name against Name, open it in browser



## Mental Model

## Core Features

## Architecture Positioning

## Scaling Characteristics

## Security Considerations

## IAM Considerations

## Networking Implications

## Cost Considerations

## HA / DR Considerations

## Alternatives

| Service | Difference |
|---|---|
|  |  |

## Real-World Usage

## Interview Questions

## Related Notes

- [[ ]]

## Revision Notes
