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



