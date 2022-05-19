---
title: Build and Deploy APIM Images in OpenShift
linkTitle: BuildImages
weight: 1
date: 2022-05-19
description: How to build and deploy API Gateway images in Openshift
---

## Intentions

Deploying the containerized artifacts of APIM on OpenShift has been challenging and has required manual workarounds from our pre sales team to make
it possible for our customers. This requirement is now strategic, so OpenShift support needs to be offered by default.

This document describes how we can build OpenShift compatible Docker images and use the native OpenShift tools to publish and deploy images into an
OpenShift cluster. We'll also describe how to set the pre-requisites (Cassandra, MySQL) as Kubernetes resources for a non production grade environment.

This document doesn't cover the packaging of the solution as a Helm chart, we'll get to this in another document.

## Pre-requisites

```
A copy of the EMT script repository,
Create a session on the OpenShift cluster
```
```
oc login
```
```
Authenticate to the OpenShift cluster's registry
```
```
docker login -u developer -p $(oc whoami -t) REGISTRY_URL
```
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
```
Create a project for the Cassandra cluster
```
```
oc new-project cassandra
```
```
Install Cassandra (also downgraded to tag 3.11, latest patch release for Cassandra v3.11)
```
```
helm install cassandra bitnami/cassandra --set podSecurityContext.enabled="false" --set
containerSecurityContext.enabled="false" --set dbUser.user=cassandra --set dbUser.password=cassandra --
set replicaCount=3 --set jvm.maxHeapSize=1024M --set jvm.newHeapSize=1024M --set resources.limits.
memory=4Gi --set resources.requests.memory=4Gi --set image.tag="3.11" -n cassandra
```
## Deployment steps




### Build the images

We'll use the quickstart.sh script as an easy method to locally build the API Gateway images, but any other methods based on this branch of the EMT
script repo is fine.

```
In the quickstart.sh script change the METRICS_DB_URL to point to the db service you created in the above step and update the password as
well to the proper one.
in the quickstart.sh update mysql jar version as following mysql_jar_version=5.1.47 (because we'll be using a 5.7 MySQL DB)
download the license file and apigw.fed (it is the default fed file, only change is updated Cassandra password to the one we had mentioned in the
above install Cassandra commands) file from the attachments in
AMC-1964 - Generate APIGW docker image for use with OpenShift CLOSED.
In the build_gateway_image() function of the quickstart.sh script, point to the fed location as for example shown below.
./build_gw_image.py --license="$LICENSE" --merge-dir="$gw_merge_dir" --default-cert --fed=~/apigw-emt-scripts/quickstart/apigw.
fed
change CASS_HOST value to CASS_HOST=cassandra.cassandra
download the gateway binary from https://support.axway.com/en/downloads/download-details/id/
./quickstart/quickstart.sh APIGateway_7.7.20210830_Install_linux-x86-64_BN02.run license.lic
After 30 minutes we should see the script execution is completed
```
### Create a project for APIM

```
oc new-project apigw
```
### Publish the images in OCR

Instead of pushing in an external Docker registry, we're leveraging the OpenShift integrated container image registry, which is internal to the cluster without
need for the nodes to authenticate to pull images.

There's multiple ways to push images in the integrated registry, we'll use in this document the external route, more information on how to do that is
available in the official documentation.

Get the registry hostname from its route, we'll refer to is as REGISTRY_URL:

```
oc get routes -n openshift-image-registry default-route
```
#### ANM image

```
Tag the local image
```
```
docker tag admin-node-manager REGISTRY_URL/apigw/anm
```
```
Publish the image in the image registry
```
```
docker push REGISTRY_URL/apigw/anm
```
```
Note that when an image is pushed to OCR, an ImageStream is created, as well as an ImageStreamMapping. The mapping is reflected in the
namespace's Imagestream. This imagestream could be used in the oc new-app commands below, instead of the full image names.
```

```
2.
```
```
oc get imagestream
```
##### NAME IMAGE

##### REPOSITORY TAGS

##### UPDATED

```
imagestream.image.openshift.io/analytics default-route-openshift-image-registry.apps.openshifsl.pyxh.
p1.openshiftapps.com/apigw-helm/analytics latest 12 days ago
imagestream.image.openshift.io/anm default-route-openshift-image-registry.apps.openshifsl.pyxh.
p1.openshiftapps.com/apigw-helm/anm latest 4 weeks ago
imagestream.image.openshift.io/apimgr default-route-openshift-image-registry.apps.openshifsl.pyxh.
p1.openshiftapps.com/apigw-helm/apimgr latest 4 weeks ago
imagestream.image.openshift.io/apiportal default-route-openshift-image-registry.apps.openshifsl.pyxh.
p1.openshiftapps.com/apigw-helm/apiportal latest 2 weeks ago
```
```
More on ImageStreams is discussed in 8. Beyond Deploying an Application on OpenShift
```
#### Gateway Manager image

```
Tag the local image with the OCR hostname
```
```
docker tag api-gateway-defaultgroup REGISTRY_URL/apigw/apimgr
```
```
Publish the image in the OCR
```
```
docker push REGISTRY_URL/apigw/apimgr
```
### Deploy in OpenShift

Create new applications using the image stream, and internally expose the ANM pods as a service.

```
oc new-app REGISTRY_URL/apigw/anm \
-e METRICS_DB_URL="jdbc:mysql://mysql.metrics:3306/metrics?useSSL=false" \
-e METRICS_DB_USERNAME=root \
-e METRICS_DB_PASS=password \
--namespace apigw
```
```
oc expose deploy anm --port=8090 -n apigw
```
```
oc new-app REGISTRY_URL/apigw/apigmr \
-e EMT_DEPLOYMENT_ENABLED=true \
-e EMT_ANM_HOSTS=anm:8090 \
-e CASS_HOST=cassandra.cassandra \
-e METRICS_DB_URL="jdbc:mysql://mysql.metrics:3306/metrics?useSSL=false" \
-e METRICS_DB_USERNAME=root \
-e METRICS_DB_PASS=password \
--namespace apigw
```
Port forward the gateway manager pod to expose the UI on your laptop

```
kubectl port-forward POD_NAME 8075:8075 -n apigw
```
Once the port-forward is active, we can open [http://localhost:8075](http://localhost:8075) to login to the gateway manager

## What we've achieved

We've validated the changes made on the EMT scripts, and used the native OpenShift tools to deploy the app in the cluster.

Next step is to extend to the other components of APIM, and to use Helm for packaging and templating.


## Reference

```
AMC-1747 - Code review of EMT script modifications for Openshift CLOSED
```
## Next

Our customers won't deploy the application component per component, we need to package it so it becomes straightforward: 6. APIM Helm Deployment
on OpenShift
