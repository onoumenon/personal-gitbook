# SIMPLE STORAGE SERVICE \(S3\)

### S3

s3 is private by default

![](../../../.gitbook/assets/screenshot-2021-07-06-at-9.44.44-pm.png)

can allow by principal \(person/ role\), bucket policies can be used to allow access by anonymous

![](../../../.gitbook/assets/screenshot-2021-07-06-at-9.46.39-pm.png)

Bucket Policy Examples : [https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html)

one policy but multiple statements possible

for external acct to access yr bucket, resource policy needs to allow, and their service policy needs to allow

### ACL

less used, and legacy

![](../../../.gitbook/assets/screenshot-2021-07-06-at-9.49.45-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-06-at-9.50.01-pm.png)

### Block public access

another security feature to override your settings

![](../../../.gitbook/assets/screenshot-2021-07-06-at-9.51.13-pm.png)

![](../../../.gitbook/assets/screenshot-2021-07-06-at-9.51.40-pm.png)

### s3 static website hosting

need index and error documents

Accessing S3 is generally done via APIs

Static Website Hosting is a feature of the product which lets you define a HTTP endpoint, set index and error documents and use S3 like a website.

![](../../../.gitbook/assets/screenshot-2021-07-06-at-10.02.55-pm.png)

offloading: dynamic html page using compute service, pings s3 for static files

out-of-band pages: eg if server is down, maintanence page is hosted by s3

![](../../../.gitbook/assets/screenshot-2021-07-06-at-10.05.26-pm.png)

### pricing

per gb month charge

into s3 free

out of s3 per gb

operations in s3 different cost per 1000 operations

cheap

### demo \(s3 web server\)

[https://learn.cantrill.io/courses/730712/lectures/26226206](https://learn.cantrill.io/courses/730712/lectures/26226206)

* go to s3, create bucket \({name}.domain name\)
* uncheck block public access, create bucket
* bucket name needs to be unique
* click bucket, edit static website hosting
* specify index document, error document \(error.html\)
* copy bucket url, upload error.html, index.html
* add folder, add images
* choose whether bucket versioning
* if you go to bucket url, it's 403
* don't use actions &gt; make public
* permission &gt; bucket policy &gt; create bucket policy with allow public
* go to properties to check your url \(you can use route 53 if you have a custom domain\)
* route 53 &gt; simple record &gt; create record \(eg below\)

![](../../../.gitbook/assets/screenshot-2021-07-06-at-10.17.04-pm.png)



