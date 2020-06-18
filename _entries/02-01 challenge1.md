---
sectionid: deploy
sectionclass: h2
title: Create an Azure Kubernetes Service (AKS) Cluster
parent-id: upandrunning
---

This step has been automated for the condensed workshop. Instead, we will discover and authenticate to the existing AKS cluster in your environment, so you can execute kubectl commands.

### Tasks

#### Discover the AKS cluster

{% collapsible %}

```sh
cluster=$(az aks list --query "[*].name" --output tsv); echo $cluster
```

{% endcollapsible %}

#### Access AKS cluster with kubectl

**Task Hints**
* `kubectl` is the main command line tool you will be using for working with Kubernetes and AKS. It is already installed in the Azure Cloud Shell
* Refer to the AKS docs which includes [a guide for connecting kubectl to your cluster](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough#connect-to-the-cluster) (Note. using the cloud shell you can skip the `install-cli` step).
* A good sanity check is listing all the nodes in your cluster `kubectl get nodes`.
* [This is a good cheat sheet](https://linuxacademy.com/site-content/uploads/2019/04/Kubernetes-Cheat-Sheet_07182019.pdf) for kubectl.

{% collapsible %}
Authenticate:

```sh
group=$(az group list --query "[].{Name:name}" --output tsv); echo $group

az aks get-credentials --resource-group $group --name $cluster
```

Check cluster connectivity by listing the nodes in your cluster:

```sh
kubectl get nodes
```
{% endcollapsible %}

#### Explore Capture Order Application

A three tiered application has already been deployed to your AKS cluster. It includes a mongo database behind an API behind a frontend behind a NGINX ingress controller.

Let's quickly explore these components.

{% collapsible %}

First, notice the service created for the database, API and frontend in the default namespace:

```sh
kubectl get svc
```

Though the API is exposed externally, we'll access the frontend through the ingress controller using a DNS name that resolves to its external IP address:

```sh
kubectl get svc -n ingress
```

Copy the external IP address, and browse to a URL that includes it like so:

```sh
http://frontend.<ingress_external_ip_address>.nip.io
```

For example:


{% endcollapsible %}

> **Resources**
>
> * <https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough>
> * <https://docs.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest#az-aks-create>
> * <https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough-portal>
> * <https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough#connect-to-the-cluster>
> * <https://linuxacademy.com/site-content/uploads/2019/04/Kubernetes-Cheat-Sheet_07182019.pdf>
