# SPARTA App Deployment

This file is a Kubernetes deployment configuration for the "SPARTA" application.

- [SPARTA App Deployment](#sparta-app-deployment)
  - [Overview](#overview)
  - [Deployment](#deployment)
    - [1. API Version and Kind](#1-api-version-and-kind)
    - [2. Metadata](#2-metadata)
    - [3. Specification (`spec`)](#3-specification-spec)
    - [4. Replica Set Configuration](#4-replica-set-configuration)
    - [5. Template](#5-template)
    - [6. Container Configuration](#6-container-configuration)
  - [Service](#service)
    - [1. API Version and Kind](#1-api-version-and-kind-1)
    - [2. Metadata](#2-metadata-1)
    - [3. Specification (`spec`)](#3-specification-spec-1)
    - [4. Selector](#4-selector)
    - [5. Service Type](#5-service-type)
  - [Summary](#summary)

---

## Overview

This file defines a deployment resource in Kubernetes for the SPARTA application. It specifies various configurations for creating and managing multiple instances of the SPARTA application within a Kubernetes cluster.

The Service resource in Kubernetes allows for exposing the SPARTA app to external network requests, making it accessible outside the Kubernetes cluster. This service uses the `NodePort` type to map an internal container port to a port on each node in the cluster.

---

## Deployment

---

### 1. API Version and Kind

```yaml
apiVersion: apps/v1  # specify api to use for deployment
kind: Deployment     # type of Kubernetes object being created
```

- `apiVersion: apps/v1`: This line specifies the API version to use for this deployment configuration. `apps/v1` is the current standard API version for managing deployments in Kubernetes.
- `kind: Deployment`: This specifies that the configuration will create a `Deployment` resource, which is responsible for managing and scaling a set of application instances (or "pods").

---

### 2. Metadata

```yaml
metadata:
  name: app-deployment
```

- `metadata`: Provides metadata information for the deployment.
- `name: app-deployment`: This defines the name of the deployment, which can be used for referencing it in other configurations.

---

### 3. Specification (`spec`)

```yaml
spec:
  selector:
    matchLabels:
      app: sparta-app  # label to match pods to this deployment
```

- `spec`: The main configuration section for the deployment.
- `selector`: This identifies which pods belong to this deployment using labels.
  - `matchLabels`: The label `app: sparta-app` is used to find pods that match this label, ensuring that they are managed by this deployment.

---

### 4. Replica Set Configuration

```yaml
  replicas: 3  # specifies the number of pod replicas to run
```

- `replicas`: This specifies the number of instances (pods) to create and maintain in the deployment. Setting it to `3` ensures that three instances of the SPARTA application are running at all times.

---

### 5. Template

The `template` section specifies the configuration for the pod that will be created by this deployment.

```yaml
  template:
    metadata:
      labels:
        app: sparta-app
```

- `template`: Defines the configuration for the pods that this deployment will create.
- `metadata`: The metadata section of the template allows labels to be assigned to pods created by this deployment.
  - `labels`: Here, the `app: sparta-app` label is used, enabling the deployment to identify these pods and manage them.

---

### 6. Container Configuration

This section defines the main container for the SPARTA application, specifying its image and port.

```yaml
    spec:
      containers:
      - name: sparta-app
        image: shonifari8/sparta-app:v1
        ports:
        - containerPort: 3000
```

- `spec`: This is the specification for the pod’s containers.
  - `containers`: Defines the container(s) to run within each pod.
    - `name: sparta-app`: The name of the container within the pod.
    - `image: shonifari8/sparta-app:v1`: Specifies the Docker image for the container, including the repository (`shonifari8`), image name (`sparta-app`), and tag (`v1`).
    - `ports`: Defines the ports exposed by the container.
      - `containerPort: 3000`: This exposes port `3000` on the container, making it accessible for networking within the Kubernetes cluster.

---

## Service

This file defines a Kubernetes `Service` resource for the SPARTA application. This service exposes the application running on the pods to external traffic through a specific port.

### 1. API Version and Kind

```yaml
apiVersion: v1  # specifies the API version for the Service resource
kind: Service   # indicates this is a Service configuration
```

- `apiVersion: v1`: Specifies the Kubernetes API version to use for this configuration. `v1` is commonly used for basic Kubernetes resources such as services.
- `kind: Service`: Indicates that this configuration will create a `Service` resource, which manages network access to a set of pods.

---

### 2. Metadata

```yaml
metadata:
  name: sparta-app-svc
  namespace: default
```

- `metadata`: Provides metadata for the service resource.
- `name: sparta-app-svc`: Defines the name of the service, which is `sparta-app-svc`.
- `namespace: default`: Specifies the namespace in which the service is created. The `default` namespace is used if no other namespace is specified.

---

### 3. Specification (`spec`)

```yaml
spec:
  ports:
  - nodePort: 30001
    port: 3000
    targetPort: 3000
```

- `spec`: Main configuration for the service.
- `ports`: Defines the ports used for network access to the service.
  - `nodePort: 30001`: Specifies the port on each Kubernetes node to expose the application externally. This port will be accessible on each node in the cluster.
  - `port: 3000`: Defines the port on which the service is exposed within the cluster. Other services or resources within the cluster can access this service on port `3000`.
  - `targetPort: 3000`: Specifies the port on the target pod that the traffic is directed to. This should match the container port defined in the deployment.

---

### 4. Selector

```yaml
  selector:
    app: sparta-app  # Label to match service to deployment
```

- `selector`: This is used to find the pods that the service will route traffic to. The service routes requests to pods with the specified label.
  - `app: sparta-app`: Matches the label defined in the deployment file, ensuring the service only routes traffic to pods with the `app: sparta-app` label.

---

### 5. Service Type

```yaml
  type: NodePort
```

- `type: NodePort`: Specifies the type of service. `NodePort` exposes the service on a static port (in this case, `30001`) on each node in the Kubernetes cluster, making it accessible externally.

---

## Summary

The deployment configuration defines a set of three identical pods for the SPARTA application, each running a container from the `shonifari8/sparta-app:v1` Docker image and exposing port `3000`. The deployment is set up to automatically manage these pods, scaling and monitoring them based on the provided configurations.

The service configuration creates a NodePort service for the SPARTA application. It maps port `3000` on the container to port `3000` on the service, and then exposes it externally on port `30001` on each node in the cluster. The service routes traffic to any pod with the `app: sparta-app` label.
