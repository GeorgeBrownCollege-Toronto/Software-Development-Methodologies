# Build a Node.js and React app with npm 

## Prerequisites

For this tutorial, you will require:

- A macOS, Linux or Windows machine with:

    - 256 MB of RAM, although more than 512MB is recommended.

    - 10 GB of drive space for Jenkins and your Docker images and containers.

- The following software installed:

    - [Docker](https://www.docker.com/) - Read more about [installing Docker](https://www.jenkins.io/doc/book/installing/#installing-docker) in the Installing Docker section of the [Installing Jenkins](https://www.jenkins.io/doc/book/installing/) page.
 Note: If you use Linux, this tutorial assumes that you are not running Docker commands as the root user, but instead with a single user account that also has access to the other tools used throughout this tutorial.

- [Git](https://git-scm.com/downloads) and optionally [GitHub Desktop](https://desktop.github.com/).

## Run Jenkins in Docker

In this tutorial, you’ll be running Jenkins as a Docker container from the [jenkinsci/blueocean](https://hub.docker.com/r/jenkinsci/blueocean/) Docker image.

To run Jenkins in Docker, follow the relevant instructions below for either [macOS and Linux](#On-macOS-and-Linux) or [Windows](#On-Windows).

You can read more about Docker container and image concepts in the [Docker](https://www.jenkins.io/doc/book/installing#docker) and [Downloading and running Jenkins in Docker](https://www.jenkins.io/doc/book/installing#downloading-and-running-jenkins-in-docker) sections of the [Installing Jenkins](https://www.jenkins.io/doc/book/installing) page.

### On macOS and Linux

1. Open up a terminal window.

2. Create a [bridge network](https://docs.docker.com/network/bridge/) in Docker using the following [`docker network create`](https://docs.docker.com/engine/reference/commandline/network_create/) command:

    ```bashscript
    docker network create jenkins
    ```

3. Create the following [volumes](https://docs.docker.com/storage/volumes/) to share the Docker client TLS certificates needed to connect to the Docker daemon and persist the Jenkins data using the following [`docker volume create`](https://docs.docker.com/engine/reference/commandline/volume_create/) commands:

    ```bashscript
    docker volume create jenkins-docker-certs
    docker volume create jenkins-data
    ```

4. In order to execute Docker commands inside Jenkins nodes, download and run the `docker:dind` Docker image using the following [`docker container run`](https://docs.docker.com/engine/reference/commandline/container_run/) command:

    ```bashscript
    docker container run --name jenkins-docker --rm --detach \
    --privileged --network jenkins --network-alias docker \
    --env DOCKER_TLS_CERTDIR=/certs \
    --volume jenkins-docker-certs:/certs/client \
    --volume jenkins-data:/var/jenkins_home \
    --volume "$HOME":/home \
    --publish 3000:3000 docker:dind
    ```

5. Run the `jenkinsci/blueocean` image as a container in Docker using the following [`docker container run`](https://docs.docker.com/engine/reference/commandline/container_run/) command (bearing in mind that this command automatically downloads the image if this hasn’t been done):

    ```bashscript
    docker container run --name jenkins-tutorial --rm --detach \
    --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
    --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
    --volume jenkins-data:/var/jenkins_home \
    --volume jenkins-docker-certs:/certs/client:ro \
    --volume "$HOME":/home \ 
    --publish 8080:8080 jenkinsci/blueocean
    ```
`--volume jenkins-data:/var/jenkins_home` Maps the `/var/jenkins_home` directory in the container to the Docker volume with the name `jenkins-data`. If this volume does not exist, then this `docker container run` command will automatically create the volume for you.
`--volume "$HOME":/home` Maps the `$HOME` directory on the host (i.e. your local) machine (usually the `/Users/<your-username>` directory) to the `/home` directory in the container.

**Note:** If copying and pasting the command snippet above doesn’t work, try copying and pasting this annotation-free version here:

    ```bashscript
    docker container run --name jenkins-tutorial --rm --detach \
    --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
    --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
    --volume jenkins-data:/var/jenkins_home \
    --volume jenkins-docker-certs:/certs/client:ro \
    --volume "$HOME":/home --publish 8080:8080 jenkinsci/blueocean
    ```

6. Proceed to the [Setup wizard](#setup-wizard).

### On Windows

The Jenkins project provides a Linux container image, not a Windows container image. Be sure that your Docker for Windows installation is configured to run `Linux Containers` rather than `Windows Containers`. See the Docker documentation for instructions to [switch to Linux containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). Once configured to run Linux Containers, the steps are:

1. Open up a command prompt window.

2. Create a [bridge network](https://docs.docker.com/network/bridge/) in Docker using the following [`docker network create`](https://docs.docker.com/engine/reference/commandline/network_create/) command:

    ```bashscript
    docker network create jenkins
    ```
3. Create the following [volumes](https://docs.docker.com/storage/volumes/) to share the Docker client TLS certificates needed to connect to the Docker daemon and persist the Jenkins data using the following [`docker volume create`](https://docs.docker.com/engine/reference/commandline/volume_create/) commands:

    ```bashscript
    docker volume create jenkins-docker-certs
    docker volume create jenkins-data
    ```

4. In order to execute Docker commands inside Jenkins nodes, download and run the `docker:dind` Docker image using the following [`docker container run`](https://docs.docker.com/engine/reference/commandline/container_run/) command:

    ```bashscript
    docker container run --name jenkins-docker --rm --detach ^
    --privileged --network jenkins --network-alias docker ^
    --env DOCKER_TLS_CERTDIR=/certs ^
    --volume jenkins-docker-certs:/certs/client ^
    --volume jenkins-data:/var/jenkins_home ^
    --volume "%HOMEDRIVE%%HOMEPATH%":/home ^
    docker:dind
    ```

Run the `jenkinsci/blueocean` image as a container in Docker using the following docker container run command (bearing in mind that this command automatically downloads the image if this hasn’t been done):

    ```bashscript
    docker container run --name jenkins-tutorial --rm --detach ^
    --network jenkins --env DOCKER_HOST=tcp://docker:2376 ^
    --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 ^
    --volume jenkins-data:/var/jenkins_home ^
    --volume jenkins-docker-certs:/certs/client:ro ^
    --volume "%HOMEDRIVE%%HOMEPATH%":/home ^
    --publish 8080:8080 --publish 50000:50000 jenkinsci/blueocean
    ```

For an explanation of these options, refer to the [macOS and Linux](#on-macos-and-linux) instructions above.

6. Proceed to the [Setup wizard](#setup-wizard).
