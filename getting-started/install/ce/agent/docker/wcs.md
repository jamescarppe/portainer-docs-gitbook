# Install Portainer Agent with Docker on Windows Container Service



### Introduction

Portainer uses the **Portainer Agent** container to communicate with the **Portainer Server** instance and provide access to the node's resources. This document will outline how to install the Portainer Agent on your node and how to connect to it from your Portainer Server instance. If you do not have a working Portainer Server instance yet, please refer to the [Portainer Server installation guide](../../server/docker/wcs.md) first.

To get started, you will need:

* The latest version of Docker installed and working
* Administrator access on the machine that will host your Portainer Server instance
* Port `9001` accessible on this machine from the Portainer Server instance. If this is not available, we recommend using the Edge Agent instead.

### Deployment

To run Portainer Agent in a Windows Container scenario, you need to execute the following commands:

```bash
docker run -d -p 9001:9001 --name portainer_agent --restart=always -v \\.\pipe\docker_engine:\\.\pipe\docker_engine portainer/agent
```

### Adding Your New Endpoint

WIP

