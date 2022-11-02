# Azure Kubernetes Service
## Morten la Cour
## mlc@integration-it.com


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

.... Coming up

```powershell

```