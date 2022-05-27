---
title: Build APIM Images
linkTitle: BuildImages
weight: 1
date: 2022-05-19
description: How to build API Gateway images for deployment with helm 
---

Building docker images for use with helm - in Openshift or in kubernetes - is the same as described in [Deploy API Gateway in containers](/docs/apim_installation/apigw_containers/_index.md)


## Prerequisites

1. Follow the prerequisites to 'Set up your Docker environment' and 'Set up API Gateway Docker scripts' detailed in [Set Docker Environment](/docs/apim_installation/apigw_containers/dockers_scripts_prereqs.md)

## Build the images

1. Build a base image as detailed in [Create base Docker image](/docs/apim_installation/apigw_containers/docker_script_baseimage).


2. Build an Admin Node Manager image as detailed in [Create an Admin Node Manager Docker image](/docs/apim_installation/apigw_containers/docker_script_anmimage/#create-an-admin-node-manager-docker-image).


3. Build the Group1 API Gateway image as detailed in [Create an API Gateway Docker image](/docs/apim_installation/apigw_containers/docker_script_gwimage/#create-an-api-gateway-docker-image).
