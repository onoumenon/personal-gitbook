# Terraform + AWS

{% embed url="https://learn.hashicorp.com/tutorials/terraform/install-cli?in=terraform/aws-get-started" %}

{% embed url="https://dev.to/techworld\_with\_nana/terraform-simply-explained-m" %}

TLDR:

Terraform has 2 main components:

* **CORE**

Terraform's Core takes two input sources, which are your configuration files \(your desired state\) and second the current state \(which is managed by Terraform\).  
With this information the Core then creates a plan of what resources needs to be created/changed/removed.

* **Provider**

The second part of the Architecture are providers. Providers can be **IaaS** \(like AWS, GCP, Azure\), PaaS \(like Heroku, Kubernetes\) or SaaS services \(like Cloudflare\).  
Providers expose resources, which makes it possible to create infrastructure across all this platforms.

![Terraform Architecture](https://res.cloudinary.com/practicaldev/image/fetch/s--Qk9SZk59--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/xxva0ol9ysbv8blan4e6.png)

![Terraform Commands](https://res.cloudinary.com/practicaldev/image/fetch/s--8iTvETvo--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/d3eh00yiowys24y85juh.png)

#### Providers

The `provider` block configures the named provider, in our case `aws`, which is responsible for creating and managing resources. 

#### Resources

The `resource` block defines a piece of infrastructure. A resource might be a physical component such as an EC2 instance, or it can be a logical resource such as a Heroku application.



### Steps

Create a folder, cd into it and add your terraform config, named `main.tf`

```text
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 2.70"
    }
  }
}

provider "aws" {
  profile = "default"
  region  = "us-west-2"
}

resource "aws_instance" "example" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"
}
```

`terraform init`

`terraform validate`

`terraform apply`

`terraform destroy`

### Destroy

The `terraform destroy` command terminates resources defined in your Terraform configuration. This command is the reverse of `terraform apply` in that it terminates all the resources specified by the configuration. It does _not_ destroy resources running elsewhere that are not described in the current configuration.

{% embed url="https://learn.hashicorp.com/tutorials/terraform/aws-variables?in=terraform/aws-get-started" %}

Terraform automatically loads all files in the current directory with the exact name of `terraform.tfvars` or any variation of `*.auto.tfvars`. If the file is named something else, you can use the `-var-file` flag to specify a file name. These files use the same syntax as Terraform configuration files \(HCL\). And like Terraform configuration files, these files can also be JSON.

We don't recommend saving usernames and passwords to version control. You can create a local file with a name like `secret.tfvars` and use `-var-file` flag to load it.

```text
terraform apply \
  -var-file="secret.tfvars" \
  -var-file="production.tfvars"
```

{% embed url="https://www.docker.com/resources/what-container" %}

{% embed url="https://www.cio.com/article/2924995/what-are-containers-and-why-do-you-need-them.html" %}

Problems arise when the supporting software environment is not identical, says Docker creator Solomon Hykes. "You're going to test using Python 2.7, and then it's going to run on Python 3 in production and something weird will happen. Or you'll rely on the behavior of a certain version of an SSL library and another one will be installed. You'll run your tests on Debian and production is on Red Hat and all sorts of weird things happen."

TLDR: 

* Consistency
* Modular
* Smaller than VMs

{% embed url="https://www.nginx.com/resources/glossary/nginx/" %}

TLDR: 

* used for web serving, reverse proxying, caching, load balancing, media streaming, etc
* originally created to solve 10k concurrency issue \( [C10K problem](https://en.wikipedia.org/wiki/C10k_problem)\)

### Web server

_The primary function of a web server is to store, process and deliver_ [_web pages_](https://en.wikipedia.org/wiki/Web_page)_/ content to clients_.



#### Basic common features

Although web server programs differ in how they are implemented, most of them offer the following basic common features.

* [HTTP](https://en.wikipedia.org/wiki/HTTP): support for one or more versions of HTTP protocol in order to send versions of HTTP responses compatible with versions of client HTTP requests, e.g. HTTP/1.0, HTTP/1.1 plus, if available, [HTTP/2](https://en.wikipedia.org/wiki/HTTP/2), [HTTP/3](https://en.wikipedia.org/wiki/HTTP/3);
* [Logging](https://en.wikipedia.org/wiki/Data_logging): usually web servers have also the capability of logging some information, about client requests and server responses, to [log files](https://en.wikipedia.org/wiki/Server_log) for security and statistical purposes.

A few other popular features \(only a very short selection\) are:

* [Authentication](https://en.wikipedia.org/wiki/Authentication), optional support for [authorization](https://en.wikipedia.org/wiki/Authorization) request \(request of [user name](https://en.wikipedia.org/wiki/User_name) and [password](https://en.wikipedia.org/wiki/Password)\) before allowing access to some or all kind of website resources.
* [Large file support](https://en.wikipedia.org/wiki/Large_file_support) to be able to serve files whose size is greater than 2 GB on 32 bit [OS](https://en.wikipedia.org/wiki/Operating_system).
* [Bandwidth throttling](https://en.wikipedia.org/wiki/Bandwidth_throttling) to limit the speed of responses in order to not saturate the network and to be able to serve more clients.



