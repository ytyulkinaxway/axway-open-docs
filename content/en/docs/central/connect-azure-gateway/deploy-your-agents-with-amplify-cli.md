---
title: Deploy your agents with AMPLIFY CLI
linkTitle: Deploy your agents with AMPLIFY CLI
draft: false
weight: 10
description: Learn how to deploy your agents using AMPLIFY CLI so that you can
  manage your Azure Gateway environment within AMPLIFY Central.
---
## Before you start

* Read [AMPLIFY Central Azure Gateway connected overview](/docs/central/connect-azure-gateway/)
* You will need information on Azure:

    * where the API Service management is located (resource group name / API Management service name)
    * service principal for using to Azure APIs
    * event hub definition (namespace / name / policy).

* It is recommended that you have access to the Azure CLI command line to run the necessary setup commands

## Objectives

Learn how to quickly install and run your Discovery and Traceability agents with basic configuration using AMPLIFY Central CLI.

## AMPLIFY Central CLI prerequisites

* Node.js 8 LTS or later
* Access to npm package (for installing AMPLIFY cli)
* Access to login.axway.com on port 443
* Minimum AMPLIFY Central CLI version: 0.7.0 (check version using `amplify central --version`)

For more information, see [Install AMPLIFY Central CLI](/docs/central/cli_central/cli_install/).

## Installing the agents

### Step 1: Folder preparation

Create an empty directory where AMPLIFY CLI will generate files. Run all AMPLIFY Central CLI from this directory.

### Step 2: Identify yourself to AMPLIFY Platform with AMPLIFY CLI

To use Central CLI to log in with your AMPLIFY Platform credentials, run the following command:

```shell
amplify auth login
```

A browser automatically opens.
Enter your valid credentials (email address and password). Once the “Authorization Successful” message is displayed, go back to AMPLIFY CLI. The browser may be closed at this point.

If you are a member of multiple AMPLIFY organizations, you may have to choose an organization.

{{< alert title="Note" color="primary" >}}If you do not have a graphical environment, forward the display to an X11 server (Xming or similar tools) using the `export DISPLAY=myLaptop:0.0` command.{{< /alert >}}

### Step 3: Run the agents' install procedure

Azure agents are delivered in a Docker image provided by Axway. You can run them from any Docker container that can access the AMPLIFY Platform and Azure Gateway.
The AMPLIFY Central CLI will guide you through the configuration of the agents.

Agents configuration will be installed in the directory from where the CLI runs.

```shell
amplify central install agents
```

If your AMPLIFY subscription is hosted in the EU region, then the following installation command must be used to correctly configure the agents:

```shell
amplify central install agents --region=EU
```

The installation procedure will prompt for the following:

1. Select the type of gateway you want to connect to (Azure gateway in this scenario).
2. Platform connectivity:
   * **environment**: can be an existing environment or a new one that will be created by the installation procedure
   * **team**: can be an existing team or a new one that will be created by the installation procedure
   * **service account**: can be an existing service account or a new one that will be created by the installation procedure. If you choose an existing one, be sure you have the appropriate public and private keys, as they will be required for the agent to connect to the AMPLIFY Platform. If you choose to create a new one, the generated private and public keys will be provided.
3. Select the agents you want to install: Discovery / Traceability / all.
4. Azure connectivity
