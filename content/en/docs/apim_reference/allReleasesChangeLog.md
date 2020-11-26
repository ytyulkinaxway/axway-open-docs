{
"title": "API Management release change log",
"linkTitle": "Release change log",
"weight":"90",
"date": "2020-10-20",
"description": "Functionality change log for all supported versions of API Gateway, API Manager, and API Portal since version 7.5.3."
}

List of the major changes made in each release of API Gateway, API Manager, and API Portal. This is a reference information to help you to decided the best strategy to migrate your product to the latest API Management release.

You can find the release notes for all service packs on [Axway Support](https://support.axway.com).

## Version 7.7

|Update       |Highlights        |Deprecated features  |Release Date|
|---------    |---               |---                  |---         |
|[7.7.20200930](/docs/apim_relnotes/20200930_apimgr_relnotes/)|API Manager Remote Hosts aligned with PolicyStudio functionality | API Management REST APIs versions 1.1 and 1.2 are deprecated. |30/09/2020|
|                                                                                                |YAML-based configuration store (Technical preview) |Swagger version 1.1 is deprecated |
|                                                                                                |Database integration updates |The requirement to run `update-apimanager.py` has been removed from the upgrade steps. | |
|                                                                                                |User membership in multiple organizations. | | |
|                                                                                                |New API Management REST API version 1.4. | | |
|                                                                                                |OpenJDK JRE update to v8u265. | | |
|                                                                                                |Changes to the way API Gateway verifies wildcarded certificates. | | |
|                                                                                                |The Groovy version used by the Scripting Filter is updated to 2.5.13. | | |
|[7.7.20200730](/docs/apim_relnotes/20200730_apimgr_relnotes/)|ICAP filter improvements. | |30/07/2020 |
|                                                                                                |Improved upgrade instructions. | | |
|                                                                                                |MSSQL Database Support. | | |
|[7.7.20200530](/docs/apim_relnotes/20200530_apimgr_relnotes/)|Updated OS Support. | |30/05/2020 |
|                                                                                                |Improved upgrade experience. | | |
|                                                                                                |Log4j file format changed to YAML. | | |
|                                                                                                |Swagger parser libs updated. | | |
|[7.7.20200330](/docs/apim_relnotes/20200330_apimgr_relnotes/)|Added TLS 1.3 support. |FIPS support was removed. |30/03/2020 |
|                                                                                                |OpenSSL updated to v1.1.1. |McAfee Sophos Clam AV embedded antivirus scanners were removed.||
|[7.7.20200130](/docs/apim_relnotes/20200130_apimgr_relnotes/)|Swagger 2.0 enhancements. |API Tester was removed from installation.|30/01/2020 |
|                                                                                                |Open API Specification (OAS) 3.0 enhancements. |RAML support was removed.| |
|                                                                                                |Try it improvements. |Export of back-end APIs is not supported for OAS3 or WSDL APIs.| |
|                                                                                                |Usage tracking improvements. |Documentation is no longer provided in PDF format.| |
|                                                                                                |Back-end API improvements. | | |
|                                                                                                |API documentation enhancements. | | |
|                                                                                                |Multi organization (beta). | | |
|                                                                                                |Increased validation of WSDLs. | | |
|                                                                                                |Filebeat updated to v6.2.2. | | |
|                                                                                                |Increased validation of users endpoint. | | |
|                                                                                                |Undesirable cipher suites disabled by default when using SSL/TLS. | | |
|                                                                                                |Endpoint identification algorithms for LDAPS enabled by default. | | |
|                                                                                                |Fields that contain confidential information are no longer returned in some API calls.| | |
|7.7 SP2|OpenSSL updated to 1.0.2t-fips to fix security vulnerabilities. | |21/12/2019 |
|7.7 SP1| | |29/08/2019 |
|[7.7](/docs/apim_relnotes/201904_release/)|Traffic monitor enhancements.|Support for MySQL Server 2005 has been removed.|12/04/2019 |
|                                                                                                |Filter JMS transactions by custom property in Traffic Monitor.| | |
|                                                                                                |Entity Explorer added to API Gateway client tools installer.| | |
|                                                                                                |Elastic topology container deployment enhancements.| | |

## Version 7.6.2

|Update       |Highlights        |Deprecated features  |Release Date|
|---------    |---               |---                  |---         |
|7.6.2 SP5|The jre directory in Policy Studio/Configuration Studio must be remove before applying the Service Pack| |17/07/2020|                          |User membership in multiple organizations | | |
|7.6.2 SP4|JQuery upadated to 3.4.0 to fix security vulnerabilities.| |19/07/2019 |
|7.6.2 SP3|jQuery updated to 3.3.1 to fix security vulnerabilities.| |18/04/2019 |
|                                                                                                                       |JRE updated to 1.8.0_202 to fix security vulnerabilities.| | |
|7.6.2 SP2| | | 17/12/2018|
|7.6.2 SP1|Spring framework updated to 4.3.18.RELEASE to fix security vulnerabilities | | 01/11/2018|
|                                                                                                                |OpenSSL updated to 1.0.2p-fips to fix security vulnerabilities.| | |
|                                                                                                                |Bouncy Castle library updated to 1.60 to fix security vulnerabilities.| | |
|[7.6.2](https://docs.axway.com/bundle/APIGateway_762_ReleaseNotes_allOS_en_HTML5/page/Content/ReleaseNotesPortal/APIGateway_ReleaseNotes_allOS_en.htm)|Elastic topology container deployment.|Axway physical appliance deployment option was removed. | 26/07/2018|
|                                                                                                                |Smooth rate limiting.|The ConnectToUrlFilter.removePreviousConnections system property is no longer available. | |
|                                                                           |Support for /tmp directory mounted with noexec- Apache Cassandra has been upgraded to version 2.2.12| | |

## Version 7.5.3

|Update       |Highlights        |Deprecated features  |Release Date|
|---------    |---               |---                  |---         |
|7.5.3 SP13|Dojo updated to 1.10.10 to fix security vulnerability |  |01/05/2020|
|7.5.3 SP12|Improvements in the XML redaction.|  |26/102019|
|                                                                                                               |Jackson-databind updated to 2.9.10 to fix security vulnerabilities.|  | |
|7.5.3 SP11|The Gateway now includes Zulu OpenJDK 1.8 JRE instead of Oracle JRE 1.8.|To allow legacy API method matching excluding the method’s defined MIME type, the Java property com.coreapireg.apimethod.contenttype.legacy must be set to true. |25/06/2019|
|                                                                                                               |It is possible to allow inbound API requests with trailing slash to match an API path with no trailing slash.|  | |
|                                                                                                               |An API method’s Content-Type is now checked against the API method’s defined MIME type when performing path matching.|  | |
|                                                                                                               |OpenSSL updated to 1.0.2r-fips.|  | |
|                                                                                                               |Added CSRF token protection for API Gateway Management APIs.|  | |
|                                                                                                               |JQuery updated to 3.4.0.|  | |
|7.5.3 SP10|Jython updated to 2.7.1 to fix this security vulnerability.|Internet Explorer versions 8.0 and 9.0 are no longer officially supported by API Gateway. |22/02/2019|
|                                                                                                               |jQuery updated to 3.3.1 to fix this security vulnerability.|  | |
|                                                                                                               |JRE updated to 1.8.0_202 to fix this security vulnerability.|  | |
|7.5.3 SP9|IBM MQ updated to 7.5.0.8 to fix this security vulnerability.| |16/11/2018|
|                                                                                                               |OpenSSL updated to 1.0.2p-fips to fix this security vulnerability.|  | |
|                                                                                                               |JRE updated to 1.8.0_191-b12 to fix this security vulnerability.|  | |
|                                                                                                               |`apimanager-promote` script requires decryption password or passfile.|  | |
|7.5.3 SP8|Bouncy Castle library updated to 1.60 to fix this security vulnerability.| |23/08/2018|
|                                                                                                               |Spring framework updated to 4.3.17.RELEASE to fix this security vulnerability.| | |
|                                                                                                               |JRE updated to 8u181 to fix this security vulnerability.| | |
|  
|7.5.3 SP7|Jackson-databind updated to 2.9.5 to fix this security vulnerability.| |08/06/2018|
|                                                                                                               |JRE updated to 8u172 to fix this security vulnerability.| | |
|                                                                                                               |OpenSSL updated to 1.0.2o to fix this security vulnerability.| | |
|7.5.3 SP6|OpenSSL updated to v1.0.2.n to fix security vulnerability.| |27/04/201|
|                                                                                                               |Apache Log4j updated to 2.8.2 to fix security vulnerability.| | |
|                                                                                                               |When exporting API collections from API Manager, the generated file is now encrypted and not plainText.| | |
|7.5.3 SP5|OpenSSL updated to 1.0.2m-fips to fix security vulnerability.| |23/01/2018|
|                                                                                                               |JQuery updated to 2.2.4, to fix security vulnerability.| | |
|                                                                                                               |commons-email updated to 1.5 to fix security vulnerability.| | |
|7.5.3 SP4|Nimbus JOSE+JWT library updated to v4.41.2 to fix security vulnerability.| |06/11/2017|
|                                                                                                               |RE version updated to v1.8u152 to fix security vulnerability.| | |
|7.5.3 SP3| | |02/10/2017|
|7.5.3 SP2|JSCH update to 0.1.54 to fix security vulnerabilities. | |31/07/2017|
|7.5.3 SP1|JRE updated to v8u131 to fix security vulnerability. | |15/06/2017|
|[7.5.3](https://docs.axway.com/bundle/APIGateway_753_ReleaseNotes_allOS_en_HTML5/page/Content/ReleaseNotesPortal/APIGateway_ReleaseNotes_allOS_en.htm)|Apache ActiveMQ updated to 5.14.3 to fix security vulnerabilities.| | 11/04/2017|
|                                                                                                               |JSCH update to 0.1.54 to fix security vulnerabilities.|Sun Access Manager filters have been deprecated and will be removed in a future release.| |
|                                                                                                               |owasp esapi updated to 2.1.0.1 to fix security vulnerabilities.|API Gateway Analytics is deprecated and will be removed in a future release. | |
|                                                                                                               |JRE updated to 8u121 to fix security vulnerabilities.|The OpenID connect filters in Policy Studio have been deprecated and will be removed in a future release.| |
|                                                                                                               |xmlsec updated to 1.5.8 to fix security vulnerabilities.|Support for Tivoli Access Manager version 6.0 has been removed and replaced by support for Tivoli Access Manager version version 6.1.| |
|                                                                                                               |spring-core updated to v4.3.5 to fix security vulnerabilities.|The ISO format for API Gateway virtual appliance has been replaced by the OVA format.| |
|                                                                                                               |Jsch dependency updated v0.1.54 to fix security vulnerabilities.| | |
|                                                                                                               |Remove Fluent-hc dependency from API Gateway.|The jabber and restJabber code samples had been removed | |
