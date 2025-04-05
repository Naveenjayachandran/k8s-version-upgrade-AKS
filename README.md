# k8s-version-upgrade-AKS

🚀 Upgrade Kubernetes Version in AKS
📌 Prerequisites
Azure CLI (az) installed and logged in

kubectl configured for the AKS cluster

Ensure the current version is supported for upgrade (check with az aks get-upgrades)

Review release notes before upgrading: https://learn.microsoft.com/en-us/azure/aks/release-notes

🔍 1. Check Available Kubernetes Versions
bash
Copy
Edit
az aks get-upgrades \
  --resource-group <RESOURCE_GROUP> \
  --name <CLUSTER_NAME> \
  --output table
This command shows:

Current Kubernetes version

Available versions to upgrade to

Node pool upgrade requirements

🛠️ 2. Upgrade the AKS Control Plane
To upgrade the control plane (master components):

bash
Copy
Edit
az aks upgrade \
  --resource-group <RESOURCE_GROUP> \
  --name <CLUSTER_NAME> \
  --kubernetes-version <NEW_VERSION> \
  --control-plane-only \
  --yes
--yes confirms the action without prompting.

🧱 3. Upgrade the Node Pools
Once the control plane is upgraded, proceed with each node pool:

bash
Copy
Edit
az aks nodepool upgrade \
  --resource-group <RESOURCE_GROUP> \
  --cluster-name <CLUSTER_NAME> \
  --name <NODEPOOL_NAME> \
  --kubernetes-version <NEW_VERSION> \
  --yes
To check all node pools:

bash
Copy
Edit
az aks nodepool list --resource-group <RESOURCE_GROUP> --cluster-name <CLUSTER_NAME> --output table
Repeat the upgrade for each node pool individually.

✅ 4. Verify the Upgrade
After the upgrade is complete, verify the versions:

bash
Copy
Edit
az aks show \
  --resource-group <RESOURCE_GROUP> \
  --name <CLUSTER_NAME> \
  --query kubernetesVersion \
  --output table
Check node version:

bash
Copy
Edit
kubectl get nodes -o wide
🚨 Notes
Upgrades are non-disruptive if workloads are properly distributed across multiple pods and nodes.

Consider using node auto-upgrade if you want AKS to manage upgrades:

bash
Copy
Edit
az aks nodepool update \
  --resource-group <RESOURCE_GROUP> \
  --cluster-name <CLUSTER_NAME> \
  --name <NODEPOOL_NAME> \
  --enable-auto-upgrade
📘 Example
bash
Copy
Edit
az aks upgrade \
  --resource-group prod-rg \
  --name prod-cluster \
  --kubernetes-version 1.29.2 \
  --yes

az aks nodepool upgrade \
  --resource-group prod-rg \
  --cluster-name prod-cluster \
  --name systempool \
  --kubernetes-version 1.29.2 \
  --yes
