# Install Portainer with Windows Container Service

### Introduction

Portainer is comprised of two elements, the Portainer Server, and the Portainer Agent. Both elements run as lightweight Docker containers on a Docker engine.

To get started, you will need:

* The latest version of Docker installed and working
* Administrator access on the machine that will host your Portainer instance
* By default, Portainer will expose the UI over port `9000` and expose a TCP tunnel server over port `8000`. The latter is optional and is only required if you plan to use the Edge compute features with Edge agents.

### Preparation

To run Portainer in a Windows Server/Desktop Environment you need to create exceptions in the firewall. These can easily be added through PowerShell by running the following commands:

```text
netsh advfirewall firewall add rule name="cluster_management" dir=in action=allow protocol=TCP localport=2377
netsh advfirewall firewall add rule name="node_communication_tcp" dir=in action=allow protocol=TCP localport=7946
netsh advfirewall firewall add rule name="node_communication_udp" dir=in action=allow protocol=UDP localport=7946
netsh advfirewall firewall add rule name="overlay_network" dir=in action=allow protocol=UDP localport=4789
netsh advfirewall firewall add rule name="swarm_dns_tcp" dir=in action=allow protocol=TCP localport=53
netsh advfirewall firewall add rule name="swarm_dns_udp" dir=in action=allow protocol=UDP localport=53
```

You will also need to install the Windows Container Host Service and install Docker:

```text
Enable-WindowsOptionalFeature -Online -FeatureName containers -All
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force
Install-Package -Name docker -ProviderName DockerMsftProvider
```

Once this is complete you will need to restart your Windows server. After the restart completes, you're ready to install Portainer itself.

### Deployment

First, create the volume that Portainer Server will use to store its database:

```text
docker volume create portainer_data
```

Then, download and install the Portainer Server container:

```text
docker run -d -p 9000:9000 --name portainer --restart always -v \.\pipe\docker_engine:\.\pipe\docker_engine -v portainer_data:C:\data portainer/portainer-ce
```

Portainer Server has now been installed. You can proceed to setting up your installation.

{% page-ref page="../../setup.md" %}



