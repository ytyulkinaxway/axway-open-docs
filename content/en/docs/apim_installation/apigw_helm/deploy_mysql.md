---
title: Deploy Mysql DB with helm
linkTitle: MysqlInContainers
weight: 3
date: 2022-05-19
description: How to deploy a MySQL DB using a public helm chart
---


```
Create a project for the MySQL instance
```

```
oc new-project metrics
```

```
Prepare a configmap with the SQL file for the schema init
```

```
echo 'CREATE DATABASE metrics; USE metrics;' | cat - quickstart/mysql-analytics.sql > mysql-analytics.sql
kubectl create configmap mysql-metrics --from-file=mysql-analytics.sql -n metrics
rm mysql-analytics.sql
```

```
Install Mysql (we'll use tag 5.7 for the mysql image, for compatibility with gateway and portal)
```

```
helm install mysql bitnami/mysql \
-n metrics \
--set primary.podSecurityContext.enabled="false" \
--set primary.containerSecurityContext.enabled="false" \
--set secondary.podSecurityContext.enabled="false" \
--set secondary.containerSecurityContext.enabled="false" \
--set auth.rootPassword=password \
--set initdbScriptsConfigMap=mysql-metrics \
--set image.tag=5.
```
