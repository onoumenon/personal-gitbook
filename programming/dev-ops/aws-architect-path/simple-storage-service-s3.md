# SIMPLE STORAGE SERVICE (S3)

### S3

s3 is private by default

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 9.44.44 PM.png>)

can allow by principal (person/ role), bucket policies can be used to allow access by anonymous

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 9.46.39 PM.png>)

Bucket Policy Examples : [https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html)

one policy but multiple statements possible

for external acct to access yr bucket, resource policy needs to allow, and their service policy needs to allow

### ACL

less used, and legacy

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 9.49.45 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 9.50.01 PM.png>)

### Block public access

another security feature to override your settings

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 9.51.13 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 9.51.40 PM.png>)

### s3 static website hosting

need index and error documents

Accessing S3 is generally done via APIs

Static Website Hosting is a feature of the product which lets you define a HTTP endpoint, set index and error documents and use S3 like a website.

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.02.55 PM.png>)

offloading: dynamic html page using compute service, pings s3 for static files

out-of-band pages: eg if server is down, maintanence page is hosted by s3

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.05.26 PM.png>)

### pricing

per gb month charge

into s3 free

out of s3 per gb

operations in s3 different cost per 1000 operations

cheap

### demo (s3 web server)

[https://learn.cantrill.io/courses/730712/lectures/26226206](https://learn.cantrill.io/courses/730712/lectures/26226206)

* go to s3, create bucket ({name}.domain name)
* uncheck block public access, create bucket
* bucket name needs to be unique
* click bucket, edit static website hosting
* specify index document, error document (error.html)
* copy bucket url, upload error.html, index.html
* add folder, add images
* choose whether bucket versioning
* if you go to bucket url, it's 403
* don't use actions > make public
* permission > bucket policy > create bucket policy with allow public
* go to properties to check your url (you can use route 53 if you have a custom domain)
* route 53 > simple record > create record (eg below)

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.17.04 PM.png>)

### Object versioning

default is disabled, but once enabled, can't be disabled, only suspended

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.26.56 PM.png>)

versioning allocates id, operations that modify objects generates new ver

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.28.33 PM.png>)

if you delete an obj, you create a delete marker

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.29.03 PM.png>)

You can delete a specific version

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.30.07 PM.png>)

### MFA delete

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.30.44 PM.png>)

### You can overwrite files by names without versioning

### Demo

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14305172" %}

* set up s3 bucket
* enable versioning
* replace a file with the same name with another file
* looks like it's overwritten, but you can toggle `list versions`
* delete objects adds delete markers to them
* to undelete object, delete the delete marker
* to permanently delete object at version, delete the version
* you can suspend, but it only stops new ver from being saved

### s3 optimization

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.36.48 PM.png>)

This lesson reviews how S3 Uploads (PutObject) works

Single PUT Upload (not recommended if >100mb)

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.38.43 PM.png>)

Multipart Upload

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.40.02 PM.png>)

Finishing up by reviewing how S3 Transfer Acceleration works and how it could benefit Animals4life remote workers when uploading large data sets.

no control over internet paths

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.41.28 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.41.57 PM.png>)

S3 Transfer Acceleration is off by default

{% hint style="info" %}
Bucket names cannot contain periods, need to be DNS compatible&#x20;
{% endhint %}

remote worker connects to -> edge location uses-> aws global network to link to ->s3 bucket

![](<../../../.gitbook/assets/Screenshot 2021-07-06 at 10.45.24 PM.png>)

the further the bucket is, the better the improvement

### Demo

* create bucket
* select bucket > transfer acceleration, get endpoint (edge location)
* check speed with aws test

AWS Accelerated Transfer Tool : [http://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html](http://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html)

### Encryption

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.07.17 PM.png>)

Difference between encryption at rest, and encryption in transit.

at rest: 'local', one entity

in transit: encryption tunnel (outsiders can't see), multiple entities

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.08.59 PM.png>)

plaintext: unencrypted data, can be doc, image, or app

algorithm: math/ code that takes secret and encrypts it with an algorithm

key: cipher

ciphertext: encrypted data

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.11.33 PM.png>)

Symmetric encryption

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.13.01 PM.png>)

symmetric encryption is not ideal for transit encyption, because you need a way to transfer the key

assymetric: choose algo, both make pub and priv keys, priv key needs to be used to decrypt stuff encrypted w pub key.&#x20;

upload pub key to be shared. the other party encrypt w pub key.

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.20.13 PM.png>)

cat only knows robot has received by 'ok'

'ok' can be proven to be from robot with signing w priv key.

use pub key to check if ok is signed w priv key

### steganography

hiding that you are hiding (encrypting) sth -- plausible deniability

algo can't easily be detected unless you know the algo (eg: image pixel changes)

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.25.01 PM.png>)

### KMS (key management service)

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.26.42 PM.png>)

comply FIPS 140 level 2

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.28.05 PM.png>)

* create a key, customer master key
* kms encrypts cmk
* client makes a encrypt call (using permission)
* client decrypts file (using permission)
* cmk NEVER leaves kms

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.30.12 PM.png>)

dek is created w cmk, bypass 4kb limit, linked to a specific cmk

kms does not store dek, it creates and then discards it

it stores the encrypted key w data

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.33.13 PM.png>)

aws managed (using kms) - cmk rotation is mandatory

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.35.13 PM.png>)

key policy is on cmk

![](<../../../.gitbook/assets/Screenshot 2021-07-07 at 10.36.18 PM.png>)

you need both key policy and iam policy

you can be permissioned to only create key but not encrypt/decrypt

### Demo

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14368224" %}

* kms, customer managed key, kms
* alias for key
* key admin, this is the role that can create and delete key
* key usage, role to decrypt and encrypt
* review key policy
* encypt w cmk

Note : the commands required will be different based on using 1) Windows 10, or 2) macOS/Linux

`# Shared`\
`echo "find all the doggos, distract them with the yumz" > battleplans.txt`&#x20;

`Windows Commands` \
`aws kms encrypt --key-id alias/catrobot --plaintext fileb://battleplans.txt --output text --profile iamadmin-general --query CiphertextBlob > battleplans.base64` \
``\
`certutil -decode battleplans.base64 not_battleplans.enc` \
``\
`aws kms decrypt --ciphertext-blob fileb://not_battleplans.enc --output text --profile iamadmin-general --query Plaintext > decreyptedplans.base64` \
``\
`certutil -decode decreyptedplans.base64 decryptedplans.txt #`&#x20;

`Linux/macOS commands` \
`aws kms encrypt \` \
&#x20;`--key-id alias/catrobot \`\
&#x20;`--plaintext fileb://battleplans.txt \`\
&#x20;`--output text \`\
&#x20;`--query CiphertextBlob \`\
&#x20;`--profile iamadmin-general | base64 \`\
&#x20;`--decode > not_battleplans.enc` \
``\
`aws kms decrypt \`\
&#x20;`--ciphertext-blob fileb://not_battleplans.enc \`\
&#x20;`--output text \`\
&#x20;`--profile iamadmin-general \`\
&#x20;`--query Plaintext | base64 --decode > decryptedplans.txt`

some services creates aws managed keys

### Object encryption

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 9.46.51 PM.png>)

Encryption at rest:

client-side means it's uploaded encrypted

server-side means user uploads in plain text but aws encrypts it&#x20;

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 9.49.24 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 9.50.27 PM.png>)

SSE-C: offload the encryption process to aws, you supply the key and plaintext

aws discard keys after, good for heavy regulated applications, but you NEED TO MANAGE KEYS

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 9.52.23 PM.png>)

SSE-S3: aws manages encryption and keys, creates master key which is not visible. key for object encryoption is created and encrypted w master key. plaintext object key is discarded, and encrypted object key is stored w object.

Default, but not suitable if you are heavily regulated, need control over keys or rotation or role separation (eg diff sets of permission)&#x20;

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 9.54.54 PM.png>)

you can manage the cons of SSE-S3 by using SSE KMS

master key is a CMK stored in KMS , which you have control over

you can put permissions on cmk, so s3 admin don't have decrypting access to s3 obj unless otherwise specified



![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 9.58.08 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.01.55 PM.png>)

header set to enable encryption server side: x-amz-server-side-encryption, per object

Bucket default encryption: default encryption used unless otherwise specified

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.05.03 PM.png>)

### Demo

{% embed url="https://learn.cantrill.io/courses/730712/lectures/14391333" %}

KMS, create cmk key, symmetric key, don't set key policy atm

upload img, enable server side encyption, if kms, choose key

open iam, open user, deny user kms:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Deny",
            "Action": "kms:*",
            "Resource": "*"
        }
    ]
}
```

edit default encryption: on bucket

### S3 storage classes

### Standard

default : s3 standard, az resilient

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.16.13 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.16.31 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.17.19 PM.png>)

s3 standard: frequent access and non replaceable

### Standard-IA

cheaper than standard, storage cost

retrieval fee is expensive, for infrequently access

min duration 30days billing

min capacity of object is 128kb (not cost effective for small objects)

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.18.56 PM.png>)

### one zone-IA

diff w standard IA is one AZ storage, durable cept AZ failure, data is non critical and replaceable, don't use if only one copy

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.21.18 PM.png>)

### Glacial&#x20;

1/5 of cost of standard

very infrequent access, cold obj (not immediate access), when retrieved, it goes to IA temporarily

first btye latency of minutes or hours

archival data

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.23.31 PM.png>)

### Glacial deep archive

cheapest, 'frozen' state, retrieval time is much longer

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.25.17 PM.png>)

### Intelligent tiering

contains 4 different tiers

auto migrating based on metrics

same costs for storing + monitoring costs&#x20;

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.27.08 PM.png>)

long-lived data and changing/ unknown usage patterns

## s3 lifecycle config

transition objects between storage tiers by duration,&#x20;

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.29.04 PM.png>)

based on duration not on usage

waterfall (transition is one way)

restrictions:

small objects can cost more (min size)

min of 30 days before transition on standard

cannot transition multiple times within 30 days in single rule

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.31.45 PM.png>)

management tab on bucket

scope of rule (bucket -all, or certain objs)

expire - prev ver to delete after days

## s3 replication

![](<../../../.gitbook/assets/Screenshot 2021-07-08 at 10.35.41 PM.png>)

differs only in same or diff aws accounts

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 8.36.01 PM.png>)

{% hint style="info" %}
same account: IAM role has access if configed

diff account: require bucket policy for dest bucket to allow role
{% endhint %}

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 8.37.03 PM.png>)

### Replication Options

default is use source bucket storage class

RTC for syncing

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 8.38.44 PM.png>)

{% hint style="info" %}
cannot replicate if no versioning, and can only replicate versions from this point on (not retrospective)
{% endhint %}

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 8.41.13 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 8.43.41 PM.png>)

### Demo

In this \[DEMO] We create 2 S3 buckets - one in N. Virginia, the other in N. California and configure Cross-Region Replication (CRR) between the two.

* create two public buckets (source, destinatiion)
* bucket > management > create replication rule -> enable versioning
* configure rule (source, what objects, destination, iam role, encryption, storage class, etc)
* iam role: create new role unless one is previously created
* by default, delete markers are not replciated
* check both buckets after upload to source bucket

## Presigned url

Problem: without presigned url, you'll need to give aws identity/credentials to the unauthenticated user or make bucket/ object public, which are ALL not ideal

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.07.54 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.09.40 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.11.39 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.13.13 PM.png>)

{% hint style="info" %}
Always use long term creds, ie: IAM user
{% endhint %}

Demo: [https://learn.cantrill.io/courses/730712/lectures/14305207](https://learn.cantrill.io/courses/730712/lectures/14305207)

* cloudshell, creds are your aws account
* `aws s3 ls`
* `aws s3 presign {{URI}} --expires-in 180`
* &#x20;copy presigned url
* paste (the url is accessible for 180 secs)

presigned url that is not expired but if the identity can't access the object anymore, the presigned url will also not be able to.

## s3 select and glacier select

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.24.21 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.25.26 PM.png>)

## event notifications

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.26.54 PM.png>)

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.27.46 PM.png>)

## access logs

![](<../../../.gitbook/assets/Screenshot 2021-07-12 at 9.29.03 PM.png>)

