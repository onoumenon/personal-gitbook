# AWS Fundamentals

### Public VS Private Service

It's all about networking, no permissions by default (ie: login)

![](<../../../.gitbook/assets/Screenshot 2021-06-24 at 11.16.33 PM.png>)

Private services like EC2 can be accessed by public network by 'pushing' part of it to aws public network.

### Global network

Edge locations are more specific, preferred for low latency needs

{% embed url="https://www.infrastructure.aws/" %}

![](<../../../.gitbook/assets/Screenshot 2021-06-24 at 11.19.16 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-24 at 11.20.11 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-24 at 11.20.39 PM.png>)

### VPC

A **default VPC is created once per region (only 1 per region) **when an AWS account is first created.

There can only be one default VPC per region, and they can be deleted and recreated from the console UI .

![](<../../../.gitbook/assets/Screenshot 2021-06-24 at 11.23.19 PM.png>)

Default VPC is always configured the same

They always have the same IP range and same '1 subnet per AZ' architecture.

![](<../../../.gitbook/assets/Screenshot 2021-06-24 at 11.25.52 PM.png>)

resilient across availability zones

![Default VPC](<../../../.gitbook/assets/Screenshot 2021-06-24 at 11.27.25 PM.png>)

You'll probably need custom VPC



### EC2 - Elastic Cloud

VPC network means EC2 is AZ resilient

![](<../../../.gitbook/assets/Screenshot 2021-06-25 at 10.24.32 PM.png>)

Terminate = delete (irreversible)

storage is still used when instance is stopped

![](<../../../.gitbook/assets/Screenshot 2021-06-25 at 10.31.44 PM.png>)

3389 port  remote desktop protocol - windows

22 port for ssh protocol - mac/ linux

### EC2 Practical

* Go to EC2
* Create keypair -> use pem if ssh/ linux -> save private part of key
* Launch instance (eg for type: linux, Amazon linux 2 AMI, x86, t2micro (free)
* configure instance -> default
* add storage (device id, root vol) ...
* security group (1 security group can be many-to-many mapping), set source to my IP (or limit it somehow)
* review and launch -> launch
* select a key pair (public key on instance)
* state = pending, then ready
* click on instance -> connect to instance, copy the `ssh ....` command
* move to folder w pem, might need to run chmod command
* run ssh

### S3

Default storage service

![](<../../../.gitbook/assets/Screenshot 2021-06-25 at 11.27.12 PM.png>)

Objects are like files

bucket are created by default in a region

![](<../../../.gitbook/assets/Screenshot 2021-06-25 at 11.28.44 PM.png>)

folders are referred to as prefixes, because /old/koala.jpg is positioned in old folder

![](<../../../.gitbook/assets/Screenshot 2021-06-25 at 11.29.57 PM.png>)

has no file-based system, it's flat

![](<../../../.gitbook/assets/Screenshot 2021-06-25 at 11.31.16 PM.png>)

### S3 Practical

* search for s3
* don't need to choose region for service, service is global, bucket is regional
* create bucket with unique name, choose region
* choose block public access (by default is private, nobody has permission to bucket)
* unchecking 'block all public access' means you can grant access to public
* ARN references at least one resource (can be wildcard)
* create folder is creating an object emulating a folder
* object action -> open (open w authentication), direct url won't be accessible since there's no permissions

### CloudFormation

use templates to update infrastructure , kinda similar to terraform

`AWSTemplateFormatVersion` must follow with `Description`

`parameters`user input

`mapping` create lookup tables

`condition`decision making based on condition

`outputs` return output after done

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 9.28.12 PM.png>)

* Select CloudFormation
* create stack, prepare template, if upload template, it'll be in an s3 bucket auto created
* create stack -> stack name, parameters (AMI, SSHandWebLocation)
* CF may create IAM roles

### Cloudwatch

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 9.59.11 PM.png>)

cloudwatch is namespaced, eg: aws/ec2

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.02.02 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.02.28 PM.png>)

### Demo Steps

* Create an EC2 instance
* t2.micro
* ensure its set to the default VPC and has a public IP
* Optionally enable detailed monitoring
* Connect to the instance and install Extras package and stress
* Install stress (commands listed in code sample below)
* Create an alarm based on the CPU Utilisation of the created instance
* Threshold greater than 15%
* Run stress 'stress -c 2'
* Wait for alarm to .. alarm
* use ctrl + c to cancel stress
* Wait for alarm to return to ..ok
* Delete the alarm
* Delete the instance

[Demo Instructions Link](https://github.com/acantril/aws-sa-associate-saac02/blob/master/04-AWS-Fundamentals/04\_cloudwatch\_demo/demo\_instructions.txt)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.03.39 PM.png>)

Create tag so you can identify instances

click on instance > connect to instance, ec2 instance connect

### Shared Responsibility

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.14.14 PM.png>)

### HA & FT & DR

HA - maximise system's online time, eg: automate switching to spare to replace, like spare tire

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.16.37 PM.png>)

FT - no failure during disruption (HA is not enough)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.19.18 PM.png>)

You have no downtime. eg: multiple spares on plane, more complex and expensive

DR - plans for disaster

offsite location

you should have backups at offsite

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.21.29 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.21.54 PM.png>)

### DNS

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.22.36 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.23.22 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.33.29 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.34.56 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.35.27 PM.png>)

Root hints : [https://www.internic.net/domain/named.root](https://www.internic.net/domain/named.root)

IANA : [https://www.iana.org](https://www.iana.org)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.46.38 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.48.01 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.49.10 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.49.56 PM.png>)

Root Servers : [https://www.iana.org/domains/root/servers](https://www.iana.org/domains/root/servers)

Root Zone Database : [https://www.iana.org/domains/root/db](https://www.iana.org/domains/root/db)

Root Zone File : [https://www.internic.net/domain/root.zone](https://www.internic.net/domain/root.zone)

Deletegation Record for .com : [https://www.iana.org/domains/root/db/com.html](https://www.iana.org/domains/root/db/com.html)

### Route 53

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 10.50.52 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 11.03.33 PM.png>)

* search route 53
* get started/ registered domains ->  register one -> registrant contact, tech contact, admin contact
* pending requests -> complete
* registered domain -> domain should appear -> name servers (able to point it to files outside aws)
* hosted zones

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 11.10.27 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 11.11.15 PM.png>)

cname cannot point to ip address, only other names

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 11.27.04 PM.png>)

mx20 (other domain, eg: send mail to non gmail acc)

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 11.27.57 PM.png>)

prove domain ownership

![](<../../../.gitbook/assets/Screenshot 2021-06-27 at 11.29.00 PM.png>)

TTL on record for time to cache/ keep record

