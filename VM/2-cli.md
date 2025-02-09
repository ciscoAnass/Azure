# Create VM using Azure CLI


## Prerequisites

- Make sure you have the Azure CLI installed on your Ubuntu machine.
- Log in to your Azure account using the CLI:

```bash
az login
```


## Create a VM in Azure Step by Step : 


### 1- Create a Resource Group

```bash
az group create --name anassresourcegroup --location westeurope
```
- You can replace the region with your preferred region ( [Regions List](https://github.com/Azure/azure-cli/issues/1520#issuecomment-493790517)  )


### 2- Create a virtual network

You need a virtual network (VNet) to place your VM. You can create it with the following command:

```bash
az network vnet create --resource-group anassresourcegroup --name ciscovnet --subnet-name ciscosubnet
```

`anassresourcegroup : ` the resource group that we did create previously
`ciscovnet : ` is the name of your virtual network (vnet) .
`ciscosubnet : ` is the name of your subnet.


### 3- Create a public IP address

To create a public IP : 
```bash
az network public-ip create --resource-group anassresourcegroup --name ciscopublicip
```



### 4- Create a network interface

```bash
az network nic create --resource-group anassresourcegroup --name ciscoNiC --vnet-name ciscovnet --subnet ciscosubnet --public-ip-address ciscopublicip
```





### 5- Create the virtual machine

```bash
az vm create \
  --resource-group anassresourcegroup \
  --name UbuntuAnass \
  --nics ciscoNiC \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --admin-username azureuser \
  --ssh-key-value /home/cisco24/.ssh/id_ed25519.pub
```

### 6- Check if the VM installed


```bash
az vm list --output table
```

- Also we can check if the VM is created in the Azure Platform
![Azure Platform](/img/vm/1/cli/1.png)




### 7- Let's Open the SSH port so we can connect to our VM

![Azure Platform](/img/vm/1/cli/2.png)


### 8- Let's CONNECT To our VM using SSH


![Azure Platform](/img/vm/1/cli/3.png)
