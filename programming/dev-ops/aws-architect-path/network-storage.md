# NETWORK STORAGE

## Elastic File System (EFS) Architecture

The Elastic File System (EFS) is an AWS managed implementation of NFS which allows for the creation of shared 'filesystems' which can be mounted within multi EC2 instances.

EFS can play an essential part in building scalable and resilient systems.

\# Lesson Links

[https://en.wikipedia.org/wiki/File\_system\_permissions](https://en.wikipedia.org/wiki/File\_system\_permissions)

[https://docs.aws.amazon.com/efs/latest/ug/performance.html](https://docs.aws.amazon.com/efs/latest/ug/performance.html)

[https://docs.aws.amazon.com/efs/latest/ug/storage-classes.html](https://docs.aws.amazon.com/efs/latest/ug/storage-classes.html)

{% embed url="https://docs.aws.amazon.com/efs/latest/ug/lifecycle-management-efs.html" %}

linux use tree structure for storing files

EFS is separate from EC2

can be accessed w config

![](<../../../.gitbook/assets/Screenshot 2021-07-22 at 10.27.59 PM.png>)

POSIX permissions use a standard that all unix systems understand

should have mount target in every az in the vpc

![](<../../../.gitbook/assets/Screenshot 2021-07-22 at 10.36.24 PM.png>)

data processing may need max i/o

generally, choose bursting

![](<../../../.gitbook/assets/Screenshot 2021-07-22 at 10.38.00 PM.png>)

### Demo

In this 2-part demo lesson you will implement a simple EFS file system in a VPC, configure mount targets and configure two EC2 instances to mount the file system within a mount point.

[1-Click Deployment](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0019-aws-associate-storage-implementing-efs/A4L\_TWO\_EFS\_EC2.yaml\&stackName=IMPLEMENTINGEFS)

[Lesson Commands](https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0019-aws-associate-storage-implementing-efs/lesson\_commands.txt)

{% embed url="https://learn.cantrill.io/courses/730712/lectures/29562469" %}

* create stack
* go to ec2 -> see that there's 2 instances
* go to efs -> create file system -> name (a4l-efs), pick vpc (a4l vpc) -> customize
* has lifecycle management, performance: general, throughput: burst
* networking: delete default security groups, define security groups(instance sg) manually for each az
* filesystem policy: should have for prod
* check mount targets
* connect to 1st instance
* `df -k` to check file systems
* `sudo mkdir -p /efs/wp-content` make folder for efs to interact w
* `sudo yum install -y amazon-efs-utils` to install efs tools
* `cd /etc` and `sudo nano fstab` to edit the doc w instructions to mount efs system in /efs/wp-content everytime instance is restarted
* `{file-system-id}:/ /efs/wp-content efs _netdev,tls,iam 0 0` replacing file system id

```
sudo mount /efs/wp-content
df -k
cd /efs/wp-content
sudo touch amazingtestfile.txt
// then connect to the other instance
```

```
# INSTANCE B

sudo mkdir -p /efs/wp-content
sudo nano /etc/fstab
file-system-id:/ /efs/wp-content efs _netdev,tls,iam 0 0
sudo mount /efs/wp-content
ls -la
// you will see amazingtestfile.txt
```

this means even if you delete the instance, the file is saved on efs

### Demo 2

In this \[DEMO] lesson you will evolve the Animals4life wordpress architecture by moving the wp-content (static media store) from the EC2 instance to an EFS based filesystem.

This will allow Wordpress to scale - we can move to an architecture where more than one Wordpress instance runs at once.

The experience you gain in this lesson while simple - will be the same experience used in much larger projects.

[1-Click Deployment (Base Infrastructure)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0020-aws-associate-storage-scaling-wordpress-with-efs/A4L\_VPC\_EFS\_AURORA.yaml\&stackName=EFSDEMO-VPC-RDS-EFS)

Follow video on WHEN to use these

[1-Click Deployment (WORDPRESS 1)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0020-aws-associate-storage-scaling-wordpress-with-efs/A4L\_WORDPRESS.yaml\&stackName=EFSDEMO-WORDPRESS1)

[1-Click Deployment (WORDPRESS 2)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0020-aws-associate-storage-scaling-wordpress-with-efs/A4L\_WORDPRESS.yaml\&stackName=EFSDEMO-WORDPRESS2)

![](<../../../.gitbook/assets/Screenshot 2021-07-22 at 11.33.14 PM.png>)
