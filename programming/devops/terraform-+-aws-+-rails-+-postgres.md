# Terraform + AWS + Rails + Postgres

{% hint style="info" %}
{variable} indicates a variable to be replaced
{% endhint %}

### Check if server and postgres can run

{% embed url="https://medium.com/@mshostdrive/how-to-run-a-rails-app-in-production-locally-f29f6556d786" %}

```text
RAILS_ENV=production rails s
```

```text
RAILS_ENV=production rake db:create db:migrate db:seed
```

Open postico to check database, where eg: user = {username}, database = {db name} 

### Check if docker image can run locally

```bash
docker network create --attachable {name}

docker run -d --restart unless-stopped --name {database_name} \
  --network {name} \
  --env POSTGRES_DB={name_env} \
  --env POSTGRES_USER={user} \
  --env POSTGRES_PASSWORD={password} \
  postgres:12.3-alpine

docker run -d --name {web} \
  --network {name} \
  -p 3000:3000 \
  --env FITME_DATABASE_USERNAME={user} \
  --env FITME_DATABASE_PASSWORD={password} \
  --env DB_HOST={database_name} \
  --env RAILS_MASTER_KEY={master_key} \
  --cap-drop ALL \
  fitme
```

### Manually send the docker image to ECR

```bash
# Authenticate with AWS CLI
aws configure

# Authenticate ECR
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin {eg: 352148427474.dkr.ecr.ap-southeast-1.amazonaws.com}

# Tag Docker
docker tag {project_name}:latest {eg: 352148427474.dkr.ecr.ap-southeast-1.amazonaws.com}/{project_name}:latest

# Push image
docker push {eg: 352148427474.dkr.ecr.ap-southeast-1.amazonaws.com}/{project_name}:latest
```

`:latest` can be replaced by git SHA for CircleCi integration later

### Run terraform to boot up an EC2 instance

```bash
provider "aws" {
  region  = "ap-southeast-1"
  version = "~> 2.0"
}

terraform {
  backend "s3" {
    bucket = "{project_name}-tf-state"
    key    = "setup/terraform.tfstate"
    region = "{eg: ap-southeast-1}"

    dynamodb_table = "{project_name}-tf-running-locks"
    encrypt        = true
  }
}

resource "aws_ecr_repository" "repo" {
  name = "{project_name}"

  image_scanning_configuration {
    scan_on_push = true
  }
}

resource "aws_ecr_lifecycle_policy" "lifecycle_policy" {
  repository = aws_ecr_repository.repo.name

  policy = <<EOF
    {
        "rules": [
            {
                "rulePriority": 1,
                "description": "Keep last 30 images",
                "selection": {
                    "tagStatus": "any",
                    "countType": "imageCountMoreThan",
                    "countNumber": 30
                },
                "action": {
                    "type": "expire"
                }
            }
        ]
    }
    EOF
}
```

### SSH into the EC2 instance in AWS and try to run docker inside

Get the key-pair from lastpass, and rename it to `{devname}.pem`.

To ssh into the server:

Add security group with SSH rule added

```bash
chmod 400 sj_developer.pem
ssh -i {devname}.pem ec2-user@{server-public-ip}
```

To run Rails Console once SSH in the server:

```bash
docker exec -it web sh
bin/rails console
```

