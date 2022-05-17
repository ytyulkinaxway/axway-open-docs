{
"title": "Install Discovery and Traceability agents",
"linkTitle": "Install Discovery and Traceability agents",
"weight":"20",
"date": "2022-05-12",
"description": "Prerequisites and steps for installing Discovery and Traceability agents."
}

Discovery and Traceability agents are used to connect your API Management system to Amplify Central.

## Prerequisites

Ensure that all of the prerequisites detailed in [prerequisites](/docs/apim_installation/apigtw_install/system_requirements) are met.
In addition to that, you need to hav valid private and public keys to connect with Amplify Central.

## Install Discovery and Traceability agents

To install Discovery and Traceability agents in GUI mode, perform an installation following the steps described in [Installation](/docs/apim_installation/apigtw_install/installation), using the following selections:

* Select the **Custom** setup type.
* Select to install the following components:
    * Discovery and Traceability agents

### Unattended mode

To install Discovery and Traceability agents in unattended mode, follow the steps described in [Unattended installation](/docs/apim_installation/apigtw_install/installation_unattended).

The following example shows how to install the Discovery and Traceability agents in unattended mode:

```
./APIGateway_7.7_Install_linux-x86-32_BN<n>.run --mode unattended --setup_type advanced --enable-components agentsConfiguration --disable-components apimgmt,apigateway,nodemanager,cassandra, qstart,policystudio,configurationstudio,analytics,apitester,packagedeploytools
```
