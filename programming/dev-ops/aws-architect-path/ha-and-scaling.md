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



