---
title: Manage your environments and gateways
linkTitle: Manage your environments and gateways
weight: 15
date: 2020-11-18
description: Understand environments in a topology, what they are, and what can you do with them.
---

{{< alert title="Public beta" color="warning" >}}This feature is currently in **public beta** and not yet available for production use.{{< /alert >}}

Within topology, environments represent a group of assets discovered from a gateway, a repository, or anything manually added to the environment. These grouped assets (for example, services, webhooks, secrets) are displayed in AMPLIFY Central. Environments are at the highest hierarchical level, and all assets are scoped within.

The following is an example of a simple environment with an API service asset:

```txt
* Environment
    * Service
        * Versions
            * Endpoints
    * Webhooks
    * Secrets
```

The services, webhooks, and secrets in the example all have a hard dependency to the environment. _If the environment is deleted all assets within the environment will also be deleted._ The same hard dependency applies to all child assets. Another example, if the version within the service is deleted, the endpoint will also be deleted but not the service.

The relationship between service assets, webhooks, and secrets is a soft dependency. If a webhook is deleted, neither of the other two will be affected. However, this may break integrations where the webhook was being used, for example, in a catalog item.

You can combine assets within an environment to create catalog items that consumers can then subscribe to and use.

## Synchronize your environment with a gateway

Using agents is the recommended way to add service assets to your environment. When a discovery agent is installed on your gateway, the agent will auto-discover service assets and add them to your environment in AMPLIFY Central. The traceability agent will send API traffic logs from your gateway to AMPLIFY Central, where you can then view and analyze the logs.

For more information about the agents, see:

* [Discovery and Traceability Agents for API Manager](/docs/central/connect-api-manager/).
* [Discovery and Traceability Agents for AWS Gateway](/docs/central/connect-aws-gateway/).

To manually synchronize your environment, you can use the [AMPLIFY Central CLI](/docs/central/cli_central/cli_apiservices) or the [AMPLIFY Central APIs](https://apicentral.axway.com/apis/docs). Note that changes in your deployment will not be automatically synchronized with AMPLIFY Central.

## Asset definitions

This section describes the assets that are represented in your environment.

### Services

A service represents a physical deployment of a resource in an environment. Examples of services include API, MFT, Protobuf, documentation, and so on. You can manually add services to your environment or they can be discovered and auto-added by Axway agents. Later, these services can be combined and packaged together to create catalog items for your consumers to access.

#### Versions

Services have a specification based on type (OAS2, OAS3, WSDL, Protobuf, etc). Whenever this specification changes a new version must be created. This helps account for different stages in the lifecycle of the service. Each version has a direct dependency on its associated services and can be individually configured for differing consumer access needs.

#### Endpoints

An endpoint is a URL that represents the deployment of a service. There can be one or many endpoints to access a deployed service version. An endpoint includes a name and description to make it easier for others to consume later. They also contain the host and port information used to access the service and have a hard dependency on the service version it is associated with.

### Webhooks

Webhook assets are used for enabling subscription flows when a catalog user wants to subscribe to your service. Webhooks are scoped to the environment level, which makes them reusable with other assets within the environment.

### Secrets

Secrets are a special kind of asset used with webhooks for securing subscription flows.
