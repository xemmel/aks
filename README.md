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

### a simple hello world pod/service deployment

```powershell

### Create Namespace

kubectl create namespace ms-demo


### Create Deployment

kubectl create deployment deployment-ms-demo `
	--image mcr.microsoft.com/azuredocs/aci-helloworld `
	--replicas=2 `
	--namespace ms-demo `
	;
	
#### Get Yaml for deployment

kubectl get deployment.apps/deployment-mlc-temp -n mlc-temp -o yaml
	
### Create Service 

kubectl expose deployment deployment-ms-demo  `
	--type LoadBalancer `
	--namespace ms-demo `
	--name service-ms-demo `
	--port 80
	;

```

#### Commands

```powershell

### Everything in the namespace
kubectl get all -n ms-demo

### Describe the service (LB)
kubectl describe service/service-ms-demo -n ms-demo

### Get pods IP
kubectl get pods -n ms-demo -o wide


#### Scale

kubectl scale deployment `
	deployment-ms-demo `
	--namespace ms-demo `
	--replicas 10

```


### Elastic Search

```yaml

spec:
      containers:
      - image: elasticsearch:7.17.3
        name: elasticimage
        ports:
        - containerPort: 9200
        - containerPort: 9300
        env:
        - name: "discovery.type"
          value: "single-node"
Service

spec:
  selector:
    app: elastic
  type: LoadBalancer
  ports:
    - protocol: TCP
      name: 'h1'
      port: 9200
      targetPort: 9200
    - protocol: TCP
      name: 'h2'
      port: 9300

```