# FLEXlm Exporter [![Build Status](https://travis-ci.org/mjtrangoni/flexlm_exporter.svg)][travis]

[![CircleCI](https://circleci.com/gh/mjtrangoni/flexlm_exporter.svg?style=svg)](https://circleci.com/gh/mjtrangoni/flexlm_exporter)
[![Docker Repository on Quay](https://quay.io/repository/mjtrangoni/flexlm_exporter/status)][quay]
[![Docker Pulls](https://img.shields.io/docker/pulls/mjtrangoni/flexlm_exporter.svg?maxAge=604800)][hub]
[![GoDoc](https://godoc.org/github.com/mjtrangoni/flexlm_exporter?status.svg)](https://godoc.org/github.com/mjtrangoni/flexlm_exporter)
[![Coverage Status](https://coveralls.io/repos/github/mjtrangoni/flexlm_exporter/badge.svg?branch=master)](https://coveralls.io/github/mjtrangoni/flexlm_exporter?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/mjtrangoni/flexlm_exporter)](https://goreportcard.com/report/github.com/mjtrangoni/flexlm_exporter)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/00e03e600d5744d1a2cc21d98e2f8273)](https://www.codacy.com/app/mjtrangoni/flexlm_exporter?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=mjtrangoni/flexlm_exporter&amp;utm_campaign=Badge_Grade)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://raw.githubusercontent.com/mjtrangoni/flexlm_exporter/master/LICENSE)
[![StyleCI](https://github.styleci.io/repos/107779392/shield?branch=master)](https://github.styleci.io/repos/107779392)

[Prometheus](https://prometheus.io/) exporter for FLEXlm License Manager
`lmstat` license information.

This fork is specialised for the reduced version of FLEXlm License Manager bundled with installations of Klocwork. This reduced bundle contains lmstat as a independent tool, instead of the lmutil lmstat tool used by the parent of this fork.

NOTE: The FLexLM Exporter currently builds only on Linux. Windows builds are a WIP. You will need to modify the Makefile to build in Windows.

## Getting

```
$ go get github.com/emenda/flexlm_exporter
```

## Building

```
$ cd $GOPATH/src/github.com/emenda/flexlm_exporter
$ make
```

## Configuration

This is an illustrative example of the configuration file in YAML format.

```
# FlexLM Licenses to be monitored.
---
licenses:
  - name: app1
    license_file: /usr/local/flexlm/licenses/license.dat.app1
    features_to_exclude: feature1,feature2
    monitor_users: True
    monitor_reservations: True
  - name: app2
    license_server: 28000@host1,28000@host2,28000@host3
    features_to_include: feature5,feature30
    monitor_users: True
    monitor_reservations: True
```

Notes:

 1. It is possible to define a license with a path in `license_file`, that has to
 be readable from the exporter instance, **or** with `license_server` in a
 `port@host` combination format.
 2. You can exclude some features from exporting with `features_to_exclude`,
 **or** export some defined and exclude the rest with `feature_to_include`.

## Running

```
$ ./flexlm_exporter --path.lmutil="/klocwork/3rdparty/bin/lmstat" <flags>
```

### Docker images

Docker images are available on,

 1. [Quay.io](https://quay.io/repository/mjtrangoni/flexlm_exporter).
    `$ docker pull quay.io/mjtrangoni/flexlm_exporter`
 1. [Docker](https://hub.docker.com/r/mjtrangoni/flexlm_exporter/).
    `$ docker pull mjtrangoni/flexlm_exporter`

You can launch a *flexlm_exporter* container with,

```
$ docker run --name flexlm_exporter -d -p 9319:9319 --volume $LMUTIL_LOCAL:/usr/bin/flexlm/ --volume $CONFIG_PATH_LOCAL:/config $DOCKER_REPOSITORY --path.lmutil="/usr/bin/flexlm/lmstat" --path.config="/config/licenses.yml"
```

Metrics will now be reachable at http://localhost:9319/metrics.

## What's exported?

 * `lmstat -v` information.
 * `lmstat -c license_file -a` or `lmstat -c license_server -a`
   license information.
 * `lmstat -c license_file -i` or `lmstat -c license_server -i`
   license features expiration date.

## Dashboards

 1. [Grafana Dashboard](https://grafana.com/dashboards/3854)

## Contributing

Refer to [CONTRIBUTING.md](https://github.com/mjtrangoni/flexlm_exporter/blob/master/CONTRIBUTING.md)

## License

Apache License 2.0, see [LICENSE](https://github.com/mjtrangoni/mjtrangoni/blob/master/LICENSE).

[travis]: https://travis-ci.org/mjtrangoni/flexlm_exporter
[hub]: https://hub.docker.com/r/mjtrangoni/flexlm_exporter/
[quay]: https://quay.io/repository/mjtrangoni/flexlm_exporter
