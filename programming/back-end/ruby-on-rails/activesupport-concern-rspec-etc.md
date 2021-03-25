# ActiveSupport Concern, rspec, openURI

{% embed url="http://vaidehijoshi.github.io/blog/2015/10/13/stop-worrying-and-start-being-concerned-activesupport-concerns/" %}

{% embed url="https://stackoverflow.com/questions/19556296/whats-the-difference-between-include-examples-and-it-behaves-like" %}

{% embed url="https://stackoverflow.com/questions/3193538/retrieve-contents-of-url-as-string" %}

{% embed url="https://ruby-doc.org/stdlib-2.6.3/libdoc/open-uri/rdoc/OpenURI.html" %}

```text
require 'open-uri'

URI.parse(url).open
```



To run methods based on model events:

```text
  before_validation :do_something, on: :create
  before_create :do_something_else
  after_create :run_async_jobs
```

to perform jobs later:  


```text
Job.perform_later()
```

