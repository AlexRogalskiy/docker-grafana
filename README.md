# github.com/tiredofit/docker-grafana

[![GitHub release](https://img.shields.io/github/v/tag/tiredofit/docker-grafana?style=flat-square)](https://github.com/tiredofit/docker-grafana/releases/latest)
[![Build Status](https://img.shields.io/github/workflow/status/tiredofit/docker-grafana/build?style=flat-square)](https://github.com/tiredofit/docker-grafana/actions?query=workflow%3Abuild)
[![Docker Stars](https://img.shields.io/docker/stars/tiredofit/grafana.svg?style=flat-square&logo=docker)](https://hub.docker.com/r/tiredofit/grafana/)
[![Docker Pulls](https://img.shields.io/docker/pulls/tiredofit/grafana.svg?style=flat-square&logo=docker)](https://hub.docker.com/r/tiredofit/grafana/)
[![Become a sponsor](https://img.shields.io/badge/sponsor-tiredofit-181717.svg?logo=github&style=flat-square)](https://github.com/sponsors/tiredofit)
[![Paypal Donate](https://img.shields.io/badge/donate-paypal-00457c.svg?logo=paypal&style=flat-square)](https://www.paypal.me/tiredofit)

## About

This will allow you to build a Docker image for Grafana.

## Maintainer

- [Dave Conroy](http://github/tiredofit/)

## Table of Contents

- [About](#about)
- [Maintainer](#maintainer)
- [Table of Contents](#table-of-contents)
- [Prerequisites and Assumptions](#prerequisites-and-assumptions)
- [Installation](#installation)
  - [Build from Source](#build-from-source)
  - [Prebuilt Images](#prebuilt-images)
    - [Multi Archictecture](#multi-archictecture)
- [Configuration](#configuration)
  - [Quick Start](#quick-start)
  - [Persistent Storage](#persistent-storage)
  - [Environment Variables](#environment-variables)
    - [Base Images used](#base-images-used)
    - [Docker Secrets](#docker-secrets)
- [Maintenance](#maintenance)
  - [Shell Access](#shell-access)
- [Support](#support)
  - [Usage](#usage)
  - [Bugfixes](#bugfixes)
  - [Feature Requests](#feature-requests)
  - [Updates](#updates)
- [License](#license)
- [References](#references)

## Prerequisites and Assumptions

## Installation
### Build from Source
Clone this repository and build the image with `docker build -t (imagename) .`

### Prebuilt Images
Builds of the image are available on [Docker Hub](https://hub.docker.com/r/tiredofit/grafana) and is the recommended method of installation.

```bash
docker pull tiredofit/grafana:(imagetag)
```
The following image tags are available along with their tagged release based on what's written in the [Changelog](CHANGELOG.md):

| Container OS | Tag       |
| ------------ | --------- |
| Alpine       | `:latest` |

#### Multi Archictecture
Images are built primarily for `amd64` architecture, and may also include builds for `arm/v6`, `arm/v7`, `arm64` and others. These variants are all unsupported. Consider [sponsoring](https://github.com/sponsors/tiredofit) my work so that I can work with various hardware. To see if this image supports multiple architecures, type `docker manifest (image):(tag)`

## Configuration

### Quick Start

* The quickest way to get started is using [docker-compose](https://docs.docker.com/compose/). See the examples folder for a working [docker-compose.yml](examples/docker-compose.yml) that can be modified for development or production use.

* Set various [environment variables](#environment-variables) to understand the capabilities of this image.

### Persistent Storage
| File                   | Description                                                              |
| ---------------------- | ------------------------------------------------------------------------ |
| `/var/run/docker.sock` | You must have access to the docker socket in order to utilize this image |

* * *
### Environment Variables

#### Base Images used

This image relies on an [Alpine Linux](https://hub.docker.com/r/tiredofit/alpine) base image that relies on an [init system](https://github.com/just-containers/s6-overlay) for added capabilities. Outgoing SMTP capabilities are handlded via `msmtp`. Individual container performance monitoring is performed by [zabbix-agent](https://zabbix.org). Additional tools include: `bash`,`curl`,`less`,`logrotate`, `nano`,`vim`.

Be sure to view the following repositories to understand all the customizable options:

| Image                                                  | Description                            |
| ------------------------------------------------------ | -------------------------------------- |
| [OS Base](https://github.com/tiredofit/docker-alpine/) | Customized Image based on Alpine Linux |


| Parameter           | Description                                                                             | Default                      |
| ------------------- | --------------------------------------------------------------------------------------- | ---------------------------- |
| `TRAEFIK_VERSION`   | What version of Traefik do you want to work against - `1` or `2`                        | `2`                          |
| `DOCKER_ENTRYPOINT` | Docker Entrypoint default (local mode)                                                  | `unix://var/run/docker.sock` |
| `DOCKER_HOST`       | (optional) If using tcp connection e.g. `tcp://111.222.111.32:2376`                     |                              |
| `DOCKER_CERT_PATH`  | (optional) If using tcp connection with TLS - Certificate location e.g. `/docker-certs` |                              |
| `DOCKER_TLS_VERIFY` | (optional) If using tcp conneciton to socket Verify TLS                                 | `1`                          |
| `REFRESH_ENTRIES`   | If record exists, update entry with new values `TRUE` or `FALSE`                        | `TRUE`                       |
| `SWARM_MODE`        | Enable Docker Swarm Mode `TRUE` or `FALSE`                                              | `FALSE`                      |
| `CF_EMAIL`          | Email address tied to Cloudflare Account - Leave Blank  for Scoped API                  |                              |
| `CF_TOKEN`          | API Token for the Domain                                                                |                              |
| `DEFAULT_TTL`       | TTL to apply to records                                                                 | `1`                          |
| `TARGET_DOMAIN`     | Destination Host to forward records to e.g. `host.example.com`                          |                              |
| `DOMAIN1`           | Domain 1 you wish to update records for.                                                |                              |
| `DOMAIN1_ZONE_ID`   | Domain 1 Zone ID from Cloudflare                                                        |                              |
| `DOMAIN1_PROXIED`   | Domain 1 True or False if proxied                                                       |                              |
| `DOMAIN1_TARGET_DOMAIN` | (optional specify target_domain for Domain 1, overriding the default value from TARGET_DOMAIN)                                          |                              |
| `DOMAIN1_EXCLUDED_SUB_DOMAINS` | (optional specify sub domain trees to be ignored in lables) ex: `DOMAIN1_EXCLUDED_SUB_DOMAINS=int` would not create a CNAME for `*.int.example.com` | |
| `DOMAIN2`           | (optional Domain 2 you wish to update records for.)                                     |                              |
| `DOMAIN2_ZONE_ID`   | Domain 2 Zone ID from Cloudflare                                                        |                              |
| `DOMAIN2_PROXIED`   | Domain 1 True or False if proxied                                                       |                              |
| `DOMAIN2_TARGET_DOMAIN`   | (optional specify target_domain for Domain 2, overriding the default value from TARGET_DOMAIN)                                     |                              |
| `DOMAIN2_EXCLUDED_SUB_DOMAINS` | (optional specify sub domain trees to be ignored in lables) ex: `DOMAIN2_EXCLUDED_SUB_DOMAINS=int` would not create a CNAME for `*.int.example2.com` | |
| `DOMAIN3....`       | And so on..                                                                             |                              |

## Maintenance
### Shell Access

For debugging and maintenance purposes you may want access the containers shell.

```bash
docker exec -it (whatever your container name is e.g. grafana) bash
```

## Support

These images were built to serve a specific need in a production environment and gradually have had more functionality added based on requests from the community.
### Usage
- The [Discussions board](../../discussions) is a great place for working with the community on tips and tricks of using this image.
- Consider [sponsoring me](https://github.com/sponsors/tiredofit) personalized support.
### Bugfixes
- Please, submit a [Bug Report](issues/new) if something isn't working as expected. I'll do my best to issue a fix in short order.

### Feature Requests
- Feel free to submit a feature request, however there is no guarantee that it will be added, or at what timeline.
- Consider [sponsoring me](https://github.com/sponsors/tiredofit) regarding development of features.

### Updates
- Best effort to track upstream changes, More priority if I am actively using the image in a production environment.
- Consider [sponsoring me](https://github.com/sponsors/tiredofit) for up to date releases.

## License
MIT. See [LICENSE](LICENSE) for more details.
## References

