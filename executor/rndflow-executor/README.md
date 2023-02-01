# R&D Flow Executor

[R&D Flow executor](https://www.server.rndflow.com/) is a Jobs Executor application of the R&D Flow Digital platform for visual collective Research&Development activity and analytic software development.

## Introduction

This chart bootstraps a  [R&D Flow executor](https://server.rndflow.com/) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.20+
- Helm 3.1.0

## Installing the Chart

Full installation instuction available [here](https://github.com/rndflow/rndflow-install/blob/main/instances/executors/main-executor/README.md).

1. Generate GPG private and public keys pair (executor.key/executor.pub).
 
2. Register Executor on R&D Flow Web console and load its public key.
 
    https://server.rndflow.com/docs/admin/#серверы-расчетов

3. Install the chart with the release name `my-executor-release`:

   ```console

    $ kubectl create secret generic my-executor-release-access-key --from-file=key="executor.key"

    $ helm repo add rndflow https://rndflow.github.io/rndflow-helms/ 

    $ helm install my-executor-release rndflow/rndflow-executor \
      --set config.registry="docker.io" \
      --set config.apiHost="http://my-rndflow-server-frontend-svc:8080" \
      --set config.keySecret="my-executor-release-access-key" \
      --set config.executorLabel="executor_label" \
      --set jupyter.enabled=true \
      --set jupyter.externalHost="http://myhost" \
      --set jupyter.ingress.hostname="myhost"
   ```

   The command deploys R&D Flow Executor on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-executor-release` deployment:

```console
$ helm delete my-executor-release
$ kubectl delete secret my-executor-release-access-key
```

## Parameters

### R&D Flow Executor parameters

| Name                                          | Description                                                                                                                                               | Value                       |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
|`config.registry`                              | Images Registry                                                                                                                                           |``                           |
|`config.replicas`                              | Number of Executor replicas to deploy                                                                                                                     |`1`                          |
|`config.image`                                 | Executor image                                                                                                                                            |`rndflow/executor:latest`    |
|`config.apiHost`                               | The host name of the R&DFlow Frontend (local or external)                                                                                                 |``                           |
|`config.executorLabel`                         | The Executors label as registered in the in R&D Flow Server                                                                                               |``                           |
|`config.keySecret`                             | The name of the kubernetes secret holding the private PGP key for the executor. The correponding public key has to be registered in the R&D Flow Server.  |``                           |
|`config.gpuVendor`                             | Supported GPU Vendors cards name (nvidia/amd)                                                                                                             |`nvidia`                     |
|`config.nodeSelector`                          | Node labels for pod assignment.                                                                                                                           |``                           |
|`config.imagePullPolicy`                       | Image pull policy                                                                                                                                         |`Always`                     |
|`config.storage.create_pod_pvc`                | Volume storage type: `emptyDir` if `false` or separate  PVC/PV if `true`                                                                               |`false`                      |
|`config.storage.use_subpath`                   | Enable/Disable using Volume [subpath mounting](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath-expanded-environment) for each pod.                                                                                                   |`false`                        |
|`config.storage.access_modes`                  | Kubernetes Volume [access mode](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes).                                         |`ReadWriteOnce`                |
|`config.storage.pod_pvc_storage_class`         | Kubernetes Volume [storage class name](https://kubernetes.io/docs/concepts/storage/storage-classes/).                                                   |``                           |
|`config.storage.pod_pvc_request_storage_size`  | Kubernetes Volume requested [storage size](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims).               |`1Gi`                        |
|`jupyter.enabled`                              | Set to true to enable Jupyter interactive task                                                                                                            |`false`                      |
|`jupyter.ingress.hostname`                     | Default host for the ingress resource                                                                                                                     |``                           |
|`jupyter.ingress.externalHost`                 | Jupyter proxy endpoint host                                                                                                                               |`http(s)://ingress.hostname` |
|`jupyter.ingress.idleLimit`                    | Default idle time limit (hours) for Jupyter jobs                                                                                                          |`24`                         |
|`jupyter.ingress.annotations`                  | Additional annotations for the Ingress resource. To enable certificate autogeneration, place here your cert-manager annotations.                          |`{}`                         |
|`jupyter.ingress.tls`                          | Ingress TLS section                                                                                                                                       |``                           |
|`jupyter.ingress.path`                         | Path for the Jupyter proxy                                                                                                                                |`/jupyter`                   |
|`jupyter.service.type`                         | Jupyter Proxy Service type                                                                                                                                |`ClusterIP`                  |
|`jupyter.service.port`                         | Jupyter Proxy Service HTTP port                                                                                                                           |`8080`                       |
|`jupyter.service.clusterIP`                    | Jupyter Proxy Service Cluster IP                                                                                                                          |``                           |
|`jupyter.service.nodePort`                     | Specify the Jupyter Proxy nodePort(s) value(s) for the LoadBalancer and NodePort service types.                                                           |``                           |
|`jupyter.service.externalIPs`                  | Set the the Jupyter Proxy External IPs                                                                                                                    |``                           |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.
