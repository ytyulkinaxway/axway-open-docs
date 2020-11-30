{
"title":"Run API Portal using ready-made Docker image",
"linkTitle":"Run using ready-made Docker image",
"weight":"2",
"date":"2019-08-09",
"description":"Use the ready-made API Portal Docker image to run in Docker containers."
}

This topic describes how to use the ready-made API Portal Docker image to run in Docker containers. The image is ready out-of-the-box, so you do not have to build it using the `Dockerfile`.

## Prerequisites

The following components are required on your system before you can deploy API Portal in Docker containers:

* [Docker engine](https://docs.docker.com/engine/).
* MySQL server.
* API Portal Docker image, available from [Axway Support](https://support.axway.com).

Optional components:

* Redis server, used for API Catalog caching.
* ClamAV, used for scanning of uploaded files.

The monitoring feature of API Portal, which enables your API consumers to monitor application and API usage, requires a connected API Manager with monitoring metrics enabled.

The following are the recommended hardware disk space and memory requirements for the Docker host machine for a single node sample architecture:

* 100 GB or more disk space
* 8 GB or more RAM

## Run a Docker container using the image

1. Download the API Portal Docker image from [Axway Support](https://support.axway.com).
2. Upload the file to your Docker host machine.
3. Enter the following command to load the image:

    `docker load -i APIPortal_7.7_Docker_Image_linux-x86-64_<build number>.tgz`

4. Run the API Portal Docker container, for example:

    ```
    docker container run --name apiportal \
      -d -p 8080:80 \
      -e MYSQL_HOST=mysql.axway.com \
      -e MYSQL_PORT=3306 \
      -e MYSQL_DATABASE=joomla \
      -e MYSQL_USER=joomla \
      -e MYSQL_PASSWORD=XXXXX \
      apiportal:7.7
    ```

    This example performs the following:

    * Runs an API Portal Docker container from an image named `apiportal`:`7.7` in detached mode.
    * Sets environment variables for connecting to the MySQL server.
    * Binds port 80 of the container to port 8080 on the host machine.

API Portal is now running in a Docker container.

To access your API Portal, you must first link it to your API Manager. For more details, see [Connect API Portal to API Manager](/docs/apim_installation/apiportal_install/connect_to_apimgr/).

If you plan to configure API Manager with environment variables, you must first [install API Manager and API Gateway](/docs/apim_installation/apigtw_install/) on-premise or in containers before you deploy API Portal in containers.

## Use environment variables to configure API Portal runtime

API Portal container supports a wide range of environment variables that allows you to configure API Portal runtime and Joomla! Administrator Interface (JAI) settings, partially.

The following is an example that you can copy and paste to an `env` file, then edit the values, and use the `env` file with `docker run` command:

```
##### NOTE #####
# Boolean environment variables can take a value of 0 or 1.
# `*_CONFIGURED` and `*_ON` variables are boolean.
################

##### REQUIRED SETTINGS #####
# This configuration settings are required for API Portal
# docker container to boot.
#############################

MYSQL_HOST=
MYSQL_PORT=3306
MYSQL_DATABASE=
MYSQL_USER=
MYSQL_PASSWORD=

##### OPTIONAL SETTINGS #####
# The rest of configuration settings are optional.
#############################

# Certificates can be passed in plain text or as base64 encoded string.
# Example:
# APACHE_SSL_CERT=base64:<base64-encoded-certificate>
# APACHE_SSL_PRIVATE_KEY=<plain-text-private-key>
APACHE_SSL_ON=0
APACHE_SSL_CERT=
APACHE_SSL_PRIVATE_KEY=

##### CHANGABLE SETTINGS #####
# The rest of configuration settings can be configured in JAI as well.
# `*_CONFIGURED` option determins if the feature is configured with
# environment variables, for example `APIMANAGER_CONFIGURED=0` means
# that the rest of `APIMANAGER_*` variables won't effect the runtime.
# On the other hand `APIMANAGER_CONFIGURED=1` will configure API Manager
# with values from environment variables.
#
# !!! With `*_CONFIGURED` option set to 1 all the changes made in JAI will be
# overriden by values from environment variables on the container restart
#
# `*_ON` option determins whether the feature is enabled or not
##############################

# For reference see "Connect API Portal to a single API Manager"
# under "Connect API Portal to API Manager" page in API Portal docs
APIMANAGER_CONFIGURED=0
APIMANAGER_NAME=Master
APIMANAGER_HOST=
APIMANAGER_PORT=8075

# For reference see "Customize Try-it by type of request"
# under "Customize API Catalog" page in API Portal docs.
# All `TRYIT_METHODS_*` vars are boolean
TRYIT_METHODS_CONFIGURED=0
TRYIT_METHODS_ENABLE_GET=1
TRYIT_METHODS_ENABLE_POST=1
TRYIT_METHODS_ENABLE_PUT=1
TRYIT_METHODS_ENABLE_DELETE=1
TRYIT_METHODS_ENABLE_PATCH=1
TRYIT_METHODS_ENABLE_HEAD=1
TRYIT_METHODS_ENABLE_OPTIONS=1

# For reference see "Enable scanning of uploaded files"
# section in "Secure API Portal" page in API Portal docs
CLAMAV_CONFIGURED=0
CLAMAV_ON=0
CLAMAV_HOST=
CLAMAV_PORT=3310

# For reference see reCapcha related topics
# under "Additional customizations" page in API Portal docs
# `LOGIN_PROTECTION_LOCK_IP` is boolean
LOGIN_PROTECTION_CONFIGURED=0
LOGIN_PROTECTION_ON=0
LOGIN_PROTECTION_ATTEMPTS_BEFORE_LOCK=3
LOGIN_PROTECTION_ATTEMPTS_BEFORE_RECAPCHA=3
LOGIN_PROTECTION_LOCK_DURATION_SEC=600
LOGIN_PROTECTION_LOCK_IP=0

# For reference see "API Portal single sign-on"
# page in API Portal docs
SSO_CONFIGURED=0
SSO_ON=0
SSO_PATH=
SSO_ENTITY_ID=
SSO_WHITELIST=

# For reference see "Secure API Portal"
# page in API Portal docs
OAUTH_WHITELIST_CONFIGURED=0
# Comma separated string
OAUTH_WHITELIST=

# For reference see "Secure API Portal"
# page in API Portal docs
API_WHITELIST_CONFIGURED=0
# Comma separated string
API_WHITELIST=

##### NON PERSISTING SETTINGS #####
# Settings under this secrtion don't persist. I.e. if you configure
# it in JAI they will be gone after container restart. So in common
# use case they should be configured via environment variables.
###################################

# For reference see "Install Redis cache"
# page in API Portal docs
REDIS_CONFIGURED=0
REDIS_ON=0
REDIS_HOST=
REDIS_PORT=6379
REDIS_CACHE_TIMEOUT_SEC=600
```

The following is an example of how to use the `env` file with `docker run` command:

```
docker container run --env-file .env \
  -e MYSQL_PASSWORD=very_secret_password \
  -e APACHE_SSL_PRIVATE_KEY="$(cat ~/certs/apiportal.key)" \
  <more-options...>
```

Note that inline environment variables take precedence over variables from the `env` file. So, in this example, `very_secret_password` and certificate taken from `apiportal.key` file will override the password and certificate in the `.env` file.

## Create data volumes to persist data

By default, API Portal container does not create volumes for data persistence. But, you might want to create Docker data volumes to persist API Portal customization data, prevent data loss when the container reboots or crashes, or when you are upgrading or setting up HA for an API Portal Docker deployment. If you are running API Portal in containers for a demo or test, there is no need to create data volumes.

The data volumes are stored in the Docker host machine, and as such they consume disk space. So, we recommend you to delete unused data volumes regularly.

The following list describes which API Portal assets you should store in a Docker volume to preserve the customizations during upgrade or HA setup of an API Portal Docker deployment:

* `/opt/axway/apiportal/enckey` - Encryption key directory. Used by Public API mode.
* `/opt/axway/apiportal/htdoc/images` - Images uploaded by API Portal users or Admins.
* `/opt/axway/apiportal/htdoc/language` - API Portal translations.
* `/opt/axway/apiportal/htdoc/templates` - Joomla! templates.
* `/opt/axway/apiportal/htdoc/administrator/language` - Joomla! admin panel translations.
* `/opt/axway/apiportal/htdoc/administrator/components/com_apiportal/assets/cert` - Certificates for API Manager.

The following is an example of how you can create data volumes:

```
# create volumes
docker volume create apiportal-enckey
docker volume create apiportal-images
docker volume create apiportal-language
docker volume create apiportal-templates
docker volume create apiportal-adm-language
docker volume create apiportal-certs

# start API Portal container using the created volumes
docker container run \
  -v apiportal-enckey:/opt/axway/apiportal/enckey \
  -v apiportal-images:/opt/axway/apiportal/htdoc/images \
  -v apiportal-language:/opt/axway/apiportal/htdoc/language \
  -v apiportal-templates:/opt/axway/apiportal/htdoc/templates \
  -v apiportal-adm-language:/opt/axway/apiportal/htdoc/administrator/language \
  -v apiportal-certs:/opt/axway/apiportal/htdoc/administrator/components/com_apiportal/assets/cert \
  <more-options>
```

As API Portal container runs as a non-root user, make sure that mounted directories are readable and writable by user with id `1048`. This user is not required to exist on the host machine though.
