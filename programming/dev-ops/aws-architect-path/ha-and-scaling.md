# HA & SCALING

## Load Balancing

Problem:  
If using R53 with health checks, there's caching so user will keep pinging downed server. updating dns is slow

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.21.07-pm.png)

bob is connected to lb

client thinks it's connected to server directly

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.22.51-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.23.39-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.24.53-pm.png)

## Application Load Balancer

ALB inspects data that passes through it, ie: headers, paths, hosts

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.27.37-pm.png)

### Architecture

one LB per AZ

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.30.17-pm.png)

![cross-zone load balancing](../../../.gitbook/assets/screenshot-2021-07-23-at-10.30.39-pm.png)

LB distributes to loads across all AZs

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.31.28-pm.png)

one target can be in multiple groups

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.32.45-pm.png)

target can be any aws compute resource \(lambda, ec2, etc\)

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.34.42-pm.png)

CLB can only use one SSL cert

## Launch config and launch templates \(use templates, it's superset of config\)

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.36.43-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-23-at-10.37.25-pm.png)

## Auto scaling group

![](../../../.gitbook/assets/screenshot-2021-07-23-at-11.17.34-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-23-at-11.18.10-pm.png)

attempt to keep no of instances in each az equal

![](../../../.gitbook/assets/screenshot-2021-07-23-at-11.20.43-pm.png)

cooling: scaling doesn't occur until certain time has passed from last one

![](../../../.gitbook/assets/screenshot-2021-07-23-at-11.21.48-pm.png)

self-healing: if min is set to 1, sick ec2 instance is terminated and new one is provisioned

![](../../../.gitbook/assets/screenshot-2021-07-23-at-11.24.21-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-23-at-11.24.06-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-23-at-11.25.09-pm.png)

## Demo

![](../../../.gitbook/assets/screenshot-2021-07-23-at-11.26.13-pm.png)



![](../../../.gitbook/assets/screenshot-2021-07-24-at-10.55.37-am.png)

![](../../../.gitbook/assets/screenshot-2021-07-24-at-10.56.21-am%20%281%29.png)



![](../../../.gitbook/assets/screenshot-2021-07-24-at-10.57.22-am.png)

![](../../../.gitbook/assets/screenshot-2021-07-24-at-10.57.02-am%20%282%29.png)

* then create launch template
* userdata: step one

## Network load balancer



