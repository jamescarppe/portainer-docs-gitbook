# Install Portainer with Docker Swarm on Linux

{% embed url="https://www.youtube.com/watch?v=L80QDuix5RE" %}

## Introduction

Portainer is comprised of two elements, the **Portainer Server** and the **Portainer Agent**. Both elements run as lightweight Docker containers on a Docker engine. This document will help you deploy the Portainer Server and Agent containers on your Linux environment. To add a new Linux Swarm environment to an existing Portainer Server installation, please refer to the [Portainer Agent installation instructions](../../agent/swarm/linux.md).

To get started, you will need:

* The latest version of Docker installed and working
* Swarm mode enabled and working, including the overlay network for the swarm service communication
* `sudo` access on the manager node of your swarm cluster
* By default, Portainer will expose the UI over port `9000` and expose a TCP tunnel server over port `8000`. The latter is optional and is only required if you plan to use the Edge compute features with Edge agents.
* The manager and worker nodes must be able to communicate with each other over port `9001`.

The installation instructions also make the following assumptions about your environment:

* You are accessing Docker via Unix sockets. Connecting via TCP is not supported in Docker Swarm.
* SELinux is disabled on the machine running Docker. If you require SELinux, you will need to pass the `--privileged` flag to Docker when deploying Portainer.
* Docker is running as root. Portainer with rootless Docker has some limitations, and requires additional configuration.
* If your nodes are using DNS records to communicate, that all records are resolvable across the cluster.

## Deployment

Portainer can be directly deployed as a service in your Docker cluster. Note that this method will automatically deploy a single instance of the Portainer Server, and deploy the Portainer Agent as a global service on every node in your cluster.

First, retrieve the stack YML manifest:

```bash
curl -L https://downloads.portainer.io/portainer-agent-stack.yml -o portainer-agent-stack.yml
```

Then, use the downloaded YML manifest to deploy your stack:

```bash
docker stack deploy -c portainer-agent-stack.yml portainer
```

Portainer Server and the Agents have now been installed. You can check to see whether the Portainer Server and Agent containers have started by running `docker ps`:

```bash
root@manager01:~# docker ps
CONTAINER ID   IMAGE                           COMMAND                  CREATED              STATUS              PORTS                NAMES
59ee466f6b15   portainer/agent:latest          "./agent"                About a minute ago   Up About a minute                        portainer_agent.xbb8k6r7j1tk9gozjku7e43wr.5sa6b3e8cl6hyu0snlt387sgv
2db7dd4bfba0   portainer/portainer-ce:latest   "/portainer -H tcp:/…"   About a minute ago   Up About a minute   8000/tcp, 9000/tcp   portainer_portainer.1.gpuvu3pqmt1m19zxfo44v7izx
```

## Logging In

Now that the installation is complete, you can log into your Portainer Server instance by opening a web browser and going to:

```bash
http://localhost:9000/
```

Replace `localhost` with the relevant IP address or FQDN if needed, and adjust the port if you changed it from `9000` earlier.

You will be presented with the initial setup page for Portainer Server.

{% page-ref page="../setup.md" %}

