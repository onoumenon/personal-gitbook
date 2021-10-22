# AWS (Architect path)

{% embed url="https://learn.acloud.guru/course/intro-to-aws/learn/cd91175f-45d3-4c98-ae7a-cb49340fe8e3/68b55286-631c-4d32-8d65-3a1621d208c9/watch" %}

A cloud is a collection of services which create a platform:\
\- databases, data storage, etc

Why cloud?

* Pay as you go (Lambda has pay by millisecond)

History: Benjamin Black wanted to efficiently scale infrastructure. old method is to install 50 servers in server room if that is needed. Elastic compute cloud is the result of that.

The following is a high-level overview of AWS and some of the services.

## Regions

AWS servers

Regions are the area where AWS physical servers are located. (Eg: ap-southeast-2)\
\
Each region has multiple availability zones, each with its own independent power, cooling, physical security, etc

![](<../../../.gitbook/assets/Screenshot 2021-04-30 at 1.14.10 PM.png>)

&#x20;Names (ap-southeast-2a) are randomly generated so users don't keep sticking to one availability zone.&#x20;

The intent of this design is so that the redundancy protects users' data from outages in one zone (since it's unlikely all zones will go down at the same time).

## Security

### IAM

* manage access for your aws accounts
* create users and groups
* allow or deny via access policies

#### Users

Root user -> can create users with roles

Users can be in groups.

![](<../../../.gitbook/assets/Screenshot 2021-04-30 at 1.30.51 PM.png>)

Example JSON policy

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:AttachVolume",
                "ec2:DetachVolume"
            ],
            "Resource": [
                "arn:aws:ec2:*:*:volume/*",
                "arn:aws:ec2:*:*:instance/*"
            ],
            "Condition": {
                "ArnEquals": {"ec2:SourceInstanceARN": "arn:aws:ec2:*:*:instance/instance-id"}
            }
        }
    ]
}
```

#### Roles

You can assign users and roles to services.

### Secrets Manager

Problem: Hardcoding passwords in code is insecure and updating is messy.

* protects secrets
* rotates automatically
* stores passwords, keys, tokens

### Directory Services

Useful for: SMEs using Microsoft Active Directory for Windows to manage users' computers

When you use AWS Microsoft AD (Standard Edition) as your primary directory, you can manage access and provide single sign-on (SSO) to cloud applications such as [Microsoft Office 365](https://aws.amazon.com/blogs/security/how-to-enable-your-users-to-access-office-365-with-aws-microsoft-active-directory-credentials/). If you have an existing Microsoft AD directory, you can also use AWS Microsoft AD (Standard Edition) as a resource forest that contains primarily computers and groups, allowing you to migrate your AD-aware applications to the AWS Cloud while using existing on-premises AD credentials.

## Compute

VMs: EC2, EC2 Spot (fault tolerant, 90% off normal price), EC2 auto scaling, Lightsail (easy to use)

Containers: ECS, ECR (registry to store, manage and deploy images), EKS (kubernetes)

Severless (no servers): Lambda

Edge: Outpost (run aws services on your own servers), Snow Family (bring your own data into aws via physical devices), AWS Wavelength (aws services via 5g networks), VMWare cloud (migrate VMWare workloads), AWS local zones (latency sensitive applications closer to end users)





