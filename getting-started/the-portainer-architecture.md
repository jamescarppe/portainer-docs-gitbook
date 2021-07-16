# The Portainer Architecture

## Introduction

Portainer is comprised of two elements, the **Portainer Server** and the **Portainer Agent**. Both elements run as lightweight containers on your existing containerized infrastructure. The Portainer Agent should be deployed to each node in your cluster and configured to report back to the Portainer Server container. A single Portainer Server will accept connections from any number of Portainer Agents, providing the ability to manage multiple clusters from one centralized interface.

To provide this, the Portainer Server container requires data persistence. The Portainer Agents are stateless however, with data being shipped back to the Portainer Server container.

{% hint style="info" %}
At present, we do not support running multiple instances of the Portainer Server container to manage the same clusters. As such, we generally recommend running Portainer Server on a specific management node, with the Portainer Agents deployed across the rest of your nodes.
{% endhint %}

## Agent vs Edge Agent

In standard deployments, the central Portainer Server instance and any endpoints it manages are assumed to be on the same network, that is, Portainer Server and the Portainer Agents are able to seamlessly communicate with one another. However, in environments where the remote endpoints are on a completely separate network to Portainer Server, say, across the internet, historically, we would have been unable to centrally manage these devices.

With the new Edge Agent, we altered the architecture, so that rather than Portainer Server needing seamless access the remote endpoint, now, only the remote endpoints need to be able to access Portainer Server. This communication is performed over an encrypted TLS tunnel. This is important in Internet connected environments where there is no desire to expose the Portainer Agent to the internet.

## Security / Compliancy

Portainer is run exclusively on your servers, within your network, behind your own firewalls. Because of this we do not currently hold any SOC or PCI/DSS compliance, as we do not host any element of your infrastructure. You can even run Portainer completely disconnected \(air-gapped\) without any impact on functionality.

While we do \(optionally\) collect anonymous usage analytics from Portainer installations, we remain compliant with GDPR. The collection of data can be disabled at installation \(or at any time afterwards\), and if your installation is air-gapped, collection will silently fail without any adverse effect.

{% page-ref page="general-prerequisites.md" %}

