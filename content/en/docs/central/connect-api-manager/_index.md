---
title: Connect API Manager
linkTitle: Connect API Manager
no_list: true
draft: false
weight: 130
description: >
  Understand why you would connect Axway API Manager to AMPLIFY Central. Learn
  how you can publish to the AMPLIFY Catalog from your API Management System in
  order to obtain a global view of your APIs and present this Catalog to your
  consumers.

  Learn how you can collect the traffic of all your gateways and see it in a single place in AMPLIFY Central Observability.
---
## What is Axway API Manager connected?

Connect your API Management system (v7.6.2 or above) to AMPLIFY Central by using two agents: Discovery and Traceability. These two agents will help you to represent and expose your API Management eco-system in AMPLIFY Central:

* Create an API Gateway environment in AMPLIFY Central that represent your actual API Management eco-system.
* Detect published API using the Discovery Agent. The Discovery Agent discovers the API from API Manager and makes them available in AMPLIFY Central. An API Service in Central is created to reference the API from API Manager and then you can optionally tell the agent to publish it to the AMPLIFY Catalog to allow your consumer to discover it.
* Manage consumer subscription using the Discovery Agent. When a consumer subscribes/unsubscribes to a Catalog asset, the Discovery Agent keeps track of the changes and maintains the API Management system accordingly.  
* Filter the Axway API Gateway logs using the Traceability Agent. The Traceability Agent uses the discovered API to filter API Gateway events to extract the transaction information and send it to the AMPLIFY platform Observability module.

### Discovery Agent

The Discovery Agent is used to discover new published APIs. The Discovery Agent pushes both REST and SOAP API definitions to AMPLIFY Central.

The Discovery Agent discovers APIs that have PassTrough / API Key / OAuth security only.

The related APIs are published to AMPLIFY Central in either disconnected mode (catalog item publication) or connected mode (Environment / API Service publication and optionally as Catalog item). For additional information, see [Discovery Agent](/docs/central/connect-api-manager/deploy-your-agents/#discovery-agent).

![Service Discovery](/Images/central/connect-api-manager/servicediscoveryapim.png)

### Traceability Agent

The Traceability Agent sends log information about APIs that have been discovered and published to AMPLIFY Central.

## Pre-requisites

* An Axway AMPLIFY Central subscription in the AMPLIFY™ platform. See [Get started with AMPLIFY Central](/docs/central/quickstart/).
* An AMPLIFY Central Service Account. See [Create a service account](/docs/central/connect-api-manager/#create-a-new-service-account).
* An AMPLIFY Central environment.
* An API Manager user having role API Manager Administrator for the discovery/traceability agent to connect to API Manager
* An API Gateway user having role API Gateway operator for traceability agent to connect to API Gateway

## System requirements

* Axway API Manager / Axway API Gateway versions 7.6.2 SPx and 7.7 SPx up and running using a Linux installation (CentOS 6.x, 7.x, 8.x,  Oracle Linux 6.x, 7.x, Red Hat Enterprise Linux 6.x, 7.x, 8.x, SUSE Linux Enterprise Server 11.x, 12.x).
* The Linux machine where API Manager and API Gateway are running must be accessible and have `sudo` rights to run the Agents.
* The agents must be installed on the same machine that API Manager and/or API Gateway is running.

## Region support

AMPLIFY Central supports two regions, US (default) and EU. The data  (APIs, traffic) that the agents send to AMPLIFY Central is stored in one of those regions based on the agent configuration.

Use one of the following URLs to access the AMPLIFY Central UI:

* US: [https://apicentral.axway.com](https://apicentral.axway.com)
* EU: [https://central.eu-fr.axway.com](https://central.eu-fr.axway.com)

Update the following variables to move data to the EU region:

* `CENTRAL_DEPLOYMENT`= **prod-eu**
* `CENTRAL_URL`= **htpps://central.eu-fr.axway.com**
* `TRACEABILITY_HOST`= **ingestion-lumberjack.visibility.eu-fr.axway.com:453**

## Connect Axway API Manager to AMPLIFY Central quickstart

The following gives you a high-level overview of the required steps to connect a API-Manager V7 environment to AMPLIFY-Central.

### Create a new Service Account

* The agent will use this service account for the secure communication with the AMPLIFY platform
* Generate a public/private key pair
    ```
    openssl genpkey -algorithm RSA -out ./private_key.pem -pkeyopt rsa_keygen_bits:2048
    openssl rsa -pubout -in ./private_key.pem -out ./public_key.pem
    ```
* Create a new Service Account user in AMPLIFY Central using the `public_key.pem` from above. You may name this Service Account (for example, v7-Agent). For additional information, see [Create a service account](/docs/central/cli_central/cli_install/#create-a-service-account).

### Add your environment to Central

* Create an environment object in AMPLIFY Central that represents the effective Axway API Gateway environment. Depending on your needs, you can create as many environments as required.
* Each discovered API or Traffic is associated to this environment and eases the filtering.
* Add your environment to AMPLIFY Central using either the [AMPLIFY Central CLI](/docs/central/cli_central/cli_environments/) or [the UI](/docs/central/connect-api-manager/prepare-amplify-central/#create-environment-using-the-UI).

### Install and prepare the Discovery Agent

* The agent will be installed in the actual runtime environment and is using the service account for the communication with the platform
* Learn more how install and setup the [Discovery Agent](/docs/central/connect-api-manager/deploy-your-agents/#discovery-agent)*

### Install and prepare the Traceability Agent

* Quite similar to the discovery agent, also the Traceability Agent needs to be installed and configured in your environment
* Learn more how install and setup the [Traceability Agent](/docs/central/connect-api-manager/deploy-your-agents/#traceability-agent)*

\* If you need assistance setting up the configuration files, please contact your Sales Account Manager, or open a support ticket on support.axway.com.

The following demonstrates how to connect an Axway API-Gateway V7 to AMPLIFY-Central.

{{< youtube kugRyYVw5nI >}}

## Role-based documentation

Some of the content in the Connect API Manager documentation is of interest to a wide range of development and administrator professions. The following topics are organized by professional role and common task scenarios:

* [Administer API Manager Gateway](/docs/central/connect-api-manager/gateway-administation/)

* [Administer API Manager agent security](/docs/central/connect-api-manager/agent-security-api-manager/)

* [Administer API Manager network traffic](/docs/central/connect-api-manager/network-traffic-apimanager/)
