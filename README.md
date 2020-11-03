[![AMiSNO Logo](images/logo.png)](https://github.com/FourmiPanda/amisno-docker-bootstrap)

  A microservice based project using [netflix oss](https://netflix.github.io/).

![GitHub All Releases](https://img.shields.io/github/downloads/FourmiPanda/amisno-docker-bootstrap/total)
![GitHub issues](https://img.shields.io/github/issues/FourmiPanda/amisno-docker-bootstrap)

## Service Architecture

**AMiSNO** is composed of many microservices written in different languages that talk to each other over http.

[![Architecture of
microservices](images/archi.png)](images/archi.png)

## Installation

This is project uses [docker-compose](https://docs.docker.com/compose/) and docker containers available at our [docker-hub page](https://hub.docker.com/search?q=fourmipanda&type=image).

Before installing, [download and install Docker](https://docs.docker.com/docker-for-windows/install/).
Docker v19.03.12 is required.

Installation is done using the
[`docker-compose up` command](https://docs.docker.com/compose/reference/up/):

```bash
$ docker-compose up
```

Follow [our installing guide](http://expressjs.com/en/starter/installing.html)
for more information.

## Features

  * Robust routing
  * Multiple isolated environments on a single host
  * Preserve volume data when containers are created
  * Only recreate containers that have changed
  * Variables and moving a composition between environments

## Docs & Community

  * [Docker](https://www.docker.com/) for documentation about docker
  * [Gitter](https://microservices.io/) for documentation about microservices

### Security Issues

If you discover a security vulnerability in AMiSNO, please let us know.

## People

The authors of AMiSNO are [FourmiPanda](https://github.com/FourmiPanda) &  [Xabibax](https://github.com/Xabibax)

## License

  [MIT](LICENSE)

