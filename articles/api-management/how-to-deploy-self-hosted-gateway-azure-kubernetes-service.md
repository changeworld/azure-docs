---
title: Deploy a self-hosted gateway to Azure Kubernetes Service
description: Learn how to deploy self-hosted gateway component of Azure API Management to Azure Kubernetes Service
author: dlepow
ms.service: azure-api-management
ms.topic: how-to
ms.date: 06/11/2021
ms.author: danlep
---

# Deploy an Azure API Management self-hosted gateway to Azure Kubernetes Service

[!INCLUDE [api-management-availability-premium-dev](../../includes/api-management-availability-premium-dev.md)]

This article provides the steps for deploying self-hosted gateway component of Azure API Management to [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/). For deploying self-hosted gateway to a Kubernetes cluster, see the how-to article for deployment by using a [deployment YAML file](how-to-deploy-self-hosted-gateway-kubernetes.md) or [with Helm](how-to-deploy-self-hosted-gateway-kubernetes-helm.md).

> [!NOTE]
> You can also deploy self-hosted gateway to an [Azure Arc-enabled Kubernetes cluster](how-to-deploy-self-hosted-gateway-azure-arc.md) as a [cluster extension](/azure/azure-arc/kubernetes/extensions).

## Prerequisites

- [Create an Azure API Management instance](get-started-create-service-instance.md)
- Create an Azure Kubernetes cluster [using the Azure CLI](/azure/aks/learn/quick-kubernetes-deploy-cli), [using Azure PowerShell](/azure/aks/learn/quick-kubernetes-deploy-powershell), or [using the Azure portal](/azure/aks/learn/quick-kubernetes-deploy-portal).
- [Provision a gateway resource in your API Management instance](api-management-howto-provision-self-hosted-gateway.md).

## Deploy the self-hosted gateway to AKS

1. Select **Gateways** from under **Deployment and infrastructure**.
2. Select the self-hosted gateway resource you intend to deploy.
3. Select **Deployment**.
4. A new token in the **Token** text box was autogenerated for you using the default **Expiry** and **Secret Key** values. Adjust either or both if desired and select **Generate** to create a new token.
5. Make sure **Kubernetes** is selected under **Deployment scripts**.
6. Select **\<gateway-name\>.yml** file link next to **Deployment** to download the file.
7. Adjust the `config.service.endpoint`, port mappings, and container name in the .yml file as needed.
8. Depending on your scenario, you might need to change the [service type](/azure/aks/concepts-network-services). 
    * The default value is `LoadBalancer`, which is the external load balancer. 
    * You can use the [internal load balancer](/azure/aks/internal-lb) to restrict the access to the self-hosted gateway to only internal users. 
    * The sample below uses `NodePort`.
1. Select the **copy** icon located at the right end of the **Deploy** text box to save the `kubectl` command to clipboard.
1. Paste the command to the terminal (or command) window. The command expects the downloaded environment file to be present in the current directory.

   ```console
   kubectl apply -f <gateway-name>.yaml
   ```
   
1. Execute the command. The command instructs your AKS cluster to:
    * Run the container, using self-hosted gateway's image downloaded from the Microsoft Container Registry. 
    * Configure the container to expose HTTP (8080) and HTTPS (443) ports.
1. Run the below command to check the gateway pod is running. Your pod name will be different.

   ```console
   kubectl get pods
   NAME                                   READY     STATUS    RESTARTS   AGE
   contoso-apim-gateway-59f5fb94c-s9stz   1/1       Running   0          1m
   ```

1. Run the below command to check the gateway service is running. Your service name and IP addresses will be different.
    ```console
    kubectl get services
    NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
    contosogateway   NodePort    10.110.230.87   <none>        80:32504/TCP,443:30043/TCP   1m
    ```
1. Return to the Azure portal and confirm that gateway node you deployed is reporting healthy status.

> [!TIP]
> Use `kubectl logs <gateway-pod-name>` command to view a snapshot of self-hosted gateway log.

## Related content

* To learn more about the self-hosted gateway, see [Azure API Management self-hosted gateway overview](self-hosted-gateway-overview.md).
* Learn [how to deploy API Management self-hosted gateway to Azure Arc-enabled Kubernetes clusters](how-to-deploy-self-hosted-gateway-azure-arc.md).
* Learn more about the [observability capabilities of the Azure API Management gateways](observability.md).
* Learn more about guidance to [run the self-hosted gateway on Kubernetes in production](how-to-self-hosted-gateway-on-kubernetes-in-production.md).
* Learn more about [Azure Kubernetes Service](/azure/aks/intro-kubernetes).
