# HA & SCALING

## Load Balancing

Problem:\
If using R53 with health checks, there's caching so user will keep pinging downed server. updating dns is slow

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.21.07 PM.png>)

bob is connected to lb

client thinks it's connected to server directly

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.22.51 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.23.39 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.24.53 PM.png>)

## Application Load Balancer

ALB inspects data that passes through it, ie: headers, paths, hosts

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.27.37 PM.png>)

### Architecture

one LB per AZ

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.30.17 PM.png>)

![cross-zone load balancing](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.30.39 PM.png>)

LB distributes to loads across all AZs

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.31.28 PM.png>)

one target can be in multiple groups

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.32.45 PM.png>)

target can be any aws compute resource (lambda, ec2, etc)

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.34.42 PM.png>)

CLB can only use one SSL cert

## Launch config and launch templates (use templates, it's superset of config)

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.36.43 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 10.37.25 PM.png>)

## Auto scaling group

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 11.17.34 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 11.18.10 PM.png>)

attempt to keep no of instances in each az equal

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 11.20.43 PM.png>)

cooling: scaling doesn't occur until certain time has passed from last one

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 11.21.48 PM.png>)

self-healing: if min is set to 1, sick ec2 instance is terminated and new one is provisioned

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 11.24.21 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 11.24.06 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 11.25.09 PM.png>)

## Demo

![](<../../../.gitbook/assets/Screenshot 2021-07-23 at 11.26.13 PM.png>)



![](<../../../.gitbook/assets/Screenshot 2021-07-24 at 10.55.37 AM (1).png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-24 at 10.56.21 AM (1).png>)



![](<../../../.gitbook/assets/Screenshot 2021-07-24 at 10.57.22 AM (1).png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-24 at 10.57.02 AM (2).png>)

* then create launch template
* userdata: step one

## Network load balancer

high performance but no understanding of http/s

![](<../../../.gitbook/assets/Screenshot 2021-07-24 at 1.49.22 PM.png>)

## SSL offload & session stickiness

![](<../../../.gitbook/assets/Screenshot 2021-07-24 at 2.05.25 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-24 at 2.06.45 PM.png>)
