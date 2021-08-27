# GLOBAL CONTENT DELIVERY AND OPTIMIZATION

client is actually acccessing cache in locations closer to them with cloudfront

![](../../../.gitbook/assets/screenshot-2021-07-30-at-11.28.28-pm.png)

origin can be anything, is location of your content, as long as it's on internet

config of cloudfront is in 'distribution'

edge locations are generally in larger cities, smaller than aws regions, generally one or more racks in 3rd party distributor

![](../../../.gitbook/assets/screenshot-2021-07-30-at-11.33.05-pm.png)

config distribution to have s3 bucket as origin

the domain name for cloudfront edge locations is unique to your distribution

if not at local edge location, will check regional edge location, otherwise origin fetch

![](../../../.gitbook/assets/screenshot-2021-07-30-at-11.37.14-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-30-at-11.37.40-pm.png)

![you can use cloudfront to add https capability to static website hosted in s3 \(acm is aws cert manager\)  ](../../../.gitbook/assets/screenshot-2021-07-30-at-11.38.27-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-30-at-11.39.44-pm.png)

caching is done on query string level

you can remove the benefit of caching if you configure query string params wrongly!

Choose NO for forward to origin of query string params if you don't need it cached at query string level

![](../../../.gitbook/assets/screenshot-2021-07-30-at-11.43.57-pm.png)

## ACM - AWS Certificate manager

certificates are certs that allows HTTPs website handle encryption and prove its identity

![](../../../.gitbook/assets/screenshot-2021-07-30-at-11.58.52-pm.png)

if it's not a managed service but self managed, generally acm doesn't support it

![](../../../.gitbook/assets/screenshot-2021-07-31-at-4.19.16-pm.png)

### Demo

[1-Click Deployment](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0026-aws-associate-cdn-cloudfront-and-s3/top10catsbucket.yaml&stackName=CFANDS3) \(**You will be using this infrastructure for this demo lesson and the next two - please DON'T delete it at the end of the lesson**\)

{% embed url="https://Whiskers.jpg" %}

* create stack
* go to resource -&gt; see that there's s3 bucket for cats which has public read access and is config as a static website \(you'll notice that it's using HTTP, and is in US\_EAST1\)
* cloudfront -&gt; create distribution -&gt; web distribution
* origin domain name: choose top 10 cats bucket, skip origin path \(unless you want it to start not at root but other path\)
* restrict bucket access \(no for now, but not ideal\)
* viewer protocol policy \(redirect http to https\)
* use a cache policy and origin request policy
* don't set restrict viewer access for now, if set, viewers must use signed urls or cookies \(good for private distribution\)
* price class: can limit it to certain geographical locations since cheaper
* firewall \(AWS WAF\)
* SSL cert: default cloudfront cert
* Default Root object: index.html \(impt\)
* create
* click on distribution id -&gt; domain name \(you can access s3 bucket via cloudfront edge location\)
* download whiskers.jpg -&gt; rename to merlin.jpg
* overwrite existing merlin.jpg with above file
* if you go through cloudfront domain, you will still see old image, as it's cached and not refreshed till TTL expires
* you can get around the issue by going to the distribution -&gt; create invalidation -&gt; add object path 
* you are billed per invalidation, so it's better to perform bigger invalidations less frequently
* use versioned objects if you always need to update object
* you can use https via domain name with https://
* that's because it's using the default cloudfront cert

![](../../../.gitbook/assets/screenshot-2021-07-31-at-4.41.19-pm.png)

you'll be adding your own custom ssl cert and making sure viewers can't directly access s3 bucket in the next part

* register domain name w route 53
* distribution id -&gt; request or import certificate, add merlin.anims4life.org \(use your own domain name\)
* validate you own domain \(dns validation, since you have access via r53\)
* review and confirm
* create record in R53
* edit distribution, domain name: merlin.animals4life.org, use custom cert you've created
* route 53, hosted zone, create record -&gt; simple routing -&gt; record name: merlin, route traffic to: cloudfront distribution, create record

## Securing s3 using an Origin Access Identity \(OAI\)



