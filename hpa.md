# Horizontal Autoscaling

## Step 1: Install Metrics Server

```sh
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

This command installs the **Metrics Server** in your Kubernetes cluster. The Metrics Server is a lightweight, efficient, and resource-aware metrics pipeline that provides CPU and memory usage data to Kubernetes. HPA relies on this data to scale pods based on resource consumption. We apply this configuration from the official Metrics Server release, making it easy to set up without creating our own manifest.

## Step 2: Check Metrics Server Pod Status

```sh
kubectl get pods -n kube-system | grep metrics-server
```

Here, we're checking the status of the **Metrics Server** pod within the `kube-system` namespace. After installing the Metrics Server, we want to verify that the server is running. The `grep metrics-server` part filters the output to show only the Metrics Server pod. This ensures that the Metrics Server deployment is successfully initialized and ready to collect and provide metrics data.

## Step 3: Describe Horizontal Pod Autoscaler

```sh
kubectl describe hpa
```

This command provides details about any **Horizontal Pod Autoscalers** (HPAs) in the cluster. The `describe hpa` command outputs information on the HPA configuration, such as the target resource usage (CPU/memory), the current usage, and the desired replica count. This step is crucial for ensuring that HPA has been set up correctly and that it's actively monitoring the application's resource usage.

## Step 4: Check Metrics Server Logs

```sh
kubectl -n kube-system logs deployment/metrics-server
```

We check the logs of the **Metrics Server deployment** to ensure that it's running as expected and not encountering any errors. Reviewing logs can help identify if the Metrics Server is successfully gathering and providing metrics or if there are connectivity or configuration issues.

## Step 5: Verify Metrics Server Deployment

```sh
kubectl get deployment -n kube-system metrics-server
```

This command verifies that the Metrics Server deployment exists and is healthy in the `kube-system` namespace. It's essential for understanding if the Metrics Server is correctly set up and if Kubernetes has created the necessary deployment resources.

## Step 6: Patch Metrics Server for Insecure TLS Connections

```sh
kubectl patch deployment metrics-server -n kube-system --type='json' -p='[
  {
    "op": "add",
    "path": "/spec/template/spec/containers/0/args/-",
    "value": "--kubelet-insecure-tls"
  }
]'
```

In certain environments, the Metrics Server may have trouble connecting securely with the **Kubelet** due to certificate verification issues. Here, we patch the Metrics Server deployment to add an argument (`--kubelet-insecure-tls`) to its configuration. This argument instructs the Metrics Server to skip TLS verification when connecting to Kubelet, which can help avoid connectivity issues in setups where certificate management is complex or unavailable.

## Step 7: Restart Metrics Server Deployment

```sh
kubectl rollout restart deployment metrics-server -n kube-system
```

After patching the deployment, we use this command to **restart the Metrics Server** deployment. Restarting is necessary for Kubernetes to apply the new configuration (`--kubelet-insecure-tls`). This command ensures that the changes take effect without requiring a full redeployment.

## Step 8: Check Metrics Server Logs Again

```sh
kubectl -n kube-system logs deployment/metrics-server
```

We check the logs again after the restart to confirm that the Metrics Server is now functioning correctly with the new configuration. This helps verify that the `--kubelet-insecure-tls` option resolved any connectivity issues.

## Step 9: View Node Metrics

```sh
kubectl top nodes
```

With the Metrics Server running, this command fetches the **current resource usage metrics** (CPU and memory) for each node in the cluster. It's an effective way to verify that the Metrics Server is accurately collecting metrics. This data allows us to validate that the HPA will have the necessary metrics data to make scaling decisions based on actual node usage.

---

In summary, each of these steps ensures that the Metrics Server is correctly set up to provide the necessary resource usage metrics, which the HPA uses to automatically scale application pods based on their CPU and memory usage. Proper configuration and verification of the Metrics Server are critical for the effective operation of Horizontal Pod Autoscaling in Kubernetes.