# Active Jobs & Sidekiq

{% embed url="https://guides.rubyonrails.org/active_job_basics.html" %}

## Queue adapter, Sidekiq

(there's other options here: [https://api.rubyonrails.org/v6.1.3/classes/ActiveJob/QueueAdapters.html](https://api.rubyonrails.org/v6.1.3/classes/ActiveJob/QueueAdapters.html))

{% embed url="https://github.com/mperham/sidekiq" %}

{% embed url="https://github.com/mperham/sidekiq/wiki/Active-Job" %}

{% embed url="https://gorails.com/episodes/testing-ruby-on-rails-active-job" %}



{% code title="applications.rb" %}
```
    config.active_job.queue_adapter = :sidekiq
    config.active_job.queue_name_prefix = Rails.env
```
{% endcode %}
