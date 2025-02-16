# Azure DevOps Course tasks results (Practice #6)

## Task 1
**Terraform folder and files structure aligned; dependencies installed and Azure connected**
**Setup script taken from MS Learn stuff**
```bash
#!/bin/bash

RESOURCE_GROUP_NAME=Markiianxxxxx
STORAGE_ACCOUNT_NAME=tfstate$RANDOM
CONTAINER_NAME=tfstate

# Create resource group
az group create --name $RESOURCE_GROUP_NAME --location eastus

# Create storage account
az storage account create --resource-group $RESOURCE_GROUP_NAME --name $STORAGE_ACCOUNT_NAME --sku Standard_LRS --encryption-services blob

# Create blob container
az storage container create --name $CONTAINER_NAME --account-name $STORAGE_ACCOUNT_NAME
```
![Screenshot](screenshots_task1/tfstate.png)
![Screenshot](screenshots_task1/terraform_installed.png)

## Task 2
**Terraform code for deploying resources specified in task description**
```bash
resource "azurerm_virtual_network" "practice6-vnet" {
  name                = "practice6-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = data.azurerm_resource_group.practice6-rg.location
  resource_group_name = data.azurerm_resource_group.practice6-rg.name
}

resource "azurerm_subnet" "practice6-subnet" {
  name                 = "practice6-subnet"
  resource_group_name  = data.azurerm_resource_group.practice6-rg.name
  virtual_network_name = azurerm_virtual_network.practice6-vnet.name
  address_prefixes     = ["10.0.1.0/24"]
}

resource "azurerm_network_security_group" "practice6-nsg" {
  name                = "practice6-nsg"
  location            = data.azurerm_resource_group.practice6-rg.location
  resource_group_name = data.azurerm_resource_group.practice6-rg.name

  security_rule {
    name                       = "Allow-SSH"
    priority                   = 1001
    direction                  = "Inbound"
    access                     = "Allow"
    protocol                   = "Tcp"
    source_port_range          = "*"
    destination_port_range     = "22"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }

  security_rule {
    name                       = "Allow-HTTP"
    priority                   = 1002
    direction                  = "Inbound"
    access                     = "Allow"
    protocol                   = "Tcp"
    source_port_range          = "*"
    destination_port_range     = "80"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }
}

resource "azurerm_network_interface" "practice6-nic" {
  name                = "practice6-nic"
  location            = data.azurerm_resource_group.practice6-rg.location
  resource_group_name = data.azurerm_resource_group.practice6-rg.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.practice6-subnet.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id          = azurerm_public_ip.practice6-ip.id
  }
}

resource "azurerm_public_ip" "practice6-ip" {
  name                = "practice6-ip"
  location            = data.azurerm_resource_group.practice6-rg.location
  resource_group_name = data.azurerm_resource_group.practice6-rg.name
  allocation_method   = "Static"
  sku                 = "Basic"
}

resource "azurerm_network_interface_security_group_association" "practice6-association" {
  network_interface_id      = azurerm_network_interface.practice6-nic.id
  network_security_group_id = azurerm_network_security_group.practice6-nsg.id
}

resource "azurerm_linux_virtual_machine" "practice6-vm" {
  name                  = "practice6-vm"
  resource_group_name   = data.azurerm_resource_group.practice6-rg.name
  location              = data.azurerm_resource_group.practice6-rg.location
  size                  = "Standard_B1s"
  admin_username        = "azureuser"
  network_interface_ids = [azurerm_network_interface.practice6-nic.id]
  admin_ssh_key {
    username   = "azureuser"
    public_key = file("../practice2-3_terraform/id_rsa_practice2.pub")
  }

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
    disk_size_gb         = 30
  }

  source_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "18.04-LTS"
    version   = "latest"
  }
}

resource "null_resource" "provisioner" {
  depends_on = [azurerm_linux_virtual_machine.practice6-vm]

  provisioner "remote-exec" {
    inline = [
      "sudo apt update -y",
      "sudo apt install nginx -y"
    ]

    connection {
      type        = "ssh"
      user        = "azureuser"
      private_key = file("../practice2-3_terraform/id_rsa_practice2")
      host        = azurerm_public_ip.practice6-ip.ip_address
    }
  }
}
```

**Terraform files structure in VS Code**
![Screenshot](screenshots_task2/files_structure.png)

**terraform apply command output and nginx website browsing**
![Screenshot](screenshots_task2/tf-output.png)
![Screenshot](screenshots_task2/nginx-vm.png)

## Task 3


