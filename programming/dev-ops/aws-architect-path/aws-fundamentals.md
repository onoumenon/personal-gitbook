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

### EC2 Practical

* Go to EC2
* Create keypair -&gt; use pem if ssh/ linux -&gt; save private part of key
* Launch instance \(eg for type: linux, Amazon linux 2 AMI, x86, t2micro \(free\)
* configure instance -&gt; default
* add storage \(device id, root vol\) ...
* security group \(1 security group can be many-to-many mapping\), set source to my IP \(or limit it somehow\)
* review and launch -&gt; launch
* select a key pair \(public key on instance\)
* state = pending, then ready
* click on instance -&gt; connect to instance, copy the `ssh ....` command
* move to folder w pem, might need to run chmod command
* run ssh

### S3

Default storage service

![](../../../.gitbook/assets/screenshot-2021-06-25-at-11.27.12-pm.png)

Objects are like files

bucket are created by default in a region

![](../../../.gitbook/assets/screenshot-2021-06-25-at-11.28.44-pm.png)

folders are referred to as prefixes, because /old/koala.jpg is positioned in old folder

![](../../../.gitbook/assets/screenshot-2021-06-25-at-11.29.57-pm.png)

has no file-based system, it's flat

![](../../../.gitbook/assets/screenshot-2021-06-25-at-11.31.16-pm.png)



