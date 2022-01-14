# Example balenaEngine docker image

[balenaEngine](https://github.com/balena-os/balena-engine) is a container engine purpose-built for embedded and IoT use cases and compatible with Docker containers.

## Build

This example is not published on any repo so you'll need to build it yourself.

```bash
# build local image for native platform
export DOCKER_BUILDKIT=1
export DOCKER_CLI_EXPERIMENTAL=enabled
docker build . --tag balena-engine

# OR cross-build for another platform
export DOCKER_BUILDKIT=1
export DOCKER_CLI_EXPERIMENTAL=enabled
docker run --rm --privileged multiarch/qemu-user-static:5.2.0-2 --reset -p yes
docker build . --tag balena-engine --platform linux/arm/v7

# OR build with docker-compose
export DOCKER_BUILDKIT=1
export DOCKER_CLI_EXPERIMENTAL=enabled
export COMPOSE_DOCKER_CLI_BUILD=1
docker-compose build balena
```

## Test

```bash
# run simple "info" and "hello-world" tests
docker run -d --privileged --name balena -p "2375:2375" balena-engine
DOCKER_HOST=tcp://127.0.0.1:2375 docker info
DOCKER_HOST=tcp://127.0.0.1:2375 docker run hello-world
docker rm --force balena

# OR test with docker-compose
export COMPOSE_DOCKER_CLI_BUILD=1
docker-compose up --build info
docker-compose up --build hello-world
docker-compose down
```

## Usage

Much like [Docker-in-Docker](https://hub.docker.com/_/docker), elevated permissions are required for running balenaEngine in a container.

```bash
# print usage flags
docker run --rm balena-engine --help

# run with volatile storage
docker run --rm -it --privileged \
  -p "2375:2375" balena-engine

# run with persistent data volume
docker run --rm -it --privileged \
  -v "balena:/var/lib/balena-engine"
  -p "2375:2375" balena-engine

# run in the background as a service (detached)
docker run -d --privileged \
  -v "balena:/var/lib/balena-engine"
  -p "2375:2375" balena-engine
```

See the included example [docker-compose.yml](docker-compose.yml) file for examples running alongside other services.

## Contributing

Please open an issue or submit a pull request with any features, fixes, or changes.
