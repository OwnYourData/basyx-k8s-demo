# BaSyx AAS Deployment Demo on Kubernetes

This repository demonstrates how to deploy an **Asset Administration Shell (AAS)** on Kubernetes using the [Eclipse BaSyx](https://www.eclipse.org/basyx/) platform.  
It guides you through generating a custom BaSyx configuration, applying the manifests in a Kubernetes cluster, and interacting with the resulting services.  
The example configuration deploys an AAS Environment, Registries, a Dashboard API, and a Web UI — all accessible under  
👉 **https://basyx.data-container.net/**

---

## 🧭 Introduction

### What is the Asset Administration Shell (AAS)?
The **Asset Administration Shell** is the standardized digital representation of an asset — a *digital twin* as defined in the context of **Industry 4.0**.  
It provides a structured way to describe all relevant information, capabilities, and relationships of an asset in a machine-readable, interoperable format.

The AAS is a cornerstone of the [Industrial Digital Twin Association (IDTA)](https://industrialdigitaltwin.org/en/) approach and supports interoperability across systems by using standardized submodels.

### What is Eclipse BaSyx?
[Eclipse BaSyx](https://www.eclipse.org/basyx/) is an open-source middleware for developing and deploying Asset Administration Shells.  
It provides ready-to-use microservices for:
- AAS registries and environments  
- Submodel repositories  
- Dashboards and UIs  
- Connectors for various storage backends (e.g., MongoDB)

BaSyx helps bridge the gap between assets on the shop floor and their digital twins in IT systems.

---

## 🚀 Step 1 – Generate your configuration

1. Go to the official BaSyx configuration generator:  
   👉 [https://basyx.org/get-started/introduction](https://basyx.org/get-started/introduction)

2. Select the components you want to deploy.  
   For this tutorial, the following setup was chosen:
   - **AAS Environment**
   - **AAS Registry**
   - **Submodel Registry**
   - **Dashboard API**
   - **MongoDB**
   - **Web UI**

3. Click **“Generate Configuration”** and download the resulting ZIP file.  
   This archive contains all configuration files required to run BaSyx locally via Docker. For convenience, the configuration used in this tutorial is available for [download here](https://github.com/OwnYourData/basyx-k8s-demo/raw/main/basyx-setup.zip).

---

## ⚙️ Step 2 – Inspect the configuration

BaSyx’s official download only provides a **Docker Compose** setup (from https://basyx.org/get-started/download).  
This tutorial provides an example of how to deploy the same setup on a **Kubernetes** cluster.

Unzip the provided Kubernetes package [`basyx-k8s-manifests.zip`](./basyx-k8s-manifests.zip) and inspect its contents (each file defines a part of the deployment):  

Each file defines a part of the deployment:
- **00-namespace.yaml** → creates the `basyx` namespace  
- **10-configmaps.yaml** → configuration for all services  
- **20-secret.yaml** → credentials for MongoDB (edit before deploying!)  
- **30-storage.yaml** → persistent volume claims  
- **40-deployments.yaml** → deployments for MongoDB, registries, and services  
- **50-services.yaml** → internal cluster services  
- **60-webui-and-apis-ingress-cert.yaml** → ingress & TLS certificate  

> Note: The Docker Compose ZIP from the BaSyx website is great for local trials.  
> The `basyx-k8s-manifests.zip` in this repo is a curated, Kubernetes-ready counterpart (same logical components, cluster-native wiring).

---

## ☸️ Step 3 – Deploy to Kubernetes

### Prerequisites
- A running Kubernetes cluster (v1.24+)
- `kubectl` configured with your cluster context
- `cert-manager` and `nginx-ingress` installed
- A valid DNS entry pointing to your cluster (here: `basyx.data-container.net`)

### Apply manifests
```bash
kubectl apply -f 00-namespace.yaml
kubectl apply -f 10-configmaps.yaml
kubectl apply -f 20-secret.yaml
kubectl apply -f 30-storage.yaml
kubectl apply -f 40-deployments.yaml
kubectl apply -f 50-services.yaml
kubectl apply -f 60-webui-and-apis-ingress-cert.yaml
```

Check that all pods are running:
`kubectl get pods -n basyx`

⸻

## 🔐 Step 4 – Verify the deployment

Once the pods are ready, open:
* Web UI: https://basyx.data-container.net/
* AAS Environment: https://basyx.data-container.net/aas-env/swagger-ui/index.html
* AAS Registry: https://basyx.data-container.net/registry/swagger-ui/index.html
* Submodel Registry: https://basyx.data-container.net/sm-registry
* Dashboard API: https://basyx.data-container.net/dashboard/swagger-ui/index.html

You should see the BaSyx Web UI interface and can explore or register Asset Administration Shells.

⸻

## 🏁 Step 5 – Result: Using the AAS
You now have a fully functional BaSyx AAS stack deployed on Kubernetes —
demonstrating how standardized digital twins can be hosted, managed, and integrated in an interoperable way.

➡️ Example live deployment: https://basyx.data-container.net/

1.  Access the Web UI via your browser.
2.  Create a new AAS or Submodel directly from the dashboard.
3.  Use the REST API endpoints for automation or external integrations.    
    Example:
    ```
    curl -s https://basyx.data-container.net/aas-env/api/v3/AASDescriptors
    ```

4.  Connect your AAS instances to physical or simulated assets.

⸻

## About  

Supported in the course of the [PACE-DPP project](https://dpp-austria.at/) by the Austrian Federal Ministry for Climate Action, Environment, Energy, Mobility, Innovation and Technology (BMK), supported by the Austrian Research Promotion Agency (FFG funded project #917177), as well as from the German Federal Ministry for Economic Affairs and Climate Action (BMWK), supported by the German Research Promotion Agency (DLR-PT).<img align="left" src="https://raw.githubusercontent.com/OwnYourData/basyx-k8s-demo/main/res/BMIMI_Logo_srgb.png" height="150">

<br clear="both" />


© 2025 OwnYourData – Licensed under the Appache 2.0 License.
