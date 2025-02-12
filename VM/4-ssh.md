# SSH on Azure

## How to enable the SSH port on VM using Azure Portal

![inboundrules](/img/vm/1/ssh/p1.png)

## How to enable the SSH port on VM using Azure CLI

 - To make everyone connect to your VM using ssh : 
```bash
az vm open-port --port 22 --resource-group anass-resources --name MyUbuntu
```
- To make a specefic network to connect to you VM using SSH : 

```bash
# Create new NSG
az network nsg create --resource-group anass-resources --name UbuntuRodrigoCaro-nsg
```

```bash
# Create the rule
az network nsg rule create \
  --resource-group anass-resources \
  --nsg-name UbuntuRodrigoCaro-nsg \
  --name AllowSSH \
  --protocol Tcp \
  --priority 100 \
  --destination-port-range 22 \
  --source-address-prefix 80.30.86.8 \
  --access Allow \
  --direction Inbound
```
## How to get Your Public Ip using Azure CLI

```bash
az vm show --show-details --resource-group anass-resources --name UbuntuRodrigoCaro --query publicIps -o tsv
```