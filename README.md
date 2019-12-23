# packer
Packer builds

## Azure info
```
$sp = Get-AzureRmADServicePrincipal -DisplayNameBeginsWith Packer
Application ID	$sp.applicationId.guid
object ID	$sp.Id.ToString()
```
```
$sub = Get-AzureRmSubscription
Tenant ID	$sub.TenantId
Subscription ID	$sub.SubscriptionId
```
## Create Service Principal

###### CLI
```
$spc = New-AzADServicePrincipal -DisplayName "PackerServicePrincipal"
$BSTR = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($spc.Secret)
$plainPassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($BSTR)
$plainPassword:
$spc.ApplicationId:
```
az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID

###### Portal
https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal
```
All services > Active Directory > App registrations
```

## Logging
###### UNIX
* Build machine: export PACKER_LOG_PATH="/home/test/packer.log"
* Build machine: export PACKER_LOG=1
* Build machine: packer build -debug ubuntu_64.json

###### WINDOWS
* Build machine: set PACKER_LOG=1
* Build machine: set PACKER_LOG_PATH=c:\temp\packer log
* Build machine: packer build -debug ubuntu_64.json

## List images
* https://docs.microsoft.com/en-us/azure/virtual-machines/linux/cli-ps-findimage
```
az vm image list --output table
az vm image list --offer Debian --all --output table
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
az vm image show --location westus --urn Canonical:UbuntuServer:18.04-LTS:latest
```

setup azure packer
* create resource group
* New-AzResourceGroup -Name "myResourceGroup" -Location "East US"

## Setup linux machine
* sudo apt-get install unzip
* wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
* sudo dpkg -i packages-microsoft-prod.deb
* sudo apt-get update
* sudo apt-get install apt-transport-https
* sudo apt-get update
* sudo apt-get install dotnet-sdk-3.1
* sudo apt install curl
* curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
* sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
* sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
* sudo apt-get update
* sudo apt-get install azure-functions-core-tools
* curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
###### Packer - install or update(sudo rm -r /usr/local/bin/)
* VER=1.4.5
* wget https://releases.hashicorp.com/packer/${VER}/packer_${VER}_linux_amd64.zip
* unzip packer_${VER}_linux_amd64.zip
* sudo mv packer /usr/local/bin
###### Set Env Variables for build process
 ```
 export azure_app_id=
 export azure_client_secret=
 export azure_sub_id=
 export azure_tenant_id=
```
