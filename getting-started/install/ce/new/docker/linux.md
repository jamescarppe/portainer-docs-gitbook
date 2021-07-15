# Install Portainer with Docker on Linux

### Introduction

Portainer is comprised of two elements, the Portainer Server, and the Portainer Agent. Both elements run as lightweight Docker containers on a Docker engine.

To get started, you will need:

* The latest version of Docker installed and working
* sudo access on the machine that will host your Portainer instance
* By default, Portainer will expose the UI over port 9000 and expose a TCP tunnel server over port 8000. 

  The latter is optional and is only required if you plan to use the Edge compute features with Edge agents.

The installation instructions also make the following assumptions about your environment:

* You are accessing Docker via Unix sockets. Alternatively, you can also connect via TCP.
* SELinux is disabled on the machine running Docker. If you require SELinux, you will need to pass the --privileged flag 

  to Docker when deploying Portainer.

* Docker is running as root. Portainer with rootless Docker has some limitations, and requires additional configuration.

### Deployment

First, create the volume that Portainer Server will use to store its database:

```bash
docker volume create portainer_data
```

Then, download and install the Portainer Server container:

```bash
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```

Portainer Server has now been installed. You can proceed to setting up your installation.

{% page-ref page="../../setup.md" %}



