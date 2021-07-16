# Install Portainer Agent with Docker Swarm on Linux

## Introduction

Portainer uses the **Portainer Agent** container to communicate with the **Portainer Server** instance and provide access to the node's resources. This document will outline how to install the Portainer Agent on your node and how to connect to it from your Portainer Server instance. If you do not have a working Portainer Server instance yet, please refer to the [Portainer Server installation guide](../../server/swarm/linux.md) first.

## Deployment

To deploy Portainer Agent on a remote swarm cluster as a swarm service, run the following commands on a manager node in the remote cluster.

First, create the network:

```bash
docker network create --driver overlay --attachable portainer_agent_network
```

Then, deploy the agent:

```bash
docker service create --name portainer_agent --network portainer_agent_network \
    --publish mode=host,target=9001,published=9001 -e AGENT_CLUSTER_ADDR=tasks.portainer_agent \
    --mode global --mount type=bind,src=//var/run/docker.sock,dst=/var/run/docker.sock \
    --mount type=bind,src=//var/lib/docker/volumes,dst=/var/lib/docker/volumes \
    --mount type=bind,src=/,dst=/host portainer/agent
```

## Adding Your New Endpoint

WIP

