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





