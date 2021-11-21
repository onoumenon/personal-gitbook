# Relational Database Service (RDS)

## Database Refresher

SQL vs NoSQL

Relational DB has a rigid schema

defined in advanced, the relationship between tables and the structure

NoSQL is the alternative, not a single thing, but generally more relaxed in schema

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.09.07 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.11.27 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.12.56 PM.png>)

Key value is really fast because of the simplicity

good for in-memory caching



![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.15.54 PM.png>)

sort/ range key for aws secondary key

architecture of key structure are rigid

the attributes of a table can be different for each row

can't do relational queries

{% embed url="https://database.guide/what-is-a-column-store-database/" %}



![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.21.30 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.23.08 PM.png>)

Redshift is a data warehouse product&#x20;

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.24.52 PM.png>)

good for complex relationship/ social media

## Acid vs BASE

This lesson steps through the ACID and BASE Database transaction models and introduces the CAP Theorem

[https://en.wikipedia.org/wiki/CAP\_theorem](https://en.wikipedia.org/wiki/CAP\_theorem)

[https://en.wikipedia.org/wiki/ACID#Consistency](https://en.wikipedia.org/wiki/ACID#Consistency)

[https://en.wikipedia.org/wiki/Eventual\_consistency](https://en.wikipedia.org/wiki/Eventual\_consistency)

{% embed url="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transactions.html" %}

CAP

Consistency: every read will get most recent write or errors out

Availability: will only receive non-error response, but not guaranteed to be most recent write

Partition tolerant: multiple network partition, and system continues to operate despite having dropped messages or errors between nodes

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.33.02 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.33.18 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.35.16 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.37.07 PM.png>)

BASE refers to NoSQL, DynamoDB

ACID refers to SQL, may refer to Dynamo DB Transactions

## Databases on EC2

usually not a good idea to run db on ec2, unless you know what you're doing

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.40.21 PM.png>)

splitting introduces dependency and cost

Not really justified: need access of os of db, but there's not many reasons why you need that

Not really justified: option tuning, but aws allows you to tune with other services

Justified: db or db ver aws don't provide

Justified: specific os/db combination

Justified: aws architecture don't provide

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.43.54 PM.png>)

patching of db with app, need to do out of core usage hours

backup/ DR management: many aws managed products have automation, why change the formula?

single AZ, need ebs snapshots, put snapshots outside of AZ

aws DB products are already good, better for replication, performance features, why diy?

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.49.06 PM.png>)

## Migrating Wordpress Monolith to dedicated EC2 DB

In this demo lesson you will migrate the database for the monolithic animals4life Wordpress instance to a dedicated MariaDB platform running on EC2.

The outcome of this lesson is to understand the process for performing a simple database migration manually...

[1-Click Deployment](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0014-aws-associate-rds-dbonec2/A4L\_WORDPRESS\_ALLINONE\_AND\_EC2DB.yaml\&stackName=MONOLITHTOEC2DB)

[Lesson Commands](https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0014-aws-associate-rds-dbonec2/lesson\_commands.txt)

Deployment Time **\~15 minutes**

Demo Time **\~15 minutes**

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 8.50.35 PM.png>)

* create stack
* go to WP instance, go to IP, install WP, make a simple post
* images are now stored in instance which later needs to be migrated out to its own file system
* right click WP instance and EC2 instance connect
* get the data on db `mysqldump -u root -p {dbname} > {dbname}.sql`
* enter dbpassword
* `ls -la`
* `mysql -h {private ipv4 of db instance} -u {dbuser} -p {dbname} < a4lwordpress.sql`
* the above command takes the db dump and injects it to the new db
* enter dbpassword
* configure WP to point to the new db server
* `cd /var/www/html`
* `sudo nano wp-config.php`
* find `DB_HOST` and change from localhost to `{private ipv4 of db instance}`
* `Ctrl-O to save, Ctrl-X to exit`
* `sudo service mariadb stop` on original WP instance

## RDS

it's not DBaas and more DatabaseServer as a service, provides managed db instance

don't need to handle hardware and engines yourself

amazon aurora is so different that it should be a category on its own&#x20;

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 9.08.55 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 9.09.58 PM.png>)

Basic building block is a rds instance (you can create more than one db instance linked to it)

client access using database CNAME&#x20;

interfacing with db instance is similar to other types of ec2 instance

db.m5: general, r5: memory, t3: burst, has variety of sizes

can be multi az if you have multiple instances

the simple setup in 1 az, storage is similar to ebs storage, ie: gp2 is default

pricing similar to ebs as well, allocated gb/m, so 10gb per month is same as 20gb for half a month

extra payment for io1 additional min iops

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 9.27.06 PM.png>)

## Migrating MariaDB database into RDS

In this \[DEMO] Lesson you will create a MySQL RDS instance and migrate the Wordpress Database from the self-managed MariaDB server running on EC2 into this RDS instance.

Due to the time required to work with RDS please ensure you have at least 1 hour to work through this demo.

[1-Click Deployment](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0015-aws-associate-rds-migrating-to-rds/A4L\_WORDPRESS\_AND\_EC2DB.yaml\&stackName=MIGRATE2RDS)

[Lesson Commands](https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0015-aws-associate-rds-migrating-to-rds/lesson\_commands.txt)

Deployment Time **\~15 mins**

Lesson Time **\~40 mins**

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 9.38.47 PM.png>)

* create stack
* go to ec2 -> select wordpress instance public ipv4, set up, log in wordpress and creeate post
* rds -> subnet groups - create db subnet group, vpc 1, az: us-east-1a to 1c
* go to vpc -> subnets -> check for ip range of db1a - 1c
* subnets: choose the ip ranges for db1a - 1c
* create subnet group
* create db -> standard create -> engine type: some are commercial, some are free, pick mysql community -> template: free tier -> pick version, some may have limitations, choose 5.7.31 -> instance identifier (a4lwordpress) -> cred settings: same as rds db -> instance class (limited to free tier) -> storage (toggle off autoscaling for demo) -> vpc: vpc1, subnet group: a4lnsgroup, public access: no, security group: create new  name a4lvpc-rds-sg -> password auth -> initial db name (need for migration), enable backups -> create db
* RDS db instance has endpoint and port, with correct vpc and subnets, has security group
* click on securtiy group, which only has inbound rule allowing your ip address
* edit inbound rule, delete ip address, allow wordpress instance instead
* go back to a4l rds db instance, connectivity and security tab
* connect to a4l wordpress instance
* backup ec2 db

```
# Backup of Source Database
mysqldump -h PRIVATEIPOFMARIADBINSTANCE -u a4lwordpress -p a4lwordpress > a4lwordpress.sql
```

* inject into rds instance (endpoint name is the CNAME)

```
# Restore to Destination Database
mysql -h CNAMEOFRDSINSTANCE -u a4lwordpress -p a4lwordpress < a4lwordpress.sql 
```

* change wp-config

```
# Change WP Config
cd /var/www/html
sudo nano wp-config.php

replace
/** MySQL hostname */
define('DB_HOST', 'PRIVATEIPOFMARIADBINSTANCE');

with 
/** MySQL hostname */
define('DB_HOST', 'REPLACEME_WITH_RDSINSTANCEENDPOINTADDRESS'); 
```

* verify by stopping db ec2 instance

## Multi AZ

cannot access standby via CNAME normally

little lag between sync, therefore syncronous replication

is not fault-tolerant, there will be a short period of outage before CNAME is switched over to point to standby

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 10.32.53 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 10.34.15 PM.png>)

## RDS backups and restores

RDS is capable of performing Manual Snapshots and Automatic backups

Manual snapshots are performed manually and live past the termination of an RDS instance

Automatic backups can be taken of an RDS instance with a 0 (Disabled) to 35 Day retention.

Automatic backups also use S3 for storing transaction logs every 5 minutes - allowing for point in time recovery.

Snapshots can be restored .. but create a new RDS instance.

Recovery Time objective, recovery point objective

rpo: max data loss

rto: duration to restore

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 10.38.58 PM.png>)

rds backups (if it's multi rds setup, the primary is never used for backups)

similar to ebs snapshots, first is full, then incremental

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 10.41.04 PM.png>)

manual snapshots are FOREVER, unless you delete them (you determine the rpo, based on frequency of backup)

final snapshot is default if you choose to terminate the instance

auto backups have transaction logs (rpo is 5 mins), retained for max 35 days (0 means auto backup is disabled)

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 10.44.40 PM.png>)

restore = new rds instance w new address (you need to update app config)

![](<../../../.gitbook/assets/Screenshot 2021-07-20 at 10.46.06 PM.png>)

## RDS Read-Replicas

can use replicas but readonly, not the same at standby.

small lag as it's async replication

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 9.12.30 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 9.13.31 PM.png>)

RPO near 0, and low RTO due to read replica promoted quickly to read-write (within mins)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 9.15.24 PM.png>)

### Demo

In this \[DEMO] lesson you will gain experience how how snapshots and restores can be used to recover from data corruption issues.

[1-Click Deployment](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0016-aws-associate-rds-snapshot-and-multiaz/A4L\_WORDPRESS\_AND\_RDS.yaml\&stackName=RDSMULTIAZSNAP)

Deploy Time **\~ 15 mins**

Demo Time **\~90 mins**

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14923954" %}

* create stack
* install wordpress and create post for wordpress instance
* rds, databases, select rds db instance-> take snapshot
* enable multi-az: select rds instance, click on modify, on availability & durability, choose create a standby instance, apply changes now or during maintanence window
* &#x20;simulate a failure by selecting rds instance > reboot > reboot w failover
* simulate a corruption by updating post title to 'Not the best cats'
* select snapshot > restore snapshot (creates new instance)
* update app to use new db instance by instance connect to wordpress instance
* `cd /var/www/html`
* `sudo nano wp-config.php`
* replace with new rds endpoint, and save file
* you can also restore to point in time via auto snapshots (rds instance > restore to point in time)

## Security

Auth, authorization, encryption&#x20;

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.06.14 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.06.45 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.07.36 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.08.20 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.08.35 PM.png>)

## Aurora (no need to choose storage options)

Aurora is a AWS designed database engine officially part of RDS

Aurora implements a number of radical design changes which offer significant performance and feature improvements over other RDS database engines.

This lesson steps through the changes introduced with the Aurora architecture.

replicas are more powerful than read/ standby replicas, can read (and write if multi master)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.10.50 PM.png>)

read/write to cluster vol

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.11.38 PM.png>)

more resilient architecture. if disk fails, aurora repairs w data from other volumes

up to 15 replicas

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.14.53 PM.png>)

reader endpoint will load balance for read operations

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.18.58 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.20.00 PM.png>)

In this \[DEMO] lesson you will migrate an RDS Snapshot created earlier in the course into an Aurora Cluster and verify access by provisioning the Wordpress EC2 instance.

The lesson also introduces the next architectural problem which the course will resolve ... local media storage.

[https://aws.amazon.com/rds/aurora/pricing/](https://aws.amazon.com/rds/aurora/pricing/)

[1-Click Deployment](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0017-aws-associate-rds-migrating-to-aurora/A4L\_WORDPRESS\_AND\_AURORA.yaml\&stackName=Aurora)

Deploy Time **\~30 mins**

Demo Time **\~ 30 mins**

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14954140" %}

* copy ARN of snapshot
* create stack with database restore snapshot (w arn)
* you can migrate snapshot into aurora cluster

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.28.07 PM.png>)

* take snapshot of writer aurora instance
* missing images cos ec2 instance was taken down and new instance has no media



![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.32.03 PM.png>)

## Aurora serverless

DB as a service (kinda), no need to manage individual instances

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.36.50 PM.png>)

ACUs are allocated fro a pool

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.38.39 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.39.25 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.41.28 PM.png>)

{% embed url="https://learn.cantrill.io/courses/730712/lectures/29562431" %}

In this \[DEMO] lesson you will experience how to migrate an Aurora provisioned snapshot into an Aurora serverless cluster. In addition you will see how an aurora serverless cluster can scale down to 0 ACU and pause - meaning the cluster costs will be for storage only. You will experience how when incoming load reaches the serverless cluster it will unpause and allocate ACU to begin servicing requests.

[1-Click Deployment](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0018-aws-associate-rds-migrating-to-aurora-serverless/A4L\_WORDPRESS\_AND\_AURORASERVERLESS.yaml\&stackName=AuroraServerless)

* rds snapshot of aurora provisioned -> copy name to creat stack w database
* alternatively, you can restore an aurora snapshot w severless type, pause compute capacity
* if you ping the app, it may timeout before cluster is up again

## Aurora Global Database

Aurora global databases are a feature of Aurora Provisioned clusters which allow data to be replicated globally providing significant RPO and RTO improvements for BC and DR planning. Additionally global databases can provide performance improvements for customers .. with data being located closer to them, in a read-only form.

Replication occurs at the storage layer and is generally \~1second between all AWS regions.

up to five regions

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.52.54 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.54.29 PM.png>)

## Aurora Multi Master

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.56.06 PM.png>)

no load balancer endpoint, fault tolerant

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.57.33 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.58.32 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 10.58.54 PM.png>)

## Database migration service

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 11.00.54 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-21 at 11.02.12 PM.png>)







