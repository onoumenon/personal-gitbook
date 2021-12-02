# Simple Wordpress with AWS Lightsail

{% embed url="https://aws.amazon.com/getting-started/hands-on/launch-a-wordpress-website/" %}

`cat bitnami_credentials` to get wordpress admin credentials and go to static ip + `/admin` for access to wp dashboard

#### HTTPS

{% embed url="https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-enabling-https-on-wordpress" %}

{% embed url="https://medium.com/@reggirl32/set-up-a-wordpress-multisite-using-an-amazon-web-services-ec2-instance-part-1-bdf128b1cf4f" %}

{% embed url="https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-editing-wp-config-for-distribution" %}

{% embed url="https://docs.bitnami.com/ibm/apps/wordpress-multisite/configuration/configure-wordpress-multisite" %}

{% embed url="https://community.bitnami.com/t/unable-to-change-main-sites-site-address-url-is-this-the-cause-to-site-health-error/70895" %}

{% embed url="https://smallbusiness.chron.com/differences-between-primary-alias-domain-73189.html" %}

```
mysql -u root -p -D bitnami_wordpress -e "UPDATE wp_options SET option_value = 'https://YOURDOMAIN.COM' WHERE option_name = 'siteurl' or option_name = 'home';"
```

#### 301 redirects

{% embed url="https://www.searchenginejournal.com/301-vs-302-redirects-seo/299843" %}

{% embed url="https://www.rapturezone.com/wordpress-multisite-on-amazon-lightsail-aws" %}

{% embed url="https://scripteverything.com/multiple-wordpress-installs-on-one-lightsail-bitnami-wordpress-box" %}

{% embed url="https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-define-the-primary-domain-for-your-wordpress-multisite" %}

{% embed url="https://github.com/awsdocs/amazon-lightsail-developer-guide" %}

{% embed url="https://aws.amazon.com/blogs/compute/using-load-balancers-on-amazon-lightsail" %}

{% embed url="https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-add-blogs-as-domains-to-your-wordpress-multisite" %}

{% embed url="https://www.chrisjmendez.com/2017/05/04/bitnami-wordpress-cheatsheet" %}



{% embed url="https://docs.bitnami.com/aws/apps/wordpress/administration/use-single-domain" %}
