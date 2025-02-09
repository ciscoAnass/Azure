
# Create VM using Terraform

### 1- Create Terraform VM File

Let's Create the Work Folder

```bash
mkdir Terraform-Vm
cd Terraform-Vm
```

Let's Create the Terraform File 

```bash
touch main.tf
touch provider.tf
```

Now Let's put some content to our Files : 

## provider.tf
```bash
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~>3.0"
    }
  }
}

provider "azurerm" {
  features {}
}
```



## main.tf
```bash
# Define the resource group
resource "azurerm_resource_group" "example" {
  name     = "anass-resources"
  location = "Spain Central"
}

# Create a public IP address
resource "azurerm_public_ip" "example" {
  name                = "anass-public-ip"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  allocation_method   = "Dynamic"
}

# Define the Ubuntu Virtual Machine
resource "azurerm_virtual_machine" "example" {
  name                  = "anass-ubuntu-vm"
  location              = azurerm_resource_group.example.location
  resource_group_name   = azurerm_resource_group.example.name
  vm_size              = "Standard_B1ls"  # Smallest cost-effective size
  network_interface_ids = [azurerm_network_interface.example.id]

  storage_os_disk {
    name              = "myosdisk1"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    disk_size_gb      = 30  # Minimum size for Ubuntu 18.04 LTS
    managed_disk_type = "Standard_LRS"
  }

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "18.04-LTS"
    version   = "latest"
  }

  os_profile {
    computer_name  = "hostname"
    admin_username = "anass"
    admin_password = "yourpassword123!"  # Secure password
  }

  os_profile_linux_config {
    disable_password_authentication = false
  }
}

# Define the network interface with public IP
resource "azurerm_network_interface" "example" {
  name                = "anass-nic"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.example.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id          = azurerm_public_ip.example.id  # Associate the public IP
  }
}

# Define a virtual network and subnet
resource "azurerm_virtual_network" "example" {
  name                = "anass-vnet"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  address_space       = ["10.0.0.0/16"]
}

resource "azurerm_subnet" "example" {
  name                 = "anass-subnet"
  resource_group_name  = azurerm_resource_group.example.name
  virtual_network_name = azurerm_virtual_network.example.name
  address_prefixes     = ["10.0.1.0/24"]
}

# Add a Network Security Group to allow SSH access
resource "azurerm_network_security_group" "example" {
  name                = "anass-nsg"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  security_rule {
    name                  s     = "SSH"
    priority                   = 1001
    direction                  = "Inbound"
    access                     = "Allow"
    protocol                   = "Tcp"
    source_port_range         = "*"
    destination_port_range    = "22"
    source_address_prefix     = "*"
    destination_address_prefix = "*"
  }
}

# Associate the NSG with the network interface
resource "azurerm_network_interface_security_group_association" "example" {
  network_interface_id      = azurerm_network_interface.example.id
  network_security_group_id = azurerm_network_security_group.example.id
}
```



### 2- Initialize Terraform

```bash
terraform init
```

### 3- Plan the Deployment
```bash
terraform plan
```

### 4- Apply the Configuration
```bash
terraform apply
```

### 5- Access Your Ubuntu VM
```bash
terraform refresh
terraform output
```

### 6- Connect to your Machine 

```bash
ssh azureuser@<public-ip>
```