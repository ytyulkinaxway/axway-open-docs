---
title: Traceability usage
linkTitle: Traceability usage
draft: false
weight: 10
description: Understand how the Traceability Agent reports the gateway usage to Amplify platform.
---
## Before you start

* Make sure your entitlement is correctly set up, based on your Gateway (AWS / Axway v7 / Azure). Refer to your Axway CSM for more information.

## Objectives

Learn how to set up Amplify platform and Traceability Agent to report the gateway usage data. Gateway usage data counts all API calls going through the Gateway.

### Preparing Amplify platform to receive usage event

In order to collect the usage metics, a platform environment is required.

{{% alert title="Note" %}}
This will soon be replaced by an automatic replication of environments in **Central > Topology**.
{{% /alert %}}

Follow these steps to create the platform environment:

 1. Log into Amplify platform with your user credentials

 2. Click on your user information and select the **Organization** menu.

    ![Organization menu](/Images/central/connected_agent_common_reference/step2_select_organization.png)

 3. Select **Environments** from the left menu and click the **+ Environment** button.

    ![Environment menu](/Images/central/connected_agent_common_reference/step3_select_environment.png)

 4. Name your environment, copy the Environment ID displayed on the screen (which you will need later), and click **Save**.

    ![Add environment](/Images/central/connected_agent_common_reference/step4_name_environment.png)

Amplify platform is now ready to receive usage events.

### Preparing Traceability Agent

The Traceability Agent that is attached to the gateway needs to know which platform environment the metrics will be reported to. Use the Traceability Agent configuration variable `CENTRAL_PLATFORMENVIRONMENTID` to identify the platform environment. Use the value from **Environment ID** in Step 4, above. If you forgot to copy it, open the **Environment** menu, identify your environment and copy the Environment ID.

The following is a sample value that is added to `ta_env_vars.env`, based on the environment that was created above in Step 4:

```shell
CENTRAL_PLATFORMENVIRONMENTID=8c22b0ae-19bb-460f-a068-f654d52e-24e2
```

If multiple gateways are attached to the same environment, update each Traceability Agent with the same `CENTRAL_PLATFORMENVIRONMENTID` variable value.

Once the Traceability Agent detects some traffic on the gateway, it will start counting the transactions. On a regular basis (5 minutes by default), the agent will send the usage counter to the platform.

The reporting interval can be changed using the `CENTRAL_EVENTAGGREGATIONINTERVAL` variable. You can use either a second, minute or hour notation.

Samples:

```shell
# report every 5 minutes expressed in second unit
CENTRAL_EVENTAGGREGATIONINTERVAL=300s

# report every 5 minutes expressed in minute unit
CENTRAL_EVENTAGGREGATIONINTERVAL=5m

# report every hour expressed in hour unit
CENTRAL_EVENTAGGREGATIONINTERVAL=1h
```

### Visualize the usage data from Amplify Central

Based on the frequency you choose, the agent will report to the platform the number of transactions that happen during each period.

Select the **Usage** menu to view the usage data.

![Usage data menu](/Images/central/connected_agent_common_reference/usage_data_menu.png)

The Usage data report has two tabs:

* **Monthy Usage** to visualize your monthly data. Filter either by month (to get a specific month's values), or products (if you have multiple product entitlements).
* **Report History** to view and download the usage report history. Filter the data per reporting period and/or per file name, environment name or status. Click **Download** to download a specific report.

![Usage report page](/Images/central/connected_agent_common_reference/usage_report_screen.png)
