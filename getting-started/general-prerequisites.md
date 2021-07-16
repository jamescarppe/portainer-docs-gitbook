# General Prerequisites

The following describes the general requirements and prerequisites you'll need to run Portainer on your infrastructure. Depending on your environment you may have additional specific needs that will be detailed later in the setup process.

### Validated Configurations

Every single release of Portainer goes through an extensive testing process \(functional tests, release tests, post release tests\) to ensure that what we are creating actually works as expected. Obviously though, we cannot possibly test Portainer against every single configuration variant out there, so we have elected to test against just a subset.

To try and alleviate confusion as to what we test against, we have documented the configurations that we personally validate as "functional"; any other variant is not tested \(this does not mean it won't work, it just means its not tested\).

#### Community Edition

| Portainer Version | Release Date | Docker Version | Kubernetes Version | Architectures |
| :--- | :--- | :--- | :--- | :--- |
| Community 2.6.1 \(latest\) | Jul 12, 2021 | 20.10.5  20.10.6 | 1.19 1.20.2 1.21 | ARM64, x86\_64 |
| Community 2.6.0 | Jun 25, 2021 | 20.10.5  20.10.6 | 1.19 1.20.2 1.21 | ARM64, x86\_64 |
| Community 2.5.1 | May 18, 2021 | 20.10.5  20.10.6 | 1.19 1.20.2 1.21 | ARM64, x86\_64 |
| Community 2.5.0 | May 18, 2021 | 20.10.5 | 1.19 1.20.2 1.21 | ARM64, x86\_64 |
| Community 2.1.x | Feb 2, 2021 | 20.10.2 | 1.20.0 | ARM64, x86\_64 |
| Community 2.0.1 | Jan 7, 2021 | 20.10.0 | 1.17.13 1.18.9 1.19.3 1.20.0 | ARM64, x86\_64 |
| Community 2.0 | Aug 31, 2020 | 19.03.12 | 1.17.13 1.18.6 1.18.9 1.19.3 | ARM64, x86\_64 |
| 1.24.1 | Jul 23, 2020 | 19.03.12 | N/A | ARM32, ARM64, x86\_64 |
| 1.24.0 | Jun 2, 2020 | 19.03.10 | N/A | ARM32, ARM64, x86\_64 |
| 1.23.2 | Mar 25, 2020 | 19.03.6 | N/A | ARM32, ARM64, x86\_64 |

#### Business Edition

| Portainer Version | Release Date | Docker Version | Kubernetes Version | Architectures |
| :--- | :--- | :--- | :--- | :--- |
| Business 2.4 \(latest\) | May 4, 2021 | 20.10.5 | 1.19 1.20.2 1.21 | ARM64, x86\_64 |
| Business 2.0 | Dec 3, 2020 | 19.03.13 | 1.17.3 1.18.6 1.19.3 | ARM64, x86\_64 |

If you encounter an issue with a configuration that isn't listed above, we recommend first updating your environment to a validated configuration and retesting before reporting a bug.

### Persistent Storage

Portainer Server requires persistent storage in order to maintain the database and configuration information needed to function, and our installation process will outline a basic storage configuration for your platform. By default, both Docker and Kubernetes provide local \(to the node\) storage only, and if cluster-wide persistent storage is desired we recommend implementing this at the infrastructure level \(for example, via NFS\).

### SSL

By default, Portainer's web interface and API are exposed over HTTP. This is not secure, and we recommend enabling SSL, particularly in a production environment. Detail on how to achieve this for your specific platform will be provided in the installation instructions.

### Ports

In order to access the UI and API, and for the Portainer Server instance and the Portainer Agents to communicate, certain ports need to be accessible. Portainer Server listens on port `9000` for the UI and API \(or `30777` for Kubernetes with NodePort\) and exposes a TCP tunnel server on port `8000` \(this second port is optional and only needed to use Edge Compute features with Edge agents\). The Portainer Agents listen on port `9001` \(or `30778` for Kubernetes with NodePort\).

All of these ports can be changed at installation time.

{% page-ref page="install/" %}

