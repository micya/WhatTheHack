## Introduction

[Open Service Mesh (OSM)](https://openservicemesh.io/) is a lightweight and extensible cloud native service mesh that conforms to the [Service Mesh Interface (SMI)](https://smi-spec.io/). It was developed by Microsoft and donated to the Cloud Native Computing Foundation.

The below follows the official [demo](https://docs.openservicemesh.io/docs/install/manual_demo/).

## Install service mesh CLI

Installation instructions given in the [OSM documentation](https://docs.openservicemesh.io/docs/install/).

```bash
system=$(uname -s)
release=v0.8.3
curl -L https://github.com/openservicemesh/osm/releases/download/${release}/osm-${release}-${system}-amd64.tar.gz | tar -vxzf - ./${system}-amd64/osm version
```

## Install service mesh

Make sure kubectl is pointing at the right context.

```bash
osm install \
    --deploy-jaeger \
    --deploy-grafana \
    --deploy-prometheus
```

The ``--deploy-jaeger`` argument enables distributed tracing with [Jaeger](https://www.jaegertracing.io/), and the ``--deploy-grafana`` and ``--deploy-prometheus`` arguments enable metrics and visualization.

## Install demo application

Install the [bookinfo](https://istio.io/latest/docs/examples/bookinfo/) application.



## Enable and verify mTLS

Mutual TLS is enabled by default.

To test mTLS, create 

## View high-level metrics

Enable metrics

```bash
osm metrics enable
```

View dashboard

```bash
osm dashboard
```

This will port-forward the Grafana dashboard to port 3000. The default username/password is admin/admin.

## Test distributed tracing & fault injection

Enable distributed tracing

```bash
osm mesh upgrade --enable-tracing
```

Port-forward Jaeger

```bash
OSM_POD=$(kubectl get pods -n osm-system --no-headers  --selector app=jaeger | awk 'NR==1{print $1}')
kubectl port-forward -n osm-system "$OSM_POD"  16686:16686
```

Open <http://localhost:16686> in the browser.
