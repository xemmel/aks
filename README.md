# Azure Kubernetes Service
## Morten la Cour
## mlc@integration-it.com


### Azure Passes


https://www.microsoftazurepass.com/


1
QF9M1FFU5J1YL370DR

2
QGF413OEREPK9FIZZC

3
QBMVI2NJHPZR4BZS28


4
QD35B5ULXXCBMQIMZ5


5
QDX247N2E96OMHF09N


6
Q13LDJQUOFDCN5YE7M


7
Q6XF9KGHD38J0Q1S0D


8
QN87BLDFM2XIF1MZIT

9
QG2S0PMKPYSG0UV47Z

10
QRW3R29BGVJ2TDTL0K

11
QJCW40IX8ZJN4XPBMP

12
Q5B2M0VVB8G6BMQ3SD

13
QJM7QP2TGPU60Q7H4F

14
QYWND7DR88FTJEL3JQ

15
QR5GREISWWIOMTNKSR

16
QYDKU3F8OCLJV04BF8

17
QWS7ELL95V1H1NSVT4

18
QCZW3MK82425E3ZDKL

19
QY49KSHG30PKRZRJCE

20
QE1LKPCZ6YE68Q8NF2





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