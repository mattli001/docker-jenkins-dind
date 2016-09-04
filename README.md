# Jenkins DinD (Docker in Docker)
This Jenkins Docker image provides Docker inside itself, which allows you to run any Docker container in your Jenkins build script.  It also optionally allows the ability to push to a self-signed secure private registry.

Run it with mounted directory from host:

```
docker run --name jenkins-dind --privileged -d -p 8080:8080 -v /your/jenkins_data_home:/var/lib/jenkins -v /your/docker_data_home -v /var/run/docker.sock:/var/run/docker.sock mattli001/docker-jenkins-dind
```

## Self-signed private registry
When using a private registry with a self-signed certificate, the Docker daemon needs to trust the registry's certificate.  Run the container with the `DOCKER_REGISTRY_CERT` and `DOCKER_REGISTRY_NAME` environment variables set to configure Docker:
```
docker run --name jenkins-dind --privileged -d -p 8080:8080 -e DOCKER_REGISTRY_CERT=/certs/registry.crt -e DOCKER_REGISTRY_NAME=registry:5000 -v /certs:/certs -v /your/path:/var/lib/jenkins killercentury/jenkins-dind
```
This configures Docker to trust the registry at `registry:5000`.

### Private registry + Jenkins
Easily stand up a private registry and jenkins environment by using [roberto/private-registry container](https://hub.docker.com/r/roberto/private-registry/) on Docker Hub.  Once the private registry containers are up and running using [private registry project's docker-compose.yml](https://github.com/rca/private-registry/blob/master/docker-compose.yml) file, use [this project's docker-compose.yml](https://github.com/rca/docker-jenkins-dind/blob/master/docker-compose.yml) file to launch the jenkins container.

# Why use this container
Because Docker container proivdes an isolated environment for running applications or tasks, which is perfect for any CI solution. This image is designed to run everything with Docker, so it doesn't pre-install any execution environment for any specific programming language. Instead, simply run the images you need from the public Docker Hub or your private Docker registry for your CI tasks.

This Docker image is based on [jpetazzo/dind](https://registry.hub.docker.com/u/jpetazzo/dind/) instead of the offical [Jenkins](https://registry.hub.docker.com/u/library/jenkins/). Supervisord is used to make sure everything has proper permission and lanuch in the right order. Morever, [Docker Compose](https://github.com/docker/compose) is available for launching multiple containers inside the CI.
