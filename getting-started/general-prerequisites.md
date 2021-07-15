# General Prerequisites

The following describes the general requirements and prerequisites you'll need to run Portainer on your infrastructure. Depending on your environment you may have additional specific needs that will be detailed later in the setup process.

### Validated Configurations

Every single release of Portainer goes through an extensive testing process \(functional tests, release tests, post release tests\) to ensure that what we are creating actually works as expected. Obviously though, we cannot possibly test Portainer against every single configuration variant out there, so we have elected to test against just a subset.

To try and alleviate confusion as to what we test against, we have documented the configurations that we personally validate as "functional"; any other variant is not tested \(this does not mean it won't work, it just means its not tested\).

### Persistent Storage

Portainer Server requires persistent storage in order to maintain the database and configuration information needed to function, and our installation process will outline a basic storage configuration for your platform. By default, both Docker and Kubernetes provide local \(to the node\) storage only, and if cluster-wide persistent storage is desired we recommend implementing this at the infrastructure level \(for example, via NFS\).

### SSL

By default, Portainer's web interface and API are exposed over HTTP. This is not secure, and we recommend enabling SSL, particularly in a production environment. Detail on how to achieve this for your specific platform will be provided in the installation instructions.

### Ports

In order to access the UI and API, and for the Portainer Server instance and the Portainer Agents to communicate, certain ports need to be accessible. Portainer Server listens on port 9000 for the UI and API \(or 30777 for Kubernetes with NodePort\) and exposes a TCP tunnel server on port 8000 \(this second port is optional and only needed to use Edge Compute features with Edge agents\). The Portainer Agents listen on port 9001 \(or 30778 for Kubernetes with NodePort\).

All of these ports can be changed at installation time.

