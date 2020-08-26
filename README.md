# Elastic-stack Helm Chart

This chart installs an elasticsearch cluster with kibana and logstash by default.
You can optionally disable logstash and install Fluentd if you prefer. It also optionally installs filebeat, nginx-ldapauth-proxy and elasticsearch-curator.

## Prerequisites Details

* Kubernetes 1.10+
* PV dynamic provisioning support on the underlying infrastructure

## Chart Details
This chart will do the following:

* Implemented a dynamically scalable elasticsearch cluster using Kubernetes StatefulSets/Deployments
* Multi-role deployment: master, client (coordinating) and data nodes
* Statefulset Supports scaling down without degrading the cluster

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release stable/elastic-stack
```

## Deleting the Charts

Delete the Helm deployment as normal

```
$ helm delete my-release
```

Deletion of the StatefulSet doesn't cascade to deleting associated PVCs. To delete them:

```
$ kubectl delete pvc -l release=my-release,component=data
```

## Configuration

Each requirement is configured with the options provided by that Chart.
Please consult the relevant charts for their configuration options.

## create customize chart

logstashPipeline is do not support on stable OSS elastic-stack chart.

Add helm repository

```
helm add repo stable https://kubernetes-charts.storage.googleapis.com/
helm add repo elastic https://helm.elastic.co/
```

search for chart on repository

```
helm search repo elasic-stack
helm search repo logstash

helm search hub logstash
```

helm chart download

```
mkdir charts && cd charts
helm pull stable/elastic-stack
helm pull elastic/logstash
```

Replace to logstash 7.9.0 on elastic-stack 2.8.3 helm chart.

```
tar zxvf elastic-stack-2.0.3.tgz
tar zxvf logstash-7.9.0.tgz

rm -rf elastic-stack/charts/logstash
mv logstash elastic-stack/charts/.
```

install customize helm chart

```
kubectl create ns observability
helm install logging ./elastic-stack -n observability
```

