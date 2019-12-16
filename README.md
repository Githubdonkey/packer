# packer
Packer builds

# build machine
subscription id
*	az account show --query "{ subscription_id: id }"
tenant id
*	az account list
object id, this is only needed for Windows
*	get-AzureRmRoleAssignment


setup azure packer
* create resource group
* New-AzResourceGroup -Name "myResourceGroup" -Location "East US"

Setup linux machine

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

az login

* $sp = New-AzADServicePrincipal -DisplayName "PackerServicePrincipal"
* $BSTR = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($sp.Secret)
* $plainPassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($BSTR)
* $plainPassword:
* $sp.ApplicationId:
* az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID

list images
* https://docs.microsoft.com/en-us/azure/virtual-machines/linux/cli-ps-findimage
* az vm image list --output table
* az vm image list --offer Debian --all --output table
* az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
* az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
* az vm image show --location westus --urn Canonical:UbuntuServer:18.04-LTS:latest

#logging
(UNIX)
* export PACKER_LOG_PATH="/var/log/packer.log"
* export PACKER_LOG=10
* packer build -debug ubuntu_64.json
(WINDOWS)
* set PACKER_LOG=10
* set PACKER_LOG_PATH=c:\temp\packer log
* packer build -debug ubuntu_64.json
