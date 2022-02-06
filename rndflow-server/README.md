# R&D Flow Server

[R&D Flow Server](https://www.server.rndflow.com/) is a Digital platform for visual collective Research&Development activity and analytic software development.

## Introduction

This chart bootstraps a  [R&D Flow Server](https://server.rndflow.com/) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Installation prerequisites

- Kubernetes 1.19+
- Helm 3.1.0
- PostgreSQL server must be installed.
- RabbitMQ server must be installed.
- MinIO storage server must be installed.
- Ingress Nginx controller is required.
- Valid internet hostname identity certificate (for example, Let’s Encrypt) for Ingress Nginx controller is recommended.
- PV provisioner support in the underlying infrastructure

Installation instuctions available [here](https://github.com/rndflow/rndflow-install/README.md).

## Installing the Chart

Full installation instuction available [here](https://github.com/rndflow/rndflow-install/blob/main/instances/server/README.md).

To install the chart with the release name `my-release`:

```console
$ helm repo add rndflow  https://rep.rndflow.com/helm/

$ helm install my-release rndflow/rndflow-server \
    --set config.registry=docker.io/ \
    --set config.databaseUri=postgresql+asyncpg://dbuser:******@dbhostname/rndflow \
    --set config.rabbitmqUri=amqp://rmquser:******@rmqhostname:5672/%2F \
    --set config.adminEmail=admin@gmail.com \
    --set api.secret=0000000-0000-0000-0000-00000000000 \
    --set frontend.ingress.enabled=true \
    --set frontend.ingress.hostname=hostname \
    --set email.username=user@gmail.com \
    --set email.password=****** \
    --set email.sender=user@gmail.com
```

The command deploys R&D Flow Server on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

## Parameters

### R&D Flow Server parameters

| Name                                          | Description                                                                                                                                               | Value                       |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
|`config.registry`                              | Images Registry                                                                                                                                           |``                           |
|`config.databaseUri`                           | Database URI                                                                                                                                              |``                           |
|`config.rabbitmqUri`                           | RabbitMQ URI                                                                                                                                              |``                           |
|`config.adminEmail`                            | Admins email                                                                                                                                              |``                           |
|`api.replicas`                                 | Number of API Serversreplicas to deploy                                                                                                                   |`1`                          |
|`api.image`                                    | API Server image                                                                                                                                          |`rndflow/api:latest`         |
|`api.secret`                                   | API Server secret                                                                                                                                         |``                           |
|`celery.replicas`                              | Number of Celery replicas to deploy                                                                                                                       |`1`                          |
|`pgRmqProxy.image`                             | PostgeSQL-RabbitMQ proxy image                                                                                                                            |`rndflow/pg-rmq-proxy:latest`| 
|`frontend.replicas`                            | Number of frontend replicas to deploy                                                                                                                     |`1`                          |
|`frontend.image`                               | Frontend client image                                                                                                                                     |`rndflow/client:latest`      | 
|`frontend.ingress.enabled`                     | Set to true to enable ingress record generation                                                                                                           |`false`                      |
|`frontend.ingress.hostname`                    | Default host for the ingress resource                                                                                                                     |``                           |
|`frontend.ingress.annotations`                 | Additional annotations for the Ingress resource. To enable certificate autogeneration, place here your cert-manager annotations.                          |`{}`                         |
|`frontend.ingress.tls`                         | Ingress TLS Secret                                                                                                                                        |``                           |
|`frontend.service.type`                        | Frontend Service type                                                                                                                                     |`ClusterIP`                  |
|`frontend.service.port`                        | Frontend Service HTTP port                                                                                                                                |`8080`                       |
|`frontend.service.clusterIP`                   | Frontend Service Cluster IP                                                                                                                               |``                           |
|`frontend.service.nodePort`                    | Specify the Frontend nodePort(s) value(s) for the LoadBalancer and NodePort service types.                                                                |``                           |
|`frontend.service.externalIPs`                 | Set the Frontend ExternalIPs                                                                                                                              |``                           |
|`email.server`                                 | SMTP Mail Server                                                                                                                                          |`smtp.gmail.com`             |
|`email.port`                                   | SMTP Mail Server port                                                                                                                                     |`587`                        |
|`email.tls`                                    | For TLS connection                                                                                                                                        |`true`                       |
|`email.ssl`                                    | For TLS connection                                                                                                                                        |`false`                      |
|`email.use_credentials`                        | It enables users to choose whether or not to login to their SMTP Server.                                                                                  |`true`                       |
|`email.username`                               | Username for email, some email hosts separates username from the default sender(AWS).                                                                     |``                           |
|`email.password`                               | Password for authentication                                                                                                                               |``                           |
|`email.sender`                                 | Sender address                                                                                                                                            |``                           |
|`hostAliases`                                  | Deployment pod host aliases                                                                                                                               |`[]`                         |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.
