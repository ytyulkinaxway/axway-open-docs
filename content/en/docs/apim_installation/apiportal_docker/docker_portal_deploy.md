{
  "title": "Build and run API Portal using Dockerfile",
  "linkTitle": "Build and run using Dockerfile",
  "draft": "true",
  "weight": "1",
  "date": "2019-08-09",
  "description": "Build an API Portal Docker image using the `Dockerfile` in the sample package."
}
This section describes how to build an API Portal Docker image using the `Dockerfile` in the sample package. It also describes how to download and run MySQL and Redis Docker containers, and how to run API Portal in Docker containers.

MySQL and Redis Docker images are not included with the API Portal Docker sample package. You must download them separately from [Docker Hub](https://hub.docker.com/).

## Prerequisites

The following components are required on your system before deploying API Portal in a Docker container:

* Docker version 1.13.x
* API Portal Docker samples package available from Axway Support at [https://support.axway.com](https://support.axway.com/)
* API Gateway and API Manager either installed on-premise or deployed in containers, see the [API Gateway Installation Guide](/docs/apim_installation/apigtw_install/) or the [API Gateway Container Deployment Guide](/docs/apim_installation/apigw_containers/) for more details.

The monitoring feature of API Portal, which enables your API consumers to monitor application and API usage, requires a connected API Manager with monitoring metrics enabled.

The following are the recommended hardware disk space and memory requirements for the Docker host machine for a single node sample architecture:

* 100 GB or more disk space
* 8 GB or more RAM

To control Docker instances you must be logged in as the `root` user or as a user in the `docker` group. To see the user groups that a user is a member of, use the `groups ${USER}` command.

## Upload API Portal Docker package

Follow these steps to upload the Docker sample package to your Docker host machine:

1. Download the API Portal Docker sample package from Axway Support at [https://support.axway.com](https://support.axway.com/).
2. Upload the package to your Docker host machine.
3. Unzip the package.

The package includes:

* `readme.md` – Details the contents of the package and provides brief instructions to build and run API Portal in Docker.
* `Dockerfile` – Enables you to build an API Portal Docker image using the `docker build` command.
* Various scripts and files that are used to build the API Portal Docker image (for example, the `.env.demo` file). See the `readme.md` for full details of these files.
* `docker-compose.yml` sample – Enables you to use Docker Compose to run your entire API Portal Docker deployment using a single `docker-compose up` command.
    * Provides a sample API Portal topology that you can customize to suit your requirements. For more information, see [Customize your API Portal topology in Docker](/docs/apim_installation/apiportal_docker/docker_config/).
    * Requires Docker Compose. Docker Compose is included by default in some Docker installations. For more information, see [Docker Compose documentation](https://docs.docker.com/compose/).

## Create API Portal network

To connect a database with API Portal without exposing the database to the outer world, create a Docker network. Complete the following steps:

1. Ensure that you are logged in to your Docker host machine as the `root` user or as a user in the `docker` group.
2. Run the following command:

    ```
    docker network create apiportal-net
    ```

## Download and run database container

To run a database Docker container, complete the following steps:

1. Ensure that you are logged in to your Docker host machine as the `root` user or as a user in the `docker` group.
2. Download a Docker image for your chosen database from [Docker Hub](https://hub.docker.com/). For example, to download a MySQL image, enter the following command:  

    ```
    docker image pull mysql:5.7
    ```

    The database must be supported by API Portal. For more information on supported databases, see [Software requirements](/docs/apim_installation/apiportal_install/install_software_prereqs/).

3. Run the database container following the instructions on [Docker Hub](https://hub.docker.com/). For example, to run a MySQL Docker container:  

    ```
    docker container run -d --name apiportal-db --network apiportal-net --network-alias db -e MYSQL_ROOT_PASSWORD=XXXXX -e MYSQL_DATABASE=joomla mysql:5.7
    ```  

    This example creates a database container named `apiportal-db` attached to the recently created network `apiportal-net`, with alias `db` in the network.

4. Verify that the database container is running:  

    ```
    docker container ls -f name=^apiportal-db$ -f status=running
    ```  

    When the database container is started it creates a volume to store the data from the database. This volume is created with a random name which you can identify using:
  
    ```
    docker container inspect apiportal-db --format '{{ json .Mounts }}'
    ```

## Build and run API Portal Docker container

To build the Docker image and run the API Portal Docker container, follow these steps:

1. Ensure that you are logged in to your Docker host machine as the `root` user or as a user in the `docker` group.
2. Change to the directory where you unzipped the Docker sample package.
3. Build the API Portal image. For example, enter the following command to build the image and tag it as `apiportal:7.7`:  

    ```
    docker build -t apiportal:7.7 .
    ```

4. Create data volumes for API Portal customized data, for example:  

    ```
    docker volume create apiportal-templates  
    docker volume create apiportal-images
    ```

    For more information about Docker volumes and a complete list of directories which you should preserve, see [Create Docker data volumes for persistence](/docs/apim_installation/apiportal_docker/docker_config/).

5. Run a container from the API Portal Docker image with the same data volumes. For example:

    ```
    docker container run -d --network apiportal-net --name apiportal -v apiportal-templates:/opt/axway/apiportal/htdoc/templates -v apiportal-images:/opt/axway/apiportal/htdoc/images -e MYSQL_HOST=db -e MYSQL_PORT=3306 -e MYSQL_ROOT_PASSWORD=XXXXX -e MYSQL_USERNAME=root -e MYSQL_PASSWORD=XXXXX -e MYSQL_DBNAME=joomla -e APIMANAGER_HOST=XXXXX -e APIMANAGER_PORT=XXXXX -p 443:443 apiportal:7.7
    ```

This example performs the following:

* Runs an API Portal Docker container from an image named `apiportal:7.7`
* Attaches to the `apiportal-net` Docker network to have access to the database.
* Sets environment variables for connecting to the MySQL container.
* Sets environment variables for connecting to API Manager.
* Binds port 443 of the container to port 443 on the host machine.
* Mounts volumes for customized data.

You can safely ignore the following messages, which might be displayed when running this command:

```
"ERROR 1060 (42S21) at line 1: Duplicate column name 'termsAndCond'"

"AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using <IP address>.
Set the 'ServerName' directive globally to suppress this message"
```

API Portal is now running in a Docker container.

Before you can use API Portal, you must link it to your API Manager. For more details, see [Connect API Portal to API Manager](/docs/apim_installation/apiportal_install/connect_to_apimgr/).

## Download and run Redis container

For better performance and scalability, you can configure API Portal to cache APIs in a Redis cache. Using a Redis cache is recommended if you plan to expose hundreds of APIs, or you plan to connect API Portal to more than one API Manager.

To download and run a Redis Docker container, complete the following steps:

1. Download the Redis Docker image from [Redis on Docker Hub](https://hub.docker.com/_/redis/). For example, enter the following command:

    ```
    docker pull redis:latest
    ```

2. Run the Redis Docker container, for example:

    ```
    docker container run -d --name apiportal-redis --network apiportal-net --network-alias redis redis:latest
    ```

Redis is now running in a Docker container. To configure API Portal to use the Redis cache, follow these steps:

1. Enter the following command to connect to the API Portal container:  

    ```
    docker container exec -it <your API Portal container name> /bin/bash
    ```

2. Open the following configuration file for editing with `vi`:

    ```
    vi /opt/axway/apiportal/htdoc/configuration.php
    ```

3. Locate the following line:  

    ```
    redis_server_host = 'localhost';
    ```

4. Change `localhost` to the IP address of the Redis container and save the file.

## Encrypt the Public API user password (optional)

To use the Public API mode in API Portal you must first run a script to encrypt the Public API user password and specify a directory to store the encryption key.

Perform the following steps after the API Portal Docker container is started:

1. Access the API Portal Docker container using the following command:

    ```
    docker exec -it <your API Portal container name> bash
    ```

2. Change to the directory where the `apiportal_encryption.sh` script is located and execute the script. For example:

    ```
    cd /
    ./apiportal_encryption.sh
    ```

3. Enter the full path to the file in which to store the encryption key.

The directory is created along with a file. The last segment of the directory is the file name, for example: `/sample/directory/for/encryption/key` creates an empty file named `key` in the desired directory.

After the script is finished, re-enter the password for the Public API mode user in JAI to encrypt and store it correctly.

{{< alert title="Note" color="primary" >}} You do not need to rerun the encryption script every time you start and stop the API Portal Docker container, but if you remove the container and create a new one using the same database (for example, when upgrading or setting up HA), you must rerun the encryption script and re-enter the password for the Public API user in JAI.{{< /alert >}}
