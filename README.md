# LigaAC Docker / Kubernets

## Install Git/Docker

Download Git : https://git-scm.com/downloads 
Download Docker : https://www.docker.com/products/docker-desktop/


git --version
docker --version

### Install/Configure WSL 2

- https://learn.microsoft.com/en-us/windows/wsl/install
- https://docs.microsoft.com/en-us/windows/wsl/install-win10

***PowerShell:***
```
wsl -l -v
wsl --set-version ubuntu 2
wsl --set-default-version 2
wsl --set-default Ubuntu
```


## Install your Linux distribution of choice from Microsoft store

- Ubuntu 16.04 LTS
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS
- openSUSE Leap 15.1
- SUSE Linux Enterprise Server 12 SP5
- SUSE Linux Enterprise Server 15 SP1
- Kali Linux
- Debian GNU/Linux
- Fedora Remix for WSL
- Pengwin
- Pengwin Enterprise
- Alpine WSL

### Get Code

```powershell
git clone https://github.dev/Fredy-SSA/LigaAC-DK
```

- https://docs.docker.com/get-started/

## Create DockerHub Repositories

<<<<<<< HEAD
- Download a web temlate "/src"
- Use Dockerfile  
=======
- Download a web template "\src" 

## Create Kubernets Cluster in Azure
>>>>>>> a3bb76268ad31cef5badbd86769a560d15424e50

1 - Build Docker Image

```powershell
docker image build -t webapp .

docker image ls
```
2 - Rename Image tag

```powershell
docker image tag webapp fredysa/webligaac:latest
docker image ls

```

3 - Test Image/Container

```powershell

docker container run -it --rm -p 8080:80  -d --name webligaac  fredysa/webligaac

```
4 - Publish to Dockerhub 

```powershell

docker login

```
```powershell

docker push fredysa/webligaac

```

## Create Azure Repositories 


Install Azure CLI  https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli or use WebShell
```
az --version

az account list-locations

```

Create Resource Group
```
az group create --name LigaAc-rg --location eastus
```

Create Azure container registry (ACR)
```
az acr create --resource-group LigaAc-rg --name ligaac --sku Basic
```
Login to Azure in local AZ Cli
```
az login
```
Login to ACR in your local ACR

```
az acr login --name ligaac
```

Check ACR Instance :  

```
 az acr list --resource-group LigaAc-rg --query "[].{acrLoginServer:loginServer}"  --output table
```

Prepare your local docker image to be push to Azure ACR:
```
docker image tag fredysa/webligaac:latest ligaac.azurecr.io/webligaac:latest
```

Push to Azure ACR:
```
docker image push ligaac.azurecr.io/webligaac:latest
```

Check output
```
az acr repository list --name ligaac --output table
```
## Create Kubernets AKS  in Azure

Create cluster / wait 
```
az aks create  --resource-group LigaAc-rg --name LigaAc-rg-cluster --node-count 2 --generate-ssh-keys --attach-acr ligaac
```

Instal Kubernets cli
```
az aks install-cli
```

Login to Kubernets CLI
```
az aks get-credentials --resource-group LigaAc-rg --name LigaAc-rg-cluster
```
Test Kubernets cli

```
kubectl get nodes
```

Deploying our application to the Kubernetes cluster
```
kubectl apply -f webligaac.yaml
```

```
 kubectl get service webligaac --watch
 
 ```

