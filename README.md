Deploys 1+ Virtual Machines to your provided VNet
=================================================

This Terraform module deploys Virtual Machines in Azure with the following characteristics:

- Ability to specify a simple string to get the latest marketplace image using `var.vm_os_simple`
- All VMs use managed disks
- Network Security Group (NSG) created and only if `var.remote_port` specified, then remote access rule created and opens this port to all nics
- VM nics attached to a single virtual network subnet of your choice (new or existing) via `var.vnet_subnet_id`.
- Public IP is created and attached only to the first VM's nic.  Once into this VM, connection can be make to the other vms using the private ip on the VNet.


Usage
-----

Provisions 2 Ubuntu 14.04 Server VMs using  `vm_os_publisher`, `vm_os_offer` and `vm_os_sku` to a new VNet and opens up port 22 for SSH access with ~/.ssh/id_rsa.pub :

```hcl 
module "mycompute2" { 
    source              = "Azure/compute/azurerm"
    resource_group_name = "mycompute2"
    location            = "westus"
    public_ip_dns       = "myubuntuservers225"
    remote_port         = "22"
    nb_instances        = 2
    vm_os_publisher     = "Canonical"
    vm_os_offer         = "UbuntuServer"
    vm_os_sku           = "14.04.2-LTS"
    vnet_subnet_id      = "${module.network.vnet_subnets[0]}"
    tags                = {
                            environment = "dev"
                            costcenter  = "it"
                          }
}
  module "network" {
    source 		= "Azure/network/azurerm"
    location 		= "westus"
    resource_group_name = "mycompute2"
}

```
