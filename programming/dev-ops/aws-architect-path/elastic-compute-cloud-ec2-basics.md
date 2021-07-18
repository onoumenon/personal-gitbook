# ELASTIC COMPUTE CLOUD \(EC2\) BASICS

## Virtualization

{% embed url="http://www.brendangregg.com/blog/2017-11-29/aws-ec2-virtualization-2017.html" %}

process by which more than one os can be run

![](../../../.gitbook/assets/screenshot-2021-07-14-at-8.17.59-pm.png)

Initially virtualization was not efficient

#### Emulated virtualization

has hypervisor, each app in their own virtual machine, os is emulated

app still tries to write to cpu, but actually is translated by hypervisor 

but is really slow \(binary translation\)

![](../../../.gitbook/assets/screenshot-2021-07-14-at-8.21.41-pm.png)

#### Para-virtualization

only works in subset of OS, OS that can be modified to make hypercalls directly to hypervisor

slightly faster than emulated virtualization because hypervisor is aware of vms

![](../../../.gitbook/assets/screenshot-2021-07-14-at-8.23.18-pm.png)

#### Hardware assisted virtualization

CPU is aware of virtualization, hardware redirects to hypervisor

but still limited by hardware

ie: network card on vm is actually connecting back to physical network card

disk io/ network io is still limited in speed

![](../../../.gitbook/assets/screenshot-2021-07-14-at-8.27.13-pm.png)

#### SR\_IOV

allows a network card to represent itself as multiple mini cards

![](../../../.gitbook/assets/screenshot-2021-07-14-at-8.28.39-pm.png)

aws has nitro

## EC2 architecture

* vm: os + resource
* shared host: shared by multiple customer
* dedicated host: you pay for entire host
* hosts = 1 AZ, not resilient across region

![](../../../.gitbook/assets/screenshot-2021-07-14-at-8.31.32-pm.png)

each ec2 instance has instance store, storage, data network

provisioned into subnet, elastic network interface is provisioned in the subnet which maps to the data network hardware in ec2

can use EBS in the same AZ

![](../../../.gitbook/assets/screenshot-2021-07-14-at-8.35.20-pm.png)

stopped and started, will relocated to another host

EC2 and EBS cannot access across AZ

![](../../../.gitbook/assets/screenshot-2021-07-14-at-8.36.46-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-14-at-8.39.05-pm.png)

## EC2 instance types

[https://aws.amazon.com/ec2/instance-types/](https://aws.amazon.com/ec2/instance-types/)

{% embed url="https://ec2instances.info/" %}

compute instance type have more ratio for cpu than memory

storage and data network bandwidth might be limiting factor

amd cpu vs x64, etc

![](../../../.gitbook/assets/screenshot-2021-07-14-at-10.48.34-pm.png)

Five main categories

![](../../../.gitbook/assets/screenshot-2021-07-14-at-10.49.46-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-14-at-10.50.22-pm.png)

always give full instance type to avoid miscomm

![](../../../.gitbook/assets/screenshot-2021-07-14-at-10.53.15-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-14-at-10.54.36-pm.png)

[https://aws.amazon.com/ec2/instance-types/](https://aws.amazon.com/ec2/instance-types/)

[https://ec2instances.info/](https://ec2instances.info/)

General: A1, M6g \(graviton, efficient\), T3, T3a \(burst, eg: email relay w low usage usually\), M5, M5a, M5n \(steady state, intel/ amd architecture\)

Compute optimized: C5, C5n 

Memory optimized: R5, R5a \(real time analytics, in memory ops\), X1, X1e \(lowest $ per GiB memory, large scale\), High memory, z1d \(directly attached NVMe\)

Accelerated computing: P3, G4, \(graphics\) F1 \(finance\), Infl \(machine learning\)

Storage Optimized: I3/I3en, D2, H1

### Demo

{% embed url="https://learn.cantrill.io/courses/730712/lectures/30093143" %}

* EC2, select EC2, security tab, inbound rules
* 80 TCP 0.0.0.0/0 \(HTTP\)
* 22 TCP 0.0.0.0/0 \(SSH\)
* 22 TCP ::/0 \(SSH IPv4\)
* SSH: right click instance and click connect, ssh, follow instructions
* note you need to cmod \(to limit permission for key\)
* EC2 instance connect: right click instance, connect, ec2 instance connect
* you are using IAMuser permission for ec2 instance connect
* if you limit ssh ip to your local ip, you won't be able to ec2 instance connect
* you can limit it to include the range for aws services \(search for EC2 instance connect with your region\)
* [https://ip-ranges.amazonaws.com/ip-ranges.json](https://ip-ranges.amazonaws.com/ip-ranges.json)

## Storage Refresher

direct: fast, but can be lost

network: resilient

![](../../../.gitbook/assets/screenshot-2021-07-14-at-11.36.29-pm.png)

EBS is persistent

Block storage no structure, mount, boot

file storage has structure, mount

object storage, flat

![](../../../.gitbook/assets/screenshot-2021-07-14-at-11.39.02-pm.png)

high perf boot from: block

read files: file storage

cat pics app: obj storage

IOPS: IO operations per second

IO size: block size \(16k, 1 meg\)

the lowest performing factor is the limiting factor \(eg: latency\)

![](../../../.gitbook/assets/screenshot-2021-07-14-at-11.42.10-pm.png)

## Elastic Block Store

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.31.41-pm.png)

EBS is separate from EC2, has separate lifecycles

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.33.41-pm.png)

## GP2 \(general usage, to be replaced w gp3\)

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.40.09-pm.png)

IO credit allocation, 1 IO credit is 16kb, if no credit = no IO operations can be performed

think of IO credit as mana, which refills

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.37.20-pm.png)

vols larger than 1000gb don't use this mana system, and baseline is always achievable

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.39.34-pm.png)

## GP3

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.41.06-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.42.44-pm.png)

## EBS Volume Types

io1, io2 Provisioned IOPS SSD

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.55.38-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.56.27-pm.png)



## HDD

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.58.23-pm.png)

moving bits = slower

IOPS in mb rather than gb

![](../../../.gitbook/assets/screenshot-2021-07-15-at-10.59.43-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-15-at-11.00.13-pm.png)

## Instance Store Volumes

An _instance store_ provides temporary block-level storage for your instance. This storage is located on disks that are physically attached to the host computer. Instance store is ideal for temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content, or for data that is replicated across a fleet of instances, such as a load-balanced pool of web servers.

An instance store consists of one or more instance store volumes exposed as block devices. The size of an instance store as well as the number of devices available varies by instance type.

The virtual devices for instance store volumes are `ephemeral[0-23]`. Instance types that support one instance store volume have `ephemeral0`. Instance types that support two instance store volumes have `ephemeral0` and `ephemeral1`, and so on.

![](../../../.gitbook/assets/screenshot-2021-07-15-at-11.01.38-pm.png)



![](../../../.gitbook/assets/screenshot-2021-07-15-at-11.02.49-pm.png)

Any files stored in ephemeral storage will be lost when an instance migrates host.

it migrates for many reasons, eg: stop and start instance

ephemeral vol can also fail

faster than ebs

![](../../../.gitbook/assets/screenshot-2021-07-15-at-11.06.21-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-15-at-11.07.33-pm.png)

## Instance store vs ebs

![](../../../.gitbook/assets/screenshot-2021-07-15-at-11.09.34-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-15-at-11.11.32-pm.png)

Remember the above

## EBS Snapshots

![](../../../.gitbook/assets/screenshot-2021-07-15-at-11.14.19-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-17-at-6.13.35-pm.png)

requested blocks from backup snapshots is fetched immediately from s3 but is slower than ebs unless FSR

![](../../../.gitbook/assets/screenshot-2021-07-17-at-6.21.46-pm.png)

gb per month, so 10gb for half month vs 5gb for full month costs the same

only calc used data, so eg: volume is 10gb, but data takes up 1gb, then first snapshot is 1gb in size, and billed at 1gb, subsequent snapshots are billed by incremental change size

![](../../../.gitbook/assets/screenshot-2021-07-17-at-6.28.17-pm.png)

### Demo

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14576059" %}

## EBS Encryption

DEK - data encryption key

![](../../../.gitbook/assets/screenshot-2021-07-17-at-6.48.41-pm.png)

ec2 instance moves, host loses decrypted DEK

![](../../../.gitbook/assets/screenshot-2021-07-17-at-6.51.09-pm.png)

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14576061" %}

* add storage \(in ec2 instance config\), choose encryption
* config security group

## Network Interface, instance ips, dns

ENI - elastic network interface

ipv6 are by default public routable

security groups are attached to interface

need to edit NAT for source/ dest check

![](../../../.gitbook/assets/screenshot-2021-07-18-at-12.24.09-am.png)

primary private ip does not change

resolvable only within vpc

public ipv4 is dynamic \(restart doesn't change, stop and start does\)

if you assign elastic ip, it removes public ipv4, toggling it off gets new ipv4

![](../../../.gitbook/assets/screenshot-2021-07-18-at-12.28.06-am.png)

mac address is viewed as static, but since ec2 is virtual, you can swap and change elastic network interfaces, so you can move the licensing as well

multiple interfaces rather than separate ips, good for security groups as it's attached to interface

config ec2 sometimes is the same as config interface

![](../../../.gitbook/assets/screenshot-2021-07-18-at-12.32.35-am.png)

demo

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14598419" %}

* ec2 instance connect, set dbname and password, etc
* sudo systemctl enable httpd, mariadb \( to help restart services if down\)
* sudo systemctl start httpd mariadb \(to start services\)
* mysqladmin -u root password $DBROOTPASSWORD
* download \(wget\) wordpress and extract \(tar\) and put it in correct folder
* rm the download file
* wp-config-sample.php -&gt; wp-config.php
* sed to replace placeholder w actual values
* chown apache:apache
* etc

[Lesson Commands](https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0006-aws-associate-ec2-wordpress-on-ec2/lesson_commands.txt)

## AMI

ami are regional, each region has own set of amis

![](../../../.gitbook/assets/screenshot-2021-07-18-at-11.48.13-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-18-at-11.48.50-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-18-at-11.52.15-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-18-at-11.53.53-pm.png)

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14599922" %}

* stop instance
* right click and create image
* creating ami will create snapshot of ebs
* in AMI tab, an image will be created

## Instance pricing

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.35.16-am.png)

On demand:

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.36.39-am.png)

Spot pricing

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.39.31-am.png)

Reserved:

reserved has highest priority if AZ/ region has issues

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.42.58-am.png)

you can use all 3 in combination

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.44.49-am.png)

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.46.37-am.png)

only new types of instances, you can create alert with action \(recover\)

{% embed url="https://learn.cantrill.io/courses/730712/lectures/29725578" %}

### Termination protection

* right click instance, change termination protection
* enable 'disableApiTermination'
* role separation
* right click instance, instance settings, change shutdown behavior
* stop/ terminate
* most of the time it shd be stop

## horizontal vertical scaling

vertical scaling

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.55.27-am.png)

 

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.56.28-am.png)

horizontal:

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.57.04-am.png)

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.57.21-am.png)

sessions are hosted somewhere else

![](../../../.gitbook/assets/screenshot-2021-07-19-at-3.59.38-am.png)

## Instance metadata

![](../../../.gitbook/assets/screenshot-2021-07-19-at-4.02.53-am.png)

`ifconfig` in ec2 connect \(no public ipv4 address is config at instance level, that's igw\)

`curl http://168.254.269.254/latest/meta-data/public-ipv4` to get ipv4

an easier way is to download metadata tool: `wget http://s3.amazonaws.com/ec2metadata/ec2-metadata`

 

![](../../../.gitbook/assets/screenshot-2021-07-19-at-4.10.30-am.png)



















