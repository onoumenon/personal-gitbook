# Docker vs Docker compose vs Kubenetes

{% embed url="https://www.techrepublic.com/article/simplifying-the-mystery-when-to-use-docker-docker-compose-and-kubernetes" %}

## TLDR:

Docker (or specifically, the _docker_ command) is used to manage individual containers, _docker-compose_ is used to manage multi-container applications and Kubernetes is a container orchestration tool.



* Docker is (in many cases) the core technology used for containers and can deploy single, containerized applications
* Docker Compose is used for configuring and starting multiple Docker containers on the same host--so you don't have to start each container separately
* Docker swarm is a container orchestration tool that allows you to run and connect containers on multiple hosts
* Kubernetes is a container orchestration tool that is similar to Docker swarm, but has a wider appeal due to its ease of automation and ability to handle higher demand



Use cases:

* Docker - when you want to deploy a single (network accessible) container
* Docker Compose - when you want to deploy multiple containers to a single host from within a single YAML file
* Docker swarm - when you want to deploy a cluster of docker nodes (multiple hosts) for a simple, scalable application
* Kubernetes - when you need to manage a large deployment of scalable, automated containers

