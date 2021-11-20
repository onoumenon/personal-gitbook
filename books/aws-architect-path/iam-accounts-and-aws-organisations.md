# IAM, ACCOUNTS AND AWS ORGANISATIONS

### IAM Policies

![](<../../../.gitbook/assets/Screenshot 2021-06-28 at 10.36.48 PM.png>)

If there's overlap, it's like a venn diagram with priorities (ordered):

* explicit DENY
* Explicit ALLOW
* implicit DENY (by default)

![](<../../../.gitbook/assets/Screenshot 2021-06-28 at 10.40.46 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-28 at 10.41.17 PM.png>)

You may have multiple IAM policies

Inline (most specific, only used in exceptions)

Managed Policy (normal default ACL, reusable, low management overhead)

![](<../../../.gitbook/assets/Screenshot 2021-06-28 at 10.43.10 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-28 at 10.44.20 PM.png>)

can be any named thing that needs long-lived access

![](<../../../.gitbook/assets/Screenshot 2021-06-28 at 10.45.29 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-06-28 at 10.46.56 PM.png>)

arn:aws:s3:::catgifs -> Bucket

arn:aws:s3:::catgifs/\* -> Objects in the bucket

You may need permission for both

![](<../../../.gitbook/assets/Screenshot 2021-06-28 at 10.50.18 PM.png>)

### Demo

* cloudformation
* create stack -> upload template -> enter stack name, parameters, acknowledge
* note: parameter may have restrictions, which may conflict w other restrictions
* physical id is auto generated and unique
* log in as iam user
* for inline policy, find user, add inline policy

![](<../../../.gitbook/assets/Screenshot 2021-06-28 at 10.53.20 PM.png>)

### IAM Groups

* no credential, can't login as group



![](<../../../.gitbook/assets/Screenshot 2021-06-28 at 11.02.33 PM.png>)

### Demo for groups

[https://learn.cantrill.io/courses/730712/lectures/14275294](https://learn.cantrill.io/courses/730712/lectures/14275294)

[1-Click Deployment](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0023-aws-associate-iam-groups/groupsdemoinfrastructure.yaml\&stackName=IAMGROUPS)

In this \[DEMO] we investigate how groups can be used to hold permissions for group members.

Permissions which were assigned to the IAM user 'Sally' are migrated to a new development group we create in the demo.

### IAM ROLES

used when the no of principles is unpredictable, or is short-lived, or external

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.04.20 PM.png>)

Trust policy list the identities that's allowed/ disallowed

then if allowed, it generates temporary security credentials

credentials are checked against permissions policy

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.06.22 PM.png>)

sts:AssumeRole, means IAM roles is involved as the identity assumes the role

### When to use IAM roles

#### LAMBDA

AWS lambda, performs function, has no permission by default.&#x20;

Running it is called lambda invocation.

Create IAM role known as lambda execution role, rather than hardcode access key in lambda function

When lambda function runs, it uses the sts:AssumeRole operation

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.10.38 PM.png>)

hardcode = security risk, hard to rotate

#### HELPDESK scenario (Emergency/ Exception)

readonly access for helpdesk member by default, but emergency occurs and a role can be used.

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.13.05 PM.png>)

#### External Identities (>5000 users)

on-premise sign in, eg: microsoft active directory, with > 5000 staff

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.15.00 PM.png>)

#### Mobile app with a lot of users

web identity federation (eg facebook)

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.16.49 PM.png>)

#### Partner account (Organizations)

can use aws organization, eg: for uploading to a partner account

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.18.26 PM.png>)

### AWS Organizations

large organizations used to have to multiple aws accounts

you can use an account to create the organization

existing accounts can be added to the org

the account used to create the org is the management acct

you can create levels (root, ou)

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.21.38 PM.png>)

org has consolidated billing

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.22.15 PM.png>)

includes all billing from all accounts

benefits from cheaper price at scale

can create an account within an org

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.23.40 PM.png>)

large orgs may separate concerns

management account for billing, and another for IAM

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.24.58 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.25.16 PM.png>)

### Org Demo

[https://learn.cantrill.io/courses/730712/lectures/14293488](https://learn.cantrill.io/courses/730712/lectures/14293488)

In this \[DEMO] Lesson we will create an organisation for the Animals4life business.

The GENERAL account will become the MASTER account for the organisation

We will invite the PRODUCTION account as a MEMBER account and create the DEVELOPMENT account as a MEMBER account.

Finally - we will create an **OrganizationAccountAccessRole** in the production account, and use this role to switch between accounts.

WARNING : If you get an error "You have exceeded the allowed number of AWS Accounts" then you can go here [https://console.aws.amazon.com/servicequotas/home?region=us-east-1#!/services/organizations/quotas/L-29A0C5DF](https://console.aws.amazon.com/servicequotas/home?region=us-east-1#!/services/organizations/quotas/L-29A0C5DF) and request a quote increase for the number of member accounts in an ORG

Need to add role if adding existing account, eg, add general account to role

switch role

provide the account from, to acct to switch role to (prod acct), display name (eg:prod)

### Service control policy

json doc, attached to org / ou / member

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.50.18 PM.png>)

management acct cannot be restricted using service control policy.

don't use management acct to do normal tasks

![](<../../../.gitbook/assets/Screenshot 2021-07-01 at 10.52.07 PM.png>)

limit but can't grant, define the limits

same flow = deny > allow > deny

preferred is a deny list, even if the user has permissions, explicit deny of SCP overrules it

allow list is easy to make mistakes for scp

![](<../../../.gitbook/assets/Screenshot 2021-07-05 at 10.25.08 PM.png>)

only the overlapping policies are actually allowed

### Demo

can create folders for organizations

select PROD from role history

aws organization -> policies -> service control policy -> enable service control policy -> default is full access -> add explicit deny by create -> detach other policies and attach the recently created scp

### Cloudwatch logs

![](<../../../.gitbook/assets/Screenshot 2021-07-05 at 10.40.07 PM.png>)

regional service

logging sources injects data to log events

log events are stored in log stream

log group stores config settings, metric filter, metric triggers alarm

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 8.19.37 PM.png>)

### Cloudtrail

CloudTrail Is a product which logs API calls and account events.

It's very often used to diagnose security or performance issues, or to provide quality account level traceability.

It is enabled by default in AWS accounts and logs free information with a 90 day retention.

It can be configured to store data indefinitely in S3 or CloudWatch Logs.

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 8.21.49 PM.png>)

only management event is logged by default

configure cloudtrail for one region or all regions

services will log to the region they're in.

you can also enable data events to be logged

you can store logs to s3

you can store in cloudwatch logs (more powerful)

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 8.25.56 PM.png>)

can configure organization trail

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 8.27.23 PM.png>)

within 15mins

### Demo

CloudTrail Pricing : [https://aws.amazon.com/cloudtrail/pricing/](https://aws.amazon.com/cloudtrail/pricing/)

CloudWatch Logs Pricing : [https://aws.amazon.com/cloudwatch/pricing/](https://aws.amazon.com/cloudwatch/pricing/)

cloudtrail (free) - 90 days, 1 trail per region, management events but no data events

Cloud trail -> Trails -> trail name, all regions for all accounts in org, use/ create s3 bucket to store logs -> log file encryption -> cloudwatch logs enabled -> choose things to log (management, data, insight) -> read and write enabled

open json log file for cloudtrail

Event history enabled by default

