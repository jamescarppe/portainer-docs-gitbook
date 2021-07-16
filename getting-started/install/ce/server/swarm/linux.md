# Install Portainer with Docker Swarm on Linux

Portainer is comprised of two elements, the **Portainer Server** and the **Portainer Agent**. Both elements run as lightweight Docker containers on a Docker engine.

To get started, you will need:

* The latest version of Docker installed and working
* Swarm mode enabled and working, including the overlay network for the swarm service communication
* `sudo` access on the manager node of your swarm cluster
* By default, Portainer will expose the UI over port `9000` and expose a TCP tunnel server over port `8000`. The latter is optional and is only required if you plan to use the Edge compute features with Edge agents.
* The manager and worker nodes must be able to communicate with each other over port `9000`.

The installation instructions also make the following assumptions about your environment:

* You are accessing Docker via Unix sockets. Connecting via TCP is not supported in Docker Swarm.
* SELinux is disabled on the machine running Docker. If you require SELinux, you will need to pass the `--privileged` flag to Docker when deploying Portainer.
* Docker is running as root. Portainer with rootless Docker has some limitations, and requires additional configuration.
* If your nodes are using DNS records to communicate, that all records are resolvable across the cluster.

### Portainer Server Deployment

Portainer can be directly deployed as a service in your Docker cluster. Note that this method will automatically deploy a single instance of the Portainer Server, and deploy the Portainer Agent as a global service on every node in your cluster.

First, retrieve the stack YML manifest:

```bash
curl -L https://downloads.portainer.io/portainer-agent-stack.yml -o portainer-agent-stack.yml
```

Then, use the downloaded YML manifest to deploy your stack:

```bash
docker stack deploy -c portainer-agent-stack.yml portainer
```

### Portainer Agent Deployment

To deploy Portainer Agent on a remote swarm cluster as a swarm service, run the following commands on a manager node in the remote cluster.

First, create the network:

```text
docker network create --driver overlay --attachable portainer_agent_network
```

Then, deploy the agent:

```text
docker service create --name portainer_agent --network portainer_agent_network \
    --publish mode=host,target=9001,published=9001 -e AGENT_CLUSTER_ADDR=tasks.portainer_agent \
    --mode global --mount type=bind,src=//var/run/docker.sock,dst=/var/run/docker.sock \
    --mount type=bind,src=//var/lib/docker/volumes,dst=/var/lib/docker/volumes \
    --mount type=bind,src=/,dst=/host portainer/agent
```

