---
title: API Gateway and API Manager 7.7 May 2022 Release Notes
linkTitle: API Gateway and API Manager May 2022
weight: 95
date: 2022-03-22
description: API Gateway and API Manager updates are cumulative, comprising new
  features and changes delivered in previous updates unless specifically
  indicated otherwise in the Release notes.
---
## Installation

* To **update** your API Gateway, see [Update from API Gateway One Version](/docs/apim_installation/apigw_upgrade/upgrade_steps_oneversion/).
* To **upgrade** from an older version, see [Upgrade from API Gateway 7.5.x or 7.6.x](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/).
* For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).
* For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

### Update a container deployment

Any custom `.fed` files deployed to a container must be upgraded using [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics#upgradeconfig-options) or [projupgrade](/docs/apim_reference/devopstools_ref#projupgrade-command-options). They must be upgraded the same way, regardless of whether they are API Manager enabled or not. The `.fed` files contain the updates for the API Manager configuration and can be used to build containers.

## New features and enhancements

The following new features and enhancements are available in this update.

### placeholder 1

placeholder

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update, which may impact on your current installation.

### New redaction rules for API Gateway

New redaction rules have been defined for both [Admin Node Manager](/docs/apim_administration/apigtw_admin/admin_node_mngr/) and API Gateway instances. New API Gateway installations now have these rules enabled by default.

When upgrading existing installations, the default redaction files will be automatically installed.

The new default rules are *not* included in existing configurations, so you must modify your product's configuration files manually to include the new redaction files.

{{< alert title="Note" >}}
To ensure that a redaction output is compatible with API Gateway versions older than **May 2022 update**, observe the following:

* If the tag `action` is not present in new rules, the default action will be `replace`.
* If the tag `replaceBy` is not present, redaction will replace `multipart/form-data` values by an empty string and `application/x-www-form-urlencoded` values by the string `null`.
{{< /alert >}}

For more information on how to configure redaction and the format of new redaction rules, see [Redaction Rules](/docs/apim_administration/apigtw_admin/admin_redactors/).

### Support for Zulu OpenJDK and how to manually enable TLS algorithms

API Gateway and API Manager now support Zulu OpenJDK 1.8.0_322. This version of OpenJDK disables TLS algorithms version 1.0 and 1.1 by default, and this might impact database connections, LDAP connections, and other connection types if these connections require the use of these algorithms.

The following sections describe how to manually enable TLS algorithms.

#### API Gateway and API Manager

If you wish to enable these algorithms in your API Gateway or API Manager, add the `jdk.tls.disabledAlgorithms` Java security property to the jvm.xml file as follows, where `value` contains the desired list of disabled algorithms.

```xml
<SecurityProperty name="jdk.tls.disabledAlgorithms" value="MD2, MD5, SHA1 jdkCA & usage TLSServer,RSA keySize < 1024, DSA keySize < 1024, EC keySize < 224" />
```

#### Policy Studio

To enable these algorithms for Policy Studio, remove "TLSv1" and "TLSv1.1" from the `jdk.tls.disabledAlgorithms` property in the INSTALL_DIR/policystudio/jre/lib/security/java.security file.

### New system property to propagate API Manager security Invoke Policy generated headers

The ticket RDAPI-23601, from the [November 2021](/docs/apim_relnotes/20211130_apimgr_relnotes/#other-fixed-issues) release, added a functionality to propagate the headers (http.headers) generated as part of an Inbound security Invoke Policy execution for further processing by API Manager, resulting in the original request headers being overwritten.

Now, a new Java system property, `com.axway.apimanager.securitydevice.httpheaders.propagate`, has been added to propagate the generated headers when required, and the previous functionality of propagating the request headers for further processing is reinstated as default. For more information, see [System property changes](/docs/apim_reference/system_props/#77-may-2022).

## Deprecated features

As part of our software development life cycle we constantly review our API Management offering. As part of this update, the following capabilities have been deprecated

### Edge agent for API Gateway

The [Edge agent](https://docs.axway.com/bundle/subusage_en/page/deploy_the_agent.html) is removed from API Gateway, and it was replaced by the [Traceability and Discovery](https://docs.axway.com/bundle/amplify-central/page/docs/connect_manage_environ/connected_agent_common_reference/index.html) agents.

## End of support notices

The following items are end of support:

### placeholder 3

placeholder

## Removed features

To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this update, the following features have been removed:

<!--No features have been removed in this update.-->

### placeholder 4

placeholder.

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [Release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID                                | Cve Identifier | Description       |
| ----------- | -------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------ |
|RDAPI-24024|01330775  01334869  01343778  01323363  01332552  01255316  01331493  01344660  01282085  01343977  01345491|CVE-2021-2161  CVE-2021-2163  CVE-2021-35560|**Issue:** Openjdk version 1.8.0_322 is vulnerable to CVEs. **Resolution:** Openjdk is updated to 1.8.0_322 which is not vulnerable to these CVEs. Note that this Openjdk version disables TLS versions 1.0 and 1.1 by default.|

### Other fixed issues

placeholder

## Known issues

The following are known issues for this update.

### Scripting filter whiteboard attributes not preloaded for Jython scripts

The Scripting filter now uses a Jython 2.7 scripting environment (previously, Jython 2.5) to execute Jython scripts. As a result of this version change, the whiteboard attributes, such as `http.request.uri` and `http.request.verb`, are no longer preloaded for use by Jython scripts. However, you can run a Jython script to load these attributes before they are accessed as follows:

```
from com.vordel.trace import Trace

def invoke(msg):
    msg.forceGenerateAttributes()
    Trace.info("This trace statement was generated in script filter!  [" + str(msg.get("http.request.verb")) + "] [" + str(msg.get("http.request.uri")) + "]")
    return True
```

Related Issue: RDAPI-21363

### When an API Gateway instance is started, Xerces SAXParserImpl writes warnings to the error console

At API Gateway instance startup, the following warnings are logged to the error console, as opposed to the trace log:

```
Warning: org.apache.xerces.jaxp.SAXParserImpl$JAXPSAXParser: Property 'http://javax.xml.XMLConstants/property/accessExternalDTD' is not recognized.
Warning: org.apache.xerces.jaxp.SAXParserImpl$JAXPSAXParser: Property 'http://www.oracle.com/xml/jaxp/properties/entityExpansionLimit' is not recognized.
```

These new properties were added in JAXP 1.5 specification, which is supported by the embedded implementation in the JRE but not supported yet in Xerces-J Apache implementation. These are harmless warning messages, which are written to the error console instead of throwing an exception if a property is not supported by the Apache Xerces-J implementation.

Related Issue: RDAPI-22218

### API Gateway web service WSDL schema validation failure

If a web service is defined using multiple WSDLs, an error of 'Cannot find the declaration of element' might occur during the schema validation of a SOAP message. This might happen because of a duplication of the WSDLs types schema `targetNamespace`. To avoid this failure, you must change the types schema `targetNamespace` to be unique across the WSDLs.

Related Issue: RDAPI-26621

### Policy Studio installation update issue

An intermittent issue exists whereby after an installation update of Policy Studio, an error dialog is shown - "An error has occurred, see the log file", and the product will fail to start. To resolve this issue, copy the `org.apache.jasper.glassfish_2.2.2.v201205150955.jar` from the Policy Studio installation backup plugins directory to the main Policy Studio plugins directory.

Related Issue: RDAPI-26743

### YAML performance issue in Policy Studio

A Policy Studio performance issue has been observed when utilizing the YAML entity store and running on the Windows operating system. This can result in increased waiting times for certain UI user actions.

Related Issue: RDAPI-26745

### API Catalog Swagger 2.0 export issue when multiple API Manager traffic ports configured

When exporting Swagger 2.0 from API Manager Catalog with multiple traffic ports defined, if you try to subsequently reimport the generated document into another API Manager, the back-end URLs (HTTP and HTTPS) are correctly generated but the port will be incorrect for one of the URLs.

The issue is caused by an inherent flaw in Swagger 2.0 as it only permits one host to be specified. At export time, API Manager takes the first SSL-based basePath, if one exists, from the list of available basePaths (host:port) and applies that to the `$.host` field in the resultant Swagger 2.0 definition.

For example, if an HTTPS traffic port of `8065` and an HTTP traffic port of `8066` are configured, and the host IP address is `127.0.0.1`, then the generated Swagger 2.0 definition will look like this:

```json
{
  "swagger" : "2.0",
  "info" : {
    "description" : "",
    "version" : "1.0.3",
    "title" : "Test API"
  },
  "host" : "127.0.0.1:8065",
  "basePath" : "/api",
  "schemes" : [ "https", "http" ],
  "paths" : {...}
}
```

Note that the `$.host` field specifies a port of 8065. Importing this definition back into API Manager will result in the following back-end URLs, with the HTTP port being incorrect.

* `https://127.0.0.1:8065/api`
* `http://127.0.0.1:8065/api`

The recommendation is to use OpenAPI, which has the ability to specify multiple back-end hosts. For more information, see [OpenAPI Specification, Server Object](https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.0.2.md#serverObject).

Related Issue: RDAPI-23379

### When multiple API Manager traffic ports are configured, specifying a virtual host that contains a host and port can cause conflicting base path URLs to be displayed in API Catalog

In API Manager, if a virtual host (global default, organization level, or for a published API) is set to `myhost` and there are multiple traffic ports (mix of HTTP and HTTPS) configured, API Manager correctly displays `https://myhost` and `http://myhost` as base path URLs in the API Catalog.

An issue only arises when a port is specified as part of the virtual host. API Manager blindly takes the specified virtual host and appends it to the supported schemes for the configured traffic ports. So if a virtual host of `myhost:9999` is set, then conflicting base paths of `https://myhost:9999` and `http://myhost:9999` are displayed in the API Catalog.

Related Issue: RDAPI-23379

## Documentation

To find all available documentation for this product version:

1. Go to [Manuals on the Axway Documentation portal](https://docs.axway.com/bundle).
2. In the left pane **Filters** list, select your product or product version.

Customers with active support contracts must log in to access restricted content.

For information on the different operating systems, databases, browsers, and thick client platforms supported by each Axway product, see [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en).

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email [support@axway.com](mailto:support@axway.com) or visit [Axway Support](https://support.axway.com/).

See [Get help with API Gateway](/docs/apim_administration/apigtw_admin/trblshoot_get_help/) for the information that you should be prepared to provide when you contact Axway Support.
