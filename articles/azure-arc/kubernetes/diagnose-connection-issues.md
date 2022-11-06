---
title: "Diagnose connection issues for Azure Arc-enabled Kubernetes clusters"
ms.date: 11/04/2022
ms.topic: how-to
description: "Learn how to resolve common issues when connecting Kubernetes clusters to Azure Arc."

---

# Diagnose connection issues for Azure Arc-enabled Kubernetes clusters

If you are experiencing issues connecting a cluster to Azure Arc, it's probably due to one of the issues listed here. We provide two flowcharts with guided help: one if you're [not using a proxy server](#connections-without-a-proxy), and one that applies if your network connection [uses a proxy server](#connections-with-a-proxy-server).

> [!TIP]
> The steps in this flowchart apply whether you're using Azure CLI or Azure PowerShell to [connect your cluster](quickstart-connect-cluster.md). However, some of the steps require the use of Azure CLI. If you haven't already [installed Azure CLI](/cli/azure/install-azure-cli), be sure to do so before you begin.

## Connections without a proxy

Review this flowchart in order to diagnose your issue when attempting to connect a cluster to Azure Arc without a proxy server. More details about each step are provided below.

:::image type="content" source="media/diagnose-connection-issues/no-proxy-flowchart.png" alt-text="Flowchart showing a visual representation of checking for connection issues when not using a proxy.":::

### Does the Azure identity have sufficient permissions?

Review the [prerequisites for connecting a cluster](quickstart-connect-cluster.md?tabs=azure-cli#prerequisites) and make sure that the identity you're using to connect the cluster has the necessary permissions.

### Is Azure CLI version above 2.30.0?

Make sure you [have the latest version installed](/cli/azure/install-azure-cli).

If you connected your cluster by using Azure PowerShell, make sure you are running [Azure PowerShell version 6.6.0 or later](/powershell/azure/install-az-ps).

### Is the `connectedk8s` extension the latest version?

Update the Azure CLI `connectedk8s` extension to the latest version by running this command:

```azurecli
az extension update --name connectedk8s
```

If you haven't installed the extension yet, you can do so by running the following command:

```azurecli
az extension add --name connectedk8s
```

### Is kubeconfig pointing to the right cluster?

Run `kubectl config get-contexts` to confirm the target context name. Then set the default context to the right cluster by running `kubectl config use-context <target-cluster-name>`.


### Are all required resource providers registered?

Be sure that the Microsoft.Kubernetes, Microsoft.KubernetesConfiguration, and Microsoft.ExtendedLocation resource providers are [registered](quickstart-connect-cluster.md#register-providers-for-azure-arc-enabled-kubernetes).

### Are all network requirements met?

Review the [network requirements](quickstart-connect-cluster.md#meet-network-requirements) and ensure that no required endpoints are blocked.

### Are all pods in the `azure-arc` namespace running?

If everything is working correctly, your pods should all be in the `Running` state. Run `kubectl get pods -n azure-arc` to confirm whether any pod's state is not `Running`.

### Still having problems?

The steps above will resolve many common connection issues, but if you're still unable to connect successfully, generate a troubleshooting log file and then [open a support request](/azure/azure-portal/supportability/how-to-create-azure-support-request) so we can investigate the problem further.

To generate the troubleshooting log file, run the following command:

```azurecli
az connectedk8s troubleshoot -g <myResourceGroup> -n <myK8sCluster>
```

When you [create your support request](/azure/azure-portal/supportability/how-to-create-azure-support-request), in the **Additional details** section, use the **File upload** option to upload the generated log file.

## Connections with a proxy server

If you are using a proxy server on at least one machine, complete the first five steps of the non-proxy flowchart (through resource provider registration) for basic troubleshooting steps. Then, if you are still encountering issues, review the next flowchart for additional troubleshooting steps. More details about each step are provided below.

:::image type="content" source="media/diagnose-connection-issues/proxy-flowchart.png" alt-text="Flowchart showing a visual representation of checking for connection issues when using a proxy." :::

### Is the machine executing commands behind a proxy server?

Be sure you have set all of the necessary environment variables. For more information, see [Connect using an outbound proxy server](quickstart-connect-cluster.md#connect-using-an-outbound-proxy-server).

### Does the proxy server only accept trusted certificates?

Be sure to include the certificate file path by including `--proxy-cert <path-to-cert-file>` when running the `az connectedk8s connect` command.

```azurecli
az connectedk8s connect --name <cluster-name> --resource-group <resource-group> --proxy-cert <path-to-cert-file>
```

### Is the proxy server able to reach required network endpoints?

Review the [network requirements](quickstart-connect-cluster.md#meet-network-requirements) and ensure that no required endpoints are blocked.

### Is the proxy server only using HTTP?

If your proxy server only uses HTTP, you can use `proxy-http` for both parameters.

If your proxy server is set up with both HTTP and HTTPS, run the `az connectedk8s connect` command with the `--proxy-https` and `--proxy-http` parameters specified. Be sure you are using `--proxy-http` for the HTTP proxy and `--proxy-https` for the HTTPS proxy.

```azurecli
az connectedk8s connect --name <cluster-name> --resource-group <resource-group> --proxy-https https://<proxy-server-ip-address>:<port> --proxy-http http://<proxy-server-ip-address>:<port>  
```

### Does the proxy server require skip ranges for service-to-service communication?

If you require skip ranges, use `--proxy-skip-range <excludedIP>,<excludedCIDR>` in your `az connectedk8s connect` command.

```azurecli
az connectedk8s connect --name <cluster-name> --resource-group <resource-group> --proxy-https https://<proxy-server-ip-address>:<port> --proxy-http http://<proxy-server-ip-address>:<port> --proxy-skip-range <excludedIP>,<excludedCIDR>
```

### Are all pods in the `azure-arc` namespace running?

If everything is working correctly, your pods should all be in the `Running` state. Run `kubectl get pods -n azure-arc` to confirm whether any pod's state is not `Running`.

### Still having problems?

The steps above will resolve many common connection issues, but if you're still unable to connect successfully, generate a troubleshooting log file and then [open a support request](/azure/azure-portal/supportability/how-to-create-azure-support-request) so we can investigate the problem further.

To generate the troubleshooting log file, run the following command:

```azurecli
az connectedk8s troubleshoot -g <myResourceGroup> -n <myK8sCluster>
```

When you [create your support request](/azure/azure-portal/supportability/how-to-create-azure-support-request), in the **Additional details** section, use the **File upload** option to upload the generated log file.


## Next steps

- View more [troubleshooting tips for using Azure Arc-enabled Kubernetes](troubleshooting.md).
- Review the process to [connect an existing Kubernetes cluster to Azure Arc](quickstart-connect-cluster.md).