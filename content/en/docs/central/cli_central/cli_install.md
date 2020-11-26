---
title: Install AMPLIFY Central CLI
linkTitle: Install AMPLIFY Central CLI
weight: 90
date: 2020-05-29T00:00:00.000Z
description: Learn how to install the AMPLIFY CLI and authorize it to use the
  AMPLIFY Central APIs. This enables you to integrate the CLI into your DevOps
  pipeline.
---

## Before you start

* You will need an administrator account for AMPLIFY Central ([Managing Accounts](https://docs.axway.com/bundle/AMPLIFY_Dashboard_allOS_en/page/managing_accounts.html)).
* You will need [Node.js](https://nodejs.org/en/download/) version 10.13.0 or later.

### Operating system supported configurations

The following table describes the operating system supported configurations that you can use with AMPLIFY Central CLI.

| OS                  | Node.js Version  | Terminal Shells Supported  | Terminal Shells Unsupported |
| ------------------- | ---------------- | -------------------------- | --------------------------- |
| Mac OSX or later    | 10.13.0 or later | bash , zsh                 |                             |
| Windows 10 or later | 10.13.0 or later | Command Prompt, PowerShell | Cygwin, Git Bash            |
| Linux               | 10.13.0 or later | bash                       |                             |

## Install AMPLIFY CLI and AMPLIFY Central CLI

1. Install `Node.js 10.13.0 LTS` or later.
2. Run the following command to install AMPLIFY CLI:

   ```
   [sudo] npm install -g @axway/amplify-cli
   ```

   Use `sudo` on Mac OS X or Linux if you do not own the directory where npm installs packages to. On Windows, you do not need to run as Administrator as npm installs packages into your AppData directory.

3. Run AMPLIFY package manager to install AMPLIFY Central CLI:

   ```
   [sudo] amplify pm install @axway/amplify-central-cli
   ```

4. Run AMPLIFY package manager list command to view available packages:

   ```
   amplify pm list
   ```

   Expected response:

   ```
   AMPLIFY CLI, version 1.4.0
   Copyright (c) 2018, Axway, Inc. All Rights Reserved.
   NAME                           | INSTALLED VERSIONS             | ACTIVE VERSION
   @axway/amplify-central-cli     | 0.1.7,0.1.8,0.1.9              | 0.1.9
   ```

All the development versions of AMPLIFY Central CLI can be found at [NPM install of AMPLIFY Central CLI](https://www.npmjs.com/package/@axway/amplify-central-cli). To install a specific development version, run the following command:

```
[sudo] amplify pm install @axway/amplify-central-cli@0.1.8-dev.10
```

### Additional installation steps on Windows

After successfully installing Amplify Central CLI, you must check if OpenSSL is installed. OpenSSL is needed to generate a public and private key pair for service account authentication, which is a pre-requisite for the creation of service accounts.

Install OpenSSL if not installed already:

1. [Download OpenSSL](https://slproweb.com/products/Win32OpenSSL.html).
2. Install OpenSSL, and ensure it is added to your path (`C:\Program Files\OpenSSL-Win64\bin`) in environment variables.

   ![Environment variables](/Images/central/cli_central/env_variables.png)

3. Verify that OpenSSL is installed and configured correctly.

   ```
    openssl version
   ```

## Authorize your CLI to use the AMPLIFY Central APIs

Before using the AMPLIFY Central APIs you must first authorize your CLI, so you can use it, for example, as part of your DevOps pipeline.

You can use the following options to authorize your CLI:

1. [Use your AMPLIFY Platform login credentials](/docs/central/cli_central/cli_install/#option-1---log-in-with-your-amplify-platform-credentials).
2. [Use a service account](/docs/central/cli_central/cli_install/#option-2---authenticate-and-authorize-your-service-account).

### Option 1 - Log in with your AMPLIFY Platform credentials

To use Central CLI to log in with your AMPLIFY Platform credentials, run the following command:

```
amplify auth login
```

On the dialog box displayed, enter valid credentials (email address and password). An "Authorization Successful" message is displayed, and you can go back to the Central CLI.

If you are a member of multiple AMPLIFY organizations, you might have to choose an organization. After that, you have completed the authorization of your CLI.

If you have used the `client-id` configuration to authorize with the CLI, you must remove it. To verify that you have used `client-id`, run:

```bash
amplify central config list
```

Expected response:

```bash
{
   ...
   'client-id': 'apicentral',
   ...
}
# OR if used a "DOSA" account before
{
   ...
   'client-id': 'DOSA_105cf15d051c432c8cd2e1313f54c2da',
   ...
}
```

To remove `client-id`, you must manually edit the configuration file `~/.axway/central.json` and remove the `client-id` value from it.

### Option 2 - Authenticate and authorize your service account

To use the Central CLI, your service account must authenticate with AMPLIFY Platform and it must be authorized to use the AMPLIFY Central APIs.

You can use the following options to create your service account:

#### 2.1 Create a service account using the CLI

To create a service account from the CLI, run the following command (You must have OpenSSL installed to run this command):

```
amplify central create service-account
```

You will be prompted to provide a name for the service account. A public and private key pair in RSA format will be generated for you.

#### 2.2 Create a service account using the AMPLIFY Central UI

To create a service account from the UI, log in to AMPLIFY Central UI as an administrator, and create a service account for your CLI. Add the public key that you created earlier. When the account is created, copy the client identifier from the **Client ID** field.

Watch the animation to learn how to do this in AMPLIFY Central UI.

![Create service account in AMPLIFY Central UI](/Images/central/service_account_animation.gif)

#### Authorize the service account with AMPLIFY platform

After you create a service account your must authorize it with AMPLIFY platform, and log in to AMPLIFY CLI using the following command:

```
amplify auth login --client-id DOSA_105cf15d051c432c8cd2e1313f54c2da --secret-file ~/test/private_key.pem
```

Expected response:

```
AMPLIFY CLI, version 1.4.0
Copyright (c) 2018, Axway, Inc. All Rights Reserved.

You are logged into 8605xxxxxxx28 as DOSA_5ed74d68defxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx604.

This account has been set as active.
```

#### Set the active service account

To set the service account client identifier for future operations:

```
amplify central config set --client-id DOSA_105cf15d051c432c8cd2e1313f54c2da
```

To view the saved configuration, run:

```
amplify central config list
```

Expected response:

```
{ 'client-id': 'DOSA_105cf15d051c432c8cd2e1313f54c2da' }
```

## Review

You have learned how to install the AMPLIFY CLI and how to register or link it to a service account, or to the AMPLIFY Platform account. Now, you can integrate the AMPLIFY CLI into your DevOps pipeline to automate actions in AMPLIFY Central.
