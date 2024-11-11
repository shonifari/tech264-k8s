# Kubernetes: A Comprehensive Overview

- [Kubernetes: A Comprehensive Overview](#kubernetes-a-comprehensive-overview)
  - [Why is Kubernetes Needed?](#why-is-kubernetes-needed)
  - [Benefits of Kubernetes](#benefits-of-kubernetes)
  - [Success Stories](#success-stories)
  - [Kubernetes Architecture](#kubernetes-architecture)
    - [Diagram](#diagram)
    - [Components](#components)
  - [The Cluster Setup](#the-cluster-setup)
    - [Master vs. Worker Nodes](#master-vs-worker-nodes)
  - [Pros and Cons of Managed Service vs. Self-Managed Kubernetes](#pros-and-cons-of-managed-service-vs-self-managed-kubernetes)
  - [Control Plane vs. Data Plane](#control-plane-vs-data-plane)
  - [Kubernetes Objects](#kubernetes-objects)
    - [Ephemeral Nature of Pods](#ephemeral-nature-of-pods)
  - [Mitigating Security Concerns with Containers](#mitigating-security-concerns-with-containers)
  - [Maintained Images](#maintained-images)
    - [Pros and Cons of Maintained Images](#pros-and-cons-of-maintained-images)
  - [What are the biggest challenges facing enterprises?](#what-are-the-biggest-challenges-facing-enterprises)
  - [How do these organisations handle Kubernetes?](#how-do-these-organisations-handle-kubernetes)
  - [Why/when would a business NOT want to use a microservices architecture?](#whywhen-would-a-business-not-want-to-use-a-microservices-architecture)

## Why is Kubernetes Needed?

Kubernetes, often abbreviated as "K8s," is a powerful open-source platform for automating the deployment, scaling, and management of containerized applications. As organizations shift towards microservices and containerized architectures, Kubernetes addresses several challenges:

- **Efficient Resource Management**: Kubernetes optimizes server usage, allowing applications to be scaled seamlessly to meet demand.
- **High Availability**: Kubernetes can automatically restart containers, reschedule workloads on different nodes, and distribute traffic effectively, ensuring uptime and resilience.
- **Portability**: Kubernetes abstracts the underlying infrastructure, allowing workloads to run consistently across on-premises, cloud, or hybrid environments.

## Benefits of Kubernetes

- **Scalability**: Kubernetes makes scaling applications easy, whether it's handling seasonal spikes or steady growth.
- **Self-Healing**: Kubernetes automatically replaces or reschedules containers that fail or become unresponsive.
- **Automated Rollouts and Rollbacks**: Kubernetes simplifies updates and rollbacks for applications, reducing downtime.
- **Resource Optimization**: Kubernetes dynamically manages resources based on application needs, leading to more efficient use of infrastructure.
- **Community and Ecosystem Support**: With a strong community, Kubernetes has numerous integrations and extensions, making it suitable for complex, evolving needs.

## Success Stories

Many organizations have achieved significant operational improvements and cost savings by using Kubernetes:

- **Airbnb** improved deployment speed and reduced engineering complexity by migrating to Kubernetes, enabling a more agile infrastructure.
- **Spotify** uses Kubernetes to manage its complex, microservice-based architecture, ensuring high availability and rapid iteration.
- **Pinterest** leverages Kubernetes for rapid scaling, enabling its platform to handle billions of pins and searches with ease.

## Kubernetes Architecture

Kubernetes architecture comprises several key components, categorized into the **control plane** and **data plane**. Here’s a high-level diagram:

### Diagram

- **Control Plane**: Manages the cluster, overseeing scheduling, maintaining cluster state, and managing deployments.
- **Data Plane**: Executes applications by running and managing containers on worker nodes.

### Components

1. **API Server**: Exposes Kubernetes API, the front end for the control plane.
2. **etcd**: A key-value store that maintains cluster data and configurations.
3. **Controller Manager**: Manages various controllers, monitoring cluster state and taking corrective actions.
4. **Scheduler**: Assigns new pods to nodes based on resource availability and policies.
5. **Worker Nodes**: Host the pods that run the application containers.

## The Cluster Setup

A **Kubernetes cluster** consists of multiple nodes (computers or VMs) working together. Clusters typically have:

- **Master Nodes**: Control plane components run here, managing the overall cluster.
- **Worker Nodes**: Run application workloads in containers.

### Master vs. Worker Nodes

- **Master Nodes** coordinate the entire cluster, maintaining the desired state, and controlling node configurations.
- **Worker Nodes** are responsible for running containers, managed by the kubelet, a local agent on each node.

## Pros and Cons of Managed Service vs. Self-Managed Kubernetes

| Managed Kubernetes Services (e.g., GKE, EKS)         | Self-Managed Kubernetes Cluster                    |
|------------------------------------------------------|---------------------------------------------------|
| **Pros**: Simplified setup, managed upgrades, automatic scaling | **Pros**: Greater control, customizability, cost efficiency |
| **Cons**: Limited customization, higher costs         | **Cons**: Requires in-depth expertise, higher maintenance overhead|

## Control Plane vs. Data Plane

- **Control Plane**: Manages and maintains the cluster state and decides where to run applications.
- **Data Plane**: Executes applications and performs workloads as directed by the control plane.

## Kubernetes Objects

Kubernetes objects are abstractions that represent the desired state of your cluster. Here are some of the most common ones:

- **Pod**: The smallest deployable unit in Kubernetes, usually containing one or more containers.
- **ReplicaSet**: Ensures a specified number of pod replicas are running.
- **Deployment**: Manages ReplicaSets, allowing declarative updates for pods.
- **Service**: Exposes an application running on a set of pods to the network.
- **ConfigMap**: Stores configuration data, enabling applications to be configured without code changes.
- **Secret**: Stores sensitive data like passwords and tokens securely.

### Ephemeral Nature of Pods

A pod is **ephemeral**, meaning it’s temporary and can be terminated or recreated at any time. This transient nature ensures flexibility and reliability, but applications need to be designed to handle such changes.

## Mitigating Security Concerns with Containers

Containers introduce unique security challenges, as they share resources on the host machine. Strategies for mitigating risks include:

- **Use Role-Based Access Control (RBAC)** to restrict access within the Kubernetes cluster.
- **Enable Network Policies** to isolate pods and control communication within the cluster.
- **Regularly update images and Kubernetes** to protect against vulnerabilities.
- **Use container security scanning** to detect vulnerabilities in images before deployment.

## Maintained Images

**Maintained images** are container images provided by reputable sources (e.g., Docker Hub official images) and regularly updated to address security vulnerabilities.

### Pros and Cons of Maintained Images

| Pros                                                  | Cons                                                        |
|-------------------------------------------------------|-------------------------------------------------------------|
| **Pros**: Security patches, stability, community support | **Cons**: Limited flexibility, dependency on external updates|

## What are the biggest challenges facing enterprises?

- Vendor lock-in.
  - Enterprises often become dependent on a single vendor for products and services, making it difficult and costly to switch providers.
  - Organisations can get stuck with their data and how its structure because theyve chosen to use a managed service.
  - Vendors will store this data in a particular format, if they wat to change this, it's a huge challenge because it's stored in a particular way.
- Cost.
- Keeping Data Secure.
  - They may want to move their data to the cloud, but they're worried about keeping it secure.
  - They want to physically controll access to where their hardware is stored.
  - They may not want to migrate the entire application onto the cloud because they may want to keep parts of those applications on-prem.
    - This results in a **hybrid** cloud solution.
    - More complicated and requires more expertise.

## How do these organisations handle Kubernetes?

- If they want to use Kubernetes, they have to abstract (seperate) the complexities of Kubernets and match them with certain developers.
- Challenges: higher complexities, more expertise, dedicated kubernetes team.

## Why/when would a business NOT want to use a microservices architecture?

- slow/low-runtime.
- Small, simple application: fine to use a monolith.
- Early stages of development.
- A business is not ready with having expertise available to handle the complexity of a microservices architecture.
- Some organisations have a cult culture to develop monoliths.
- An application that needs to use every part of the database.
  - The challenge of breaking this aplpication up might not be worth the change. Too big a challenge.

The main reason that makes one architecture more successful than the other: the culture of the organisation.
