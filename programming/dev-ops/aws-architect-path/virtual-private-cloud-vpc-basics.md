# VIRTUAL PRIVATE CLOUD (VPC) BASICS

## Networking refresher

IPv4: 0.0.0.0 to 255.255.255.255

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.38.14 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.38.43 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.39.12 PM.png>)

Private-only networks

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.40.20 PM.png>)

AWS use class B (most cloud do)

![first address/ prefix (smaller number = bigger network)](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.41.11 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.42.07 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.42.40 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.43.47 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.46.38 PM.png>)

![don't blindly choose default](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.47.28 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.48.21 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.49.27 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.51.02 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.51.19 PM.png>)

## VPC planning

* important cos it's foundation for later on
* avoid ranges other parties use

![avoid already used ip ranges](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.53.59 PM.png>)



![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.54.41 PM.png>)

azure is using default vpc of aws, AVOID using default vpc if possible

use 10.16.y.z range (16 is nice base 2 number, avoid 10.0, and 10.1 and 10.10 cos other folks tend to use them)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.59.25 PM.png>)

Lesson Links

[https://aws.amazon.com/answers/networking/aws-single-vpc-design/](https://aws.amazon.com/answers/networking/aws-single-vpc-design/)

[https://cloud.google.com/vpc/docs/vpc](https://cloud.google.com/vpc/docs/vpc)

{% embed url="https://github.com/acantril/aws-sa-associate-saac02/tree/master/07-VPC-Basics/01_vpc_sizing_and_structure" %}

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.00.31 PM.png>)

how many AZs? default is usually 3 AZs, then add one spare, so start with at least 4 /18s, futher split into 4 tiers = /20

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.03.11 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.05.03 PM.png>)

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14441636" %}

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.06.23 PM.png>)

## Custom vpc

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.07.33 PM.png>)

VPC Limits [https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.09.29 PM.png>)

dedicated = locked in, don't choose unless you know why

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.10.45 PM.png>)

ipv6 has no concept of private and public networks

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.12.56 PM.png>)

dns problems, check vpc dns settings

### Demo

* go to vpc -> create vpc (vpc-1, 10.16.0.0/16, amazon provided, default tenancy)
* edit vpc-1, enable dns resolution, enable dns hostname



Add subnets (blue = private)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.30.25 PM.png>)

1 subnet in 1 AZ

1 AZ can have many subnets

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.31.58 PM.png>)

### reserved subnets

unusable: network address (first address), vpc router, reserved (dns), reserved for future, last ip address (broadcast)

keep in mind if you're creating subnets, esp smaller ones (first 4: network 0, +1 to +3, and last address)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.34.47 PM.png>)

DHCP options set can be created but not edited

you can set auto assign ipv4/v6

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 10.37.16 PM.png>)

* use Subnet Calculator : [https://www.site24x7.com/tools/ipv4-subnetcalculator.html](https://www.site24x7.com/tools/ipv4-subnetcalculator.html) to calculate subnets, /16
* Subnets.txt [https://github.com/acantril/aws-sa-associate-saac02/blob/master/07-VPC-Basics/03\_vpc\_subnets/subnets.txt](https://github.com/acantril/aws-sa-associate-saac02/blob/master/07-VPC-Basics/03\_vpc\_subnets/subnets.txt)
* vpc -> choose vpc -> create subnet for AZ A
* vpc id (select vpc-1)
* 1 of 1 subnet settings (name: sn-reserved-A, CIDR range
* add new subnet (2 of 2) ...
* till subnet 4
* repeat for AZ B, C
* modify auto-assign ip settings, enable auto-assign ipv6



## VPC Routing and Internet Gateway

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 9.29.57 PM.png>)

destination can be range or 1 ip

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 9.34.28 PM.png>)

IGW is region resilient by design

main route table is set by default

goes to most specific matches for destination first, local takes priority over non-local

target: local (same vpc)/ aws gateway



![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 9.36.05 PM.png>)

only covers AZ that VPC is using

it's also used for 'public' services, like s3 (to make private files avail)

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 9.36.34 PM.png>)

associate rt to web subnet

target is IGW

private ip address is not used, it's mapped to public address in IGW record

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 10.40.16 PM.png>)

### Demo

* click on Vpc, attach IGW to vpc by create Internet Gateway and create attach
* create route table with target as IGW (eg: vpc-1-rt-web)
* associate web-a web-b and web-c subnets (otherwise it's assoc w main route table)
* edit table, edit route, add 0.0.0.0/0, target igw, add ::/0, target igw
* auto-assign IPV4 for web-a, web-b, web-c

### Network Access Control List

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 10.56.01 PM.png>)

Inbound rule for inbound traffic

Outbound rule for outbound traffic

Processed in order of rule # (lowest first)

NACLs can explicitly ALLOW or DENY traffic to subnet

all fields must match for rule to match (first rule that matches applies)

default is implicit deny&#x20;

all ip comm is initiation + response

need to add outbound rule for ephemeral ports for response traffic

if web subnet is communicating with app subnet, then app subnet needs a inbound rule, and web an outbound rule

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.14.39 PM.png>)

NACLs are only for subnet

NACLs can explicity deny traffic to subnet

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.17.07 PM.png>)

## Security Group

Security group are like NACLs but they are attached to resource not subnet

But NACLs is stateless, but Security Group is stateful, so it only requires inbound/ outbound (as response auto allow)

can ref instance instead of ip, source can be itself, hidden implicit deny

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.21.46 PM.png>)

Security Groups (SGs) are another security feature of AWS VPC ... only unlike NACLs they are attached to AWS resources, not VPC subnets.

SGs offer a few advantages vs NACLs in that they can recognize AWS resources and filter based on them, they can reference other SGs and also themselves.

But.. SGs are not capable of explicitly blocking traffic - so often require assistance from NACL

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.22.53 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.23.22 PM.png>)

## NAT

IGW uses NAT to map private ip to public ip

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.25.21 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.27.32 PM.png>)

route table routes to NAT

NAT is only AZ resilient in the subnet it's in

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.31.09 PM.png>)

two charges: duration + data vol

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.33.33 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.32.53 PM.png>)

Disable Source/Destination checks if you want to use ec2 instance as Nat instance

use NAT gateway over NAT instance

unless you need cheaper or free

no free tier for NAT gateway

NAT gateway cannot do port fowarding or use as bastion server

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.37.15 PM.png>)

### Demo

{% embed url="https://learn.cantrill.io/courses/730712/lectures/27209177" %}

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.39.26 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.41.27 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-13 at 11.41.58 PM.png>)

### Demo to add nat gateways

{% embed url="https://learn.cantrill.io/courses/730712/lectures/27209178" %}

