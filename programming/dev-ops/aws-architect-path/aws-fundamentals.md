# AWS Fundamentals

### Public VS Private Service

It's all about networking, no permissions by default \(ie: login\)

![](../../../.gitbook/assets/screenshot-2021-06-24-at-11.16.33-pm.png)

Private services like EC2 can be accessed by public network by 'pushing' part of it to aws public network.

### Global network

Edge locations are more specific, preferred for low latency needs

{% embed url="https://www.infrastructure.aws/" %}

![](../../../.gitbook/assets/screenshot-2021-06-24-at-11.19.16-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-24-at-11.20.11-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-24-at-11.20.39-pm.png)

### VPC

A **default VPC is created once per region \(only 1 per region\)** when an AWS account is first created.

There can only be one default VPC per region, and they can be deleted and recreated from the console UI .

![](../../../.gitbook/assets/screenshot-2021-06-24-at-11.23.19-pm.png)

Default VPC is always configured the same

They always have the same IP range and same '1 subnet per AZ' architecture.

![](../../../.gitbook/assets/screenshot-2021-06-24-at-11.25.52-pm.png)

resilient across availability zones

![Default VPC](../../../.gitbook/assets/screenshot-2021-06-24-at-11.27.25-pm.png)

You'll probably need custom VPC



### EC2 - Elastic Cloud

VPC network means EC2 is AZ resilient

![](../../../.gitbook/assets/screenshot-2021-06-25-at-10.24.32-pm.png)

Terminate = delete \(irreversible\)

storage is still used when instance is stopped

![](../../../.gitbook/assets/screenshot-2021-06-25-at-10.31.44-pm.png)

3389 port  remote desktop protocol - windows

22 port for ssh protocol - mac/ linux



