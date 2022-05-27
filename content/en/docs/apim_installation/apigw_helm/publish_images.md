---
title: Publish Images
linkTitle: PublicImages
weight: 2
date: 2022-05-19
description: How to publish images for use with helm charts
---

Once docker images have been built for the API Gateway [Build APIM Images](/docs/apim_installation/apigw_containers/build_images.md) they must be tagged and pushed to a repository which is accessible by the environment from which the helm charts will be deployed. 

## Example of publishing the images to an Openshift Container Registry

```
oc login
```

```
Authenticate to the OpenShift cluster's registry
```

```
docker login -u developer -p $(oc whoami -t) REGISTRY_URL
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
