# Docker

## Installation

### MacOS

Installation Instructions - [https://docs.docker.com/docker-for-mac/install/](https://docs.docker.com/docker-for-mac/install/)

### Windows

Installation Instructions for **Windows 10 Pro with Entreprise and Education(1607 Anniversary update, Build 14393or later)** - [https://docs.docker.com/docker-for-windows/install/](https://docs.docker.com/docker-for-windows/install/)

Installation Instructions for other Windows version :

- Install **Docker-Toolbox** from [https://download.docker.com/win/stable/DockerToolbox.exe](https://download.docker.com/win/stable/DockerToolbox.exe)
- Double click on setup and follow on-screen instructions and do not forget to check Oracle Virtual Box if you do not have installed previously in the Docker Toolbox set up window.
- Once installed the Docker Toolbox Quickstart Icon is available on Desktop. Open it. (It will create a default machine)
- Run the command `docker-machine ip` to get the IP address of docker machine.

### Windows Subsystem for Linux

- Installation instructions for Ubuntu as a Windows subsystem - [https://docs.microsoft.com/en-us/windows/wsl/install-win10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

- Installation instructions for Docker-Toolbox - [https://download.docker.com/win/stable/DockerToolbox.exe](https://download.docker.com/win/stable/DockerToolbox.exe)

- Launch Docker QuickStart

- Installation instructions of docker-client on windows subsystem Ubuntu : [https://medium.com/@sebagomez/installing-the-docker-client-on-ubuntus-windows-subsystem-for-linux-612b392a44c4](https://medium.com/@sebagomez/installing-the-docker-client-on-ubuntus-windows-subsystem-for-linux-612b392a44c4)

- Run

```bashscript
export DOCKER_HOST=tcp://192.168.99.100:2376 // your Docker IP
export DOCKER_CERT_PATH=/mnt/c/Users/YOUR_USERNAME/.docker/machine/certs
export DOCKER_TLS_VERIFY=1
```

- Restart WSL