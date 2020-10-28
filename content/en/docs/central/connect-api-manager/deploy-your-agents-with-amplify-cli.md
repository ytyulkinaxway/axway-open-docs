---
title: Deploy your agents with AMPLIFY CLI
linkTitle: Deploy your agents with AMPLIFY CLI
draft: false
weight: 37
description: Learn how to deploy your agents using AMPLIFY CLI so that you can
  manage your Axway API Gateway environment within AMPLIFY Central.
---
## Before you start

* Read [AMPLIFY Central and Axway API Manager connected overview](/docs/central/connect-api-manager/)
* You will need a basic knowledge of Axway API Management installation

    * where the API Gateway is running (host / port / path to event logs)
    * where the Admin node manager is running (host / port)
    * what users are available for the agent to use. It is recommended to have one API Manager user for Discovery Agent and one API Gateway operator user for Traceability Agent.

## Objectives

Learn how to quickly install and run your Discovery and Traceability agents with basic configuration using AMPLIFY Central CLI.

## AMPLIFY Central CLI pre-requisites

* Node.js 8 LTS or later
* Access to npm package (for installing AMPLIFY cli)
* Access to login.axway.com on port 443
* Minimum AMPLIFY Central CLI version: 0.1.14 (check version using `amplify central --version`)

More information is available at [Install AMPLIFY Central CLI](/docs/central/cli_central/cli_install/).

## Installing the agents

### Step 1: Identify yourself to AMPLIFY platform with AMPLIFY CLI

To use Central CLI to log in with your AMPLIFY Platform credentials, run the following command and use `apicentral` as the client identifier:

```shell
amplify auth login --client-id apicentral
```

A browser will automatically open.
Enter your valid credentials (email address and password). Once the “Authorization Successful” message is displayed, you can go back to the AMPLIFY CLI.

If you are a member of multiple AMPLIFY organizations, you may have to choose an organization.

{{< alert title="Note" color="primary" >}}If you do not have a graphical environment, you will have to forward the display to an X11 server (Xming or similar tools) using the `export DISPLAY=myLaptop:0.0` command .{{< /alert >}}

### Step 2: Running the agents' install procedure

Agents will be installed in the directory from where the CLI runs. You can install the agent from anywhere, but then you must transfer the agent and its configuration to the API Management system machine for the agent to operate correctly.

```shell
amplify central install agents
```

The installation procedure will prompt for the following:

1. Select the type of gateway you want to connect to, V7 gateway in this scenario
2. Select the agents you want to install: Discovery / Traceability / all
3. Select the agent deployment mode: binary / Docker image
4. Platform connectivity:
   * **environment**: can be an existing one or a new one that will be created by the installation procedure
   * **team**: can be an existing one or a new one that will be created by the installation procedure
   * **service account**: can be an existing one or a new one that will be created by the installation procedure. If you choose an existing one, be sure you have the appropriate public and private keys, as they will be required for the agent to connect to the AMPLIFY Platform. If you choose to create a new one, the generated private/public keys will be provided.
5. API Manager connectivity:
   * **hostname** of the API Manager (localhost by default)
   * **port** of the API Manager (8075 by default)
   * user/password
6. API Gateway connectivity:
   * **hostname** of the API Gateway (localhost by default)
   * **port** of the API Gateway (8090 by default)
   * user/password
   * event log path

Once you have answered all questions, the agents are downloaded, the configuration files are updated, the Amplify Central resources are created and the key pair are generated (if you chose to create a new service account).
The current directory should contain the following files:

```shell
discovery_agent
discovery_agent.yml
traceability_agent
traceability_agent.yml
da_env_vars.env
ta_env_vars.env
private_key.pem
public_key.pem
```

`discovery_agent` / `discovery_agent.yml` / `traceability_agent` / `traceability_agent.yml` files will be present only if you choose the binary mode installation.

`da_env_vars.env` / `ta_env_vars.env` contains the specific configuration you entered during the installation procedure.

`discovery_agent.yml` and `traceability_agent.yml` contain the default minimum agent configuration.

`private_key.pem` and `public_key.pem` are the generated key pair the agent will use to securely talk with the AMPLIFY Platform (if you choose to let the installation generate them).

## Starting the agents

### Binary mode

As mentioned in the installation procedure, agents can be started with the following commands:

Discovery Agent:

```shell
./discovery_agent --envFile ./da_env_vars.env
```

Traceability Agent:

```shell
./discovery_agent --envFile ./ta_env_vars.env
```

### Docker mode

As mentioned in the installation procedure, agents can be started with the following commands:

Discovery Agent:

```shell
docker run -it --env-file $(pwd)/da_env_vars.env -v $(pwd):/keys axway-docker-public-registry.bintray.io/agent/v7-discovery-agent:latest
```

Traceability Agent:

```shell
docker run -it --env-file $(pwd)/ta_env_vars.env -v $(pwd):/keys -v EVENT_LOG_PATH_ENTERED_DURING_INSTALLATION:/events axway-docker-public-registry.bintray.io/agent/v7-traceability-agent:latest
```

See [Administer API Gateway](/docs/central/connect-api-manager/gateway-administation/index.html) for additional information about agent features.
