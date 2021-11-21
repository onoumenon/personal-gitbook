# Enable s3

#### Add setup

* add 'aws-sdk' gem
* add secret keys
* add storage config in storage.yml

{% code title="storage.yml" %}
```
amazon:
  service: S3
  access_key_id: <%= Rails.application.credentials.dig(:aws, :access_key_id) %>
  secret_access_key: <%= Rails.application.credentials.dig(:aws, :secret_access_key) %>
  bucket: <%= Rails.application.credentials.dig(:aws, :s3_bucket)  %>
  region: ap-southeast-1

```
{% endcode %}



#### Add presigned url

{% embed url="https://medium.com/@aidan.hallett/securing-aws-s3-uploads-using-presigned-urls-aa821c13ae8d" %}

* add the api route for creating the presigned url

{% code title="routes.rb" %}
```
resources :upload_url, only: [:create]
```
{% endcode %}

* add aws config
* create service and controller

In service, validates params (eg: filename is present using `include ActiveModel::Validations`)

If self.valid, get and return a presigned url by creating a resource in your aws bucket:

{% embed url="https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/S3/Resource.html" %}



```
    bucket_url = Rails.application.credentials.aws[:s3_bucket]
    bucket = Aws::S3::Resource.new.bucket(bucket_url)
    presigned_url = bucket.presigned_post(
      key: key,
      success_action_status: '201',
      acl: 'public-read',
      cache_control: "max-age=86400"
    )
```

#### Add the model and table for the media

* eg: should have valid url, created\_by, itemable
* should have right associations

```
    resources :documents, only: %i[update show] do
      resources :media, only: [:create]
    end
```

Other resources

{% embed url="http://ruby-metaprogramming.rubylearning.com/html/ruby_metaprogramming_2.html" %}

{% embed url="https://www.rubyguides.com/2017/06/ruby-struct-and-openstruct/" %}

{% embed url="https://www.rubyguides.com/2019/10/scopes-in-ruby-on-rails/" %}

