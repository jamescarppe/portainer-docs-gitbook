# Install Portainer Agent with Docker Swarm on Windows Container Service

## Introduction

Portainer uses the **Portainer Agent** container to communicate with the **Portainer Server** instance and provide access to the node's resources. This document will outline how to install the Portainer Agent on your node and how to connect to it from your Portainer Server instance. If you do not have a working Portainer Server instance yet, please refer to the [Portainer Server installation guide](../../server/swarm/wcs.md) first.

## Deployment

To run Portainer Agent in a Windows Container scenario, you will need to execute the following commands.

First, download the YAML manifest:

```text
curl -L https://downloads.portainer.io/agent-stack-windows.yml -o agent-stack-windows.yml
```

Then, deploy the stack using the manifest:

```text
docker stack deploy --compose-file=agent-stack-windows.yml portainer-agent
```

## Adding Your New Endpoint

WIP

