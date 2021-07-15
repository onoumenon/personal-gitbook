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





