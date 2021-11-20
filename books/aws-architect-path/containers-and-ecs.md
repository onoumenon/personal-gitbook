# CONTAINERS & ECS

## Virtualization problems

Guest OS is duplicated

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 10.34.17 PM.png>)

Containers solve the problem by having the container runs as a process in the host OS, but each container is isolated from each other

Much lighter than virtualization

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 10.36.28 PM.png>)

Container is running copy of docker image

Docker image is created by Dockerfile steps

first line is base image, centos:7, and each instructions layers on top of it

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 10.41.49 PM.png>)

Docker container has read write layer

Base layers can be gotten from the registry, eg Dockerhub

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 10.44.31 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 10.45.10 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 10.47.23 PM.png>)

Demo: [https://learn.cantrill.io/courses/730712/lectures/14640455](https://learn.cantrill.io/courses/730712/lectures/14640455)

* ec2 instance connect
* [Lesson Commands](https://github.com/acantril/aws-sa-associate-saac02/blob/master/09-Containers-ECS/container\_of\_cats/lesson\_commands.txt)
* install docker
* sudo service docker start
* give user permission to use docker `sudo usermod -a -G docker ec2-user`
* exit and log back in ec2 instance
* `sudo yum install git`
* `git clone {repo}`
* there should be Dockerfile, jpeg files

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 10.51.29 PM.png>)

entrypoint is apache web server

* `docker build -t containerofcats .`
* `docker images`
* `docker run -t -i p 80:80 containerofcats`
* `docker login --username {name}`
* enter password
* `docker tag {img id} {user}/{image name}`

## &#x20;ECS - Elastic Container Service

ECS lets you create a cluster, which is where containers run from

ECR: aws container registry

Container definition tells ECS where container is, which port is used on container

think of container definition as just a pointer to where container is stored, and which port

task may contain 1 or more containers

task definition store the rest: storage mode, task role, which give containers within ecs permission to access aws services

service lets you determine how to scale task, load balance for tasks, replace failed tasks, etc

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 11.04.46 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 11.04.13 PM.png>)

ECS is capable of running in EC2 mode or Fargate mode.

EC2 mode deploys EC2 instances into your AWS account which can be used to deploy tasks and services.

With EC2 mode you pay for the EC2 instances regardless of container usage

Fargate mode uses shared AWS infrastructure, and ENI's which are injected into your VPC

You pay only for container resources used while they are running.

## ECS Cluster types

ASG: auto scaling group

EC2 mode: not serverless, handles no of instances deployed, you handle capacity, no of container hosts, great middle ground

you pay for ec2 instances regardless of container usage

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 11.09.06 PM.png>)

fargate: no servers to manage, use fargate shared infra, injected into vpc, no need to care for capacity

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 11.11.26 PM.png>)

EC2: virtualization

ECS in EC2 mode: containerized app&#x20;

Fargate: less management overhead, burst cos charge for only what you use

![](<../../../.gitbook/assets/Screenshot 2021-07-19 at 11.14.29 PM.png>)

Demo: [https://learn.cantrill.io/courses/730712/lectures/14640459](https://learn.cantrill.io/courses/730712/lectures/14640459)

* ECS
* Clusters -> create cluster -> networking only fargate
* create w/o create vpc
* cos default alr has public ip address for all subnets
* task definition - create fargate
* task role: set task role
* task memory and cpu
* add container, can add multiple containers, choose image
* since web server port 80, set port to 80, click create
* click clusters, select cluster, click tasks tab
* &#x20;run a new task
* switch launch type fargate
* select vpc to attach to (default: 172...)
* select two subnets
* attach security group, run task
* copy public ip of cluster



