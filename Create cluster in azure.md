Cluster in azure:
https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-portal?tabs=azure-cli

  102 curl -L https://aka.ms/InstallAzureCli | bash
  103  az aks install-cli
  104  az version
  105  az aks install-cli
  106  az login
  107  az account set --subscription c6a634ad-2fb9-4fb8-ba44-c38b37f19f12
  108  az aks get-credentials --resource-group mansi-network --name mansi-cluster --overwrite-existing
  109  kubectl get deployments --all-namespaces=true
  110  kubectl get deployments
  111  kubectl logs
  112  kubectl logs -h
  113  kubectl get nodes
