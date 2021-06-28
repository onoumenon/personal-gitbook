# IAM, ACCOUNTS AND AWS ORGANISATIONS

### IAM Policies

![](../../../.gitbook/assets/screenshot-2021-06-28-at-10.36.48-pm.png)

If there's overlap, it's like a venn diagram with priorities \(ordered\):

* explicit DENY
* Explicit ALLOW
* implicit DENY \(by default\)

![](../../../.gitbook/assets/screenshot-2021-06-28-at-10.40.46-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-28-at-10.41.17-pm.png)

You may have multiple IAM policies

Inline \(most specific, only used in exceptions\)

Managed Policy \(normal default ACL, reusable, low management overhead\)

![](../../../.gitbook/assets/screenshot-2021-06-28-at-10.43.10-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-28-at-10.44.20-pm.png)

can be any named thing that needs long-lived access

![](../../../.gitbook/assets/screenshot-2021-06-28-at-10.45.29-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-28-at-10.46.56-pm.png)

arn:aws:s3:::catgifs -&gt; Bucket

arn:aws:s3:::catgifs/\* -&gt; Objects in the bucket

You may need permission for both

![](../../../.gitbook/assets/screenshot-2021-06-28-at-10.50.18-pm.png)

### Demo

* cloudformation
* create stack -&gt; upload template -&gt; enter stack name, parameters, acknowledge
* note: parameter may have restrictions, which may conflict w other restrictions
* physical id is auto generated and unique
* log in as iam user
* for inline policy, find user, add inline policy

![](../../../.gitbook/assets/screenshot-2021-06-28-at-10.53.20-pm.png)

### IAM Groups

* no credential, can't login as group



![](../../../.gitbook/assets/screenshot-2021-06-28-at-11.02.33-pm.png)

