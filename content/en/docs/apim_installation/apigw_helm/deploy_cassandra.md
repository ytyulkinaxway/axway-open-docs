---
title: Deploy a Cassandra Cluster
linkTitle: CassCluster
weight: 4
date: 2022-05-19
description: How to deploy a cassandra cluster
---

## For Production Create A Cassandra Cluster on Virtual Machines

For production deployments we recommend you follow this link to create a cassandra cluster: [Configure a Cassandra HA cluster](/docs/cass_admin/cassandra_config/).

## For Development Environments Create a Cassandra Cluster in Containers

For development environments it it possible to create a Cassandra cluster in containers using helm

If on OpenShift create a new project - eg ```oc new-project cassandra```

Using bitnami/cassandra install cassandra using this sample command:

```helm install cassandra bitnami/cassandra --set podSecurityContext.enabled="false" --set
containerSecurityContext.enabled="false" --set dbUser.user=cassandra --set dbUser.password=cassandra --
set replicaCount=3 --set jvm.maxHeapSize=1024M --set jvm.newHeapSize=1024M --set resources.limits.
memory=4Gi --set resources.requests.memory=4Gi --set image.tag="3.11" -n cassandra```


