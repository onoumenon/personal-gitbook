# VIRTUAL PRIVATE CLOUD \(VPC\) BASICS

## Networking refresher

IPv4: 0.0.0.0 to 255.255.255.255

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.38.14-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.38.43-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.39.12-pm.png)

Private-only networks

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.40.20-pm.png)

AWS use class B \(most cloud do\)

![first address/ prefix \(smaller number = bigger network\)](../../../.gitbook/assets/screenshot-2021-07-12-at-9.41.11-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.42.07-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.42.40-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.43.47-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.46.38-pm.png)

![don&apos;t blindly choose default](../../../.gitbook/assets/screenshot-2021-07-12-at-9.47.28-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.48.21-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.49.27-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.51.02-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.51.19-pm.png)

## VPC planning

* important cos it's foundation for later on
* avoid ranges other parties use

![avoid already used ip ranges](../../../.gitbook/assets/screenshot-2021-07-12-at-9.53.59-pm.png)



![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.54.41-pm.png)

azure is using default vpc of aws, AVOID using default vpc if possible

use 10.16.y.z range \(16 is nice base 2 number, avoid 10.0, and 10.1 and 10.10 cos other folks tend to use them\)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-9.59.25-pm.png)

Lesson Links

[https://aws.amazon.com/answers/networking/aws-single-vpc-design/](https://aws.amazon.com/answers/networking/aws-single-vpc-design/)

[https://cloud.google.com/vpc/docs/vpc](https://cloud.google.com/vpc/docs/vpc)

{% embed url="https://github.com/acantril/aws-sa-associate-saac02/tree/master/07-VPC-Basics/01\_vpc\_sizing\_and\_structure" %}

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.00.31-pm.png)

how many AZs? default is usually 3 AZs, then add one spare, so start with at least 4 /18s, futher split into 4 tiers = /20

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.03.11-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.05.03-pm.png)

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14441636" %}

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.06.23-pm.png)

## Custom vpc

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.07.33-pm.png)

VPC Limits [https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.09.29-pm.png)

dedicated = locked in, don't choose unless you know why

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.10.45-pm.png)

ipv6 has no concept of private and public networks

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.12.56-pm.png)

dns problems, check vpc dns settings

### Demo

* go to vpc -&gt; create vpc \(vpc-1, 10.16.0.0/16, amazon provided, default tenancy\)
* edit vpc-1, enable dns resolution, enable dns hostname



Add subnets \(blue = private\)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.30.25-pm.png)

1 subnet in 1 AZ

1 AZ can have many subnets

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.31.58-pm.png)

### reserved subnets

unusable: network address \(first address\), vpc router, reserved \(dns\), reserved for future, last ip address \(broadcast\)

keep in mind if you're creating subnets, esp smaller ones \(first 4: network 0, +1 to +3, and last address\)

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.34.47-pm.png)

DHCP options set can be created but not edited

you can set auto assign ipv4/v6

![](../../../.gitbook/assets/screenshot-2021-07-12-at-10.37.16-pm.png)

* use Subnet Calculator : [https://www.site24x7.com/tools/ipv4-subnetcalculator.html](https://www.site24x7.com/tools/ipv4-subnetcalculator.html) to calculate subnets, /16
* Subnets.txt [https://github.com/acantril/aws-sa-associate-saac02/blob/master/07-VPC-Basics/03\_vpc\_subnets/subnets.txt](https://github.com/acantril/aws-sa-associate-saac02/blob/master/07-VPC-Basics/03_vpc_subnets/subnets.txt)
* vpc -&gt; choose vpc -&gt; create subnet for AZ A
* vpc id \(select vpc-1\)
* 1 of 1 subnet settings \(name: sn-reserved-A, CIDR range
* add new subnet \(2 of 2\) ...
* till subnet 4
* repeat for AZ B, C
* modify auto-assign ip settings, enable auto-assign ipv6







