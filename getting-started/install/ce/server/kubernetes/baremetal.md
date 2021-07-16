# Install Portainer with Kubernetes on your Self-Managed Infrastructure

{% embed url="https://www.youtube.com/watch?v=wxXi\_bmX\_Zw" %}

## Introduction

Portainer is comprised of two elements, the **Portainer Server** and the **Portainer Agent**. Both elements run as lightweight containers on Kubernetes.

To get started, you will need:

* A working and up to date Kubernetes cluster.
* Access to run `helm` or `kubectl` commands on your cluster.
* Cluster Admin rights on your Kubernetes cluster. This is so Portainer can create the necessary `ServiceAccount` and `ClusterRoleBinding` for it to access the Kubernetes cluster.
* A `default` StorageClass configured \(see below\).

The installation instructions also make the following assumptions about your environment:

* Kubernetes RBAC is enabled and working \(this is required for the access control functionality in Portainer\).
* You will be using the `portainer` namespace for Portainer.
* Kubernetes' metrics server is installed and working \(if you wish to use the metrics within Portainer\).

## Data Persistence

Portainer requires data persistence, and as a result needs at least one StorageClass available to use. Portainer will attempt to use the default StorageClass during deployment. If you do not have a StorageClass tagged as `default` the deployment will likely fail.

You can check if you have a default StorageClass by running the following command on your cluster:

```text
kubectl get sc
```

and looking for a StorageClass with `(default)` after its name:

```text
root@kubemaster01:~# kubectl get sc
NAME                            PROVISIONER                                   RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
managed-nfs-storage (default)   k8s-sigs.io/nfs-subdir-external-provisioner   Delete          Immediate           false                  11d
```

To set a StorageClass as default, you can use the following:

```text
kubectl patch storageclass <storage-class-name> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

replacing `<storage-class-name>` with the name of your StorageClass. Alternatively, if you are installing using our Helm chart, you can pass the following parameter in your helm install command to specify the StorageClass to use for Portainer:

```text
--set persistence.storageClass=<storage-class-name>
```

## Deployment

To deploy Portainer within a Kubernetes cluster you can either use our Helm chart, or alternatively our provided YAML manifests.

### Deploy using Helm

{% hint style="info" %}
Ensure you're using at least Helm v3.2, which includes support for the `--create-namespace` argument.
{% endhint %}

First add the Portainer Helm repository by running the following commands:

```text
helm repo add portainer https://portainer.github.io/k8s/
helm repo update
```

Once the update completes, you're ready to begin the installation. Which method you choose will depend on how you wish to expose the Portainer service:

{% tabs %}
{% tab title="Expose via NodePort" %}
Using the following command, Portainer will be available on port `30777`:

```text
helm install --create-namespace -n portainer portainer portainer/portainer
```
{% endtab %}

{% tab title="Expose via Ingress" %}
In this example, Portainer will be deployed to your cluster and assigned a Cluster IP, with an nginx Ingress Controller at the defined hostname. For more on Ingress options, refer to the list of Chart Configuration Options.

```text
helm install --create-namespace -n portainer portainer portainer/portainer \
  --set service.type=ClusterIP \
  --set ingress.enabled=true \
  --set ingress.annotations.'kubernetes\.io/ingress\.class'=nginx \
  --set ingress.hosts[0].host=portainer.example.io \
  --set ingress.hosts[0].paths[0].path="/"
```
{% endtab %}

{% tab title="Expose via Load Balancer" %}
Using the following command, Portainer will be available at an assigned Load Balancer IP on port `9000`:

```text
helm install --create-namespace -n portainer portainer portainer/portainer --set service.type=LoadBalancer
```
{% endtab %}
{% endtabs %}

### Deploy using YAML manifests

Our YAML manifests support exposing Portainer via either NodePort or Load Balancer.

{% tabs %}
{% tab title="Expose via NodePort" %}
To expose via NodePort, you can use the following command \(Portainer will be available on port `30777`\):

```text
kubectl apply -n portainer -f https://raw.githubusercontent.com/portainer/k8s/master/deploy/manifests/portainer/portainer.yaml
```
{% endtab %}

{% tab title="Expose via Load Balancer" %}
To expose via Load Balancer, this command will provision Portainer at an assigned Load Balancer IP on port `9000`:

```text
kubectl apply -n portainer -f https://raw.githubusercontent.com/portainer/k8s/master/deploy/manifests/portainer/portainer-lb.yaml
```
{% endtab %}
{% endtabs %}

## Logging In

Now that the installation is complete, you can log into your Portainer Server instance. Depending on how you chose to expose your Portainer installation, open a web browser and navigate to the following URL:

{% tabs %}
{% tab title="NodePort" %}
```bash
http://localhost:30777/
```
{% endtab %}

{% tab title="Ingress" %}
```bash
http://localhost:9000/
```
{% endtab %}

{% tab title="Load Balancer" %}
```bash
http://localhost:9000/
```
{% endtab %}
{% endtabs %}

Replace `localhost` with the relevant IP address or FQDN if needed, and adjust the port if you changed it earlier.

You will be presented with the initial setup page for Portainer Server.

{% page-ref page="../setup.md" %}



