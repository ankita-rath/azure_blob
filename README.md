Steps to Implement the Project

1️⃣ Prerequisites
	•	Azure Subscription (Free Tier works)
	•	Terraform Installed (Download Here)
	•	Azure CLI Installed (Download Here)
	•	VS Code or any code editor
 2️⃣ Authenticate Terraform with Azure

Run the following command to log in to your Azure account:
az login

Get your subscription ID:
az account show --query id --output tsv

3️⃣ Create Terraform Configuration Files

🔹 Create a new folder and initialize Terraform:
mkdir terraform-vm-automation
cd terraform-vm-automation

terraform init

🔹 Define the Terraform configuration file (main.tf)

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "terraform-vm-rg"
  location = "East US"
}

resource "azurerm_virtual_network" "vnet" {
  name                = "terraform-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
}

resource "azurerm_subnet" "subnet" {
  name                 = "terraform-subnet"
  resource_group_name  = azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.0.1.0/24"]
}

resource "azurerm_network_interface" "nic" {
  name                = "terraform-nic"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.subnet.id
    private_ip_address_allocation = "Dynamic"
  }
}

resource "azurerm_virtual_machine" "vm" {
  name                  = "terraform-vm"
  location              = azurerm_resource_group.rg.location
  resource_group_name   = azurerm_resource_group.rg.name
  network_interface_ids = [azurerm_network_interface.nic.id]
  vm_size               = "Standard_B1s"

  storage_os_disk {
    name              = "terraform-os-disk"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "18.04-LTS"
    version   = "latest"
  }

  os_profile {
    computer_name  = "terraformvm"
    admin_username = "azureuser"
    admin_password = "Terraform@123"
  }

  os_profile_linux_config {
    disable_password_authentication = false
  }
}

4️⃣ Deploy the VM Using Terraform

🔹 Initialize Terraform
terraform init

🔹 Validate Configuration
terraform validate

🔹 Preview Execution Plan
terraform plan

🔹 Apply Configuration to Deploy the VM
terraform apply -auto-approve

5️⃣ Verify the Deployment
	•	Go to Azure Portal → Resource Groups → terraform-vm-rg → Virtual Machines
	•	You should see the Ubuntu VM running
 6️⃣ Set Up Azure Monitor for VM Tracking
	•	Navigate to the VM in Azure Portal
	•	Go to Monitoring → Insights
	•	Enable Azure Monitor for CPU, Memory, Disk, and Network performance tracking
 7️⃣ Destroy the VM (Optional)

If you want to delete the setup, run:
terraform destroy -auto-approve



---------------------------------------------------------------------------------------------------------

Project: Virtual Machine Automation with Terraform
	•	Automated Azure VM provisioning using Terraform, eliminating manual setup efforts.
	•	Configured Azure Monitor to track VM performance in real time.
	•	Ensured efficient cloud infrastructure management using Infrastructure as Code (IaC) principles.

This project demonstrates real-world cloud automation skills using Terraform & Azure, making it a strong addition to your resume.


 
