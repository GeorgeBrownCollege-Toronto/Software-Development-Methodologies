# Install Jenkins on Docker

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

    ```bash
    docker network create jenkins
    ```

3. Create the following [volumes](https://docs.docker.com/storage/volumes/) to share the Docker client TLS certificates needed to connect to the Docker daemon and persist the Jenkins data using the following [`docker volume create`](https://docs.docker.com/engine/reference/commandline/volume_create/) commands:

    ```bash
    docker volume create jenkins-docker-certs
    docker volume create jenkins-data
    ```

4. In order to execute Docker commands inside Jenkins nodes, download and run the `docker:dind` Docker image using the following [`docker container run`](https://docs.docker.com/engine/reference/commandline/container_run/) command

    ```bash
    docker container run --name jenkins-docker --rm --detach \
    --privileged --network jenkins --network-alias docker \
    --env DOCKER_TLS_CERTDIR=/certs \
    --volume jenkins-docker-certs:/certs/client \
    --volume jenkins-data:/var/jenkins_home \
    --volume "$HOME":/home \
    --publish 3000:3000 docker:dind
    ```

5. Run the `jenkinsci/blueocean` image as a container in Docker using the following [`docker container run`](https://docs.docker.com/engine/reference/commandline/container_run/) command (bearing in mind that this command automatically downloads the image if this hasn’t been done):

    ```bash
    docker container run --name jenkins-tutorial --rm --detach \
    --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
    --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
    --volume jenkins-data:/var/jenkins_home \
    --volume jenkins-docker-certs:/certs/client:ro \
    --volume "$HOME":/home \ 
    --publish 8080:8080 jenkinsci/blueocean
    ```

- `--volume jenkins-data:/var/jenkins_home` maps the `/var/jenkins_home` directory in the container to the Docker volume with the name `jenkins-data`. If this volume does not exist, then this `docker container run` command will automatically create the volume for you.
- `--volume "$HOME":/home` maps the `$HOME` directory on the host (i.e. your local) machine (usually the `/Users/<your-username>` directory) to the `/home` directory in the container.

**Note:** If copying and pasting the command snippet above doesn’t work, try copying and pasting this annotation-free version here:

```bash
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

    ```bash
    docker network create jenkins
    ```
3. Create the following [volumes](https://docs.docker.com/storage/volumes/) to share the Docker client TLS certificates needed to connect to the Docker daemon and persist the Jenkins data using the following [`docker volume create`](https://docs.docker.com/engine/reference/commandline/volume_create/) commands:

    ```bash
    docker volume create jenkins-docker-certs
    docker volume create jenkins-data
    ```

4. In order to execute Docker commands inside Jenkins nodes, download and run the `docker:dind` Docker image using the following [`docker container run`](https://docs.docker.com/engine/reference/commandline/container_run/) command:

    ```bash
    docker container run --name jenkins-docker --rm --detach ^
    --privileged --network jenkins --network-alias docker ^
    --env DOCKER_TLS_CERTDIR=/certs ^
    --volume jenkins-docker-certs:/certs/client ^
    --volume jenkins-data:/var/jenkins_home ^
    --volume "%HOMEDRIVE%%HOMEPATH%":/home ^
    docker:dind

Run the `jenkinsci/blueocean` image as a container in Docker using the following docker container run command (bearing in mind that this command automatically downloads the image if this hasn’t been done):

```bash
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

### Accessing the Jenkins/Blue Ocean Docker container

If you have some experience with Docker and you wish or need to access the Jenkins/Blue Ocean Docker container through a terminal/command prompt using the [`docker container exec`](https://docs.docker.com/engine/reference/commandline/container_exec/) command, you can add an option like `--name jenkins-tutorial` to the `docker exec` command. That will access the Jenkins Docker container named "jenkins-tutorial".

This means you could access the Jenkins/Blue Ocean container (through a separate terminal/command prompt window) with a `docker container exec` command like:

```bash
docker container exec -it jenkins-tutorial bash
```

### Setup wizard
Before you can access Jenkins, there are a few quick "one-off" steps you’ll need to perform.

#### Unlocking Jenkins
When you first access a new Jenkins instance, you are asked to unlock it using an automatically-generated password.

1. After the 2 sets of asterisks appear in the terminal/command prompt window, browse to `http://localhost:8080` and wait until the **Unlock Jenkins** page appears.
![unlock jenkins page](./setup-jenkins-01-unlock-jenkins.jpg)

2. Display the Jenkins console log with the command:

```bash
docker logs jenkins-tutorial
```

3. From your terminal/command prompt window again, copy the automatically-generated alphanumeric password (between the 2 sets of asterisks).
![admin password](./setup-jenkins-02-copying-initial-admin-password.png)

4. On the **Unlock Jenkins** page, paste this password into the **Administrator password** field and click **Continue**.

#### Customizing Jenkins with plugins
After [unlocking Jenkins](#unlocking-jenkins), the Customize Jenkins page appears.

On this page, click Install suggested plugins.

![install suggested plugins](./setup-jenkins-03-install-suggested-plugins.png)

The setup wizard shows the progression of Jenkins being configured and the suggested plugins being installed. This process may take a few minutes

### Creating the first administrator user
Finally, Jenkins asks you to create your first administrator user.

When the **Create First Admin User** page appears, specify your details in the respective fields .

![first admin](./setup-jenkins-04-create-first-user.png)

and click **Save and Finish**

![save and finish] (./setup-jenkins-05-save-and-finish.png)

When the **Jenkins is ready** page appears, click **Start using Jenkins**.

**Notes:** This page may indicate **Jenkins is almost ready!** instead and if so, click **Restart**.

If the page doesn’t automatically refresh after a minute, use your web browser to refresh the page manually.

If required, log in to Jenkins with the credentials of the user you just created and you’re ready to start using Jenkins!

#### Stopping and restarting Jenkins
Throughout the remainder of this tutorial, you can stop the Jenkins/Blue Ocean Docker container by running `docker container stop jenkins jenkins-docker`.

To restart the Jenkins/Blue Ocean Docker container:

1. Run the same `docker run …`​ command you ran for [macOS, Linux](#on-macos-and-linux) or [Windows](#on-windows) above.

    **Note:** This process also updates the `jenkinsci/blueocean` Docker image, if an updated one is available.

2. Browse to `http://localhost:8080`.

3. Wait until the log in page appears and log in.