# Azure Kubernetes Service
## Morten la Cour
## mlc@integration-it.com


### Azure Passes


https://www.microsoftazurepass.com/







### Prereq

- Azure Subscription
- Azure CLI
- Kubernetes CLI (kubectl)

### Install through Chocolatey

Powershell in *Administrator* mode

```powershell

Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

```

May require a restart of PS

```powershell
choco install azure-cli -y
choco install kubernetes-cli -y

```

### Login to Azure Subscription

If logged in, in your **default browser**
```powershell

az login

```

If not

```powershell

az login --use-device-code

```

### Create an AKS Cluster



```powershell

## Create Resource Group

$rgName = "your_resource_group_name";
$location = "westeurope"; ## Soon to be "Denmark East"??

$rg = az group create --name $rgName --location $location;
$rg;

### Create AKS Cluster

$clusterName = "your_aks_cluster_name";

$cluster = az aks create --name $clusterName `
	--resource-group $rgName `
	--generate-ssh-keys `
	 --node-count 1 `
	 --no-wait
	 ;
$cluster;

```

### Connect kubectl to your Cluster

```powershell

az aks get-credentials --name $clusterName --resource-group $rgName

```

### Check your Context (Which Kubernetes Cluster are you pointing at)

```powershell

kubectl config get-contexts

```

### Create namespace

```powershell
$namespace = "sandbox";

kubectl create namespace $namespace;

```

### List all namespaces

```powershell

kubectl get namespaces

```