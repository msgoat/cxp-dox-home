# Azure Virtual Machine Basics

## Virtual Machine

@see [Linux virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/)

@see [Terraform azurerm_virtual_network](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/linux_virtual_machineg)

## Managed Disks

@see [Introduction to Azure managed disks](https://docs.microsoft.com/en-us/azure/virtual-machines/managed-disks-overview)

## Placement Group

@see [Co-locate resources for improved latency](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/co-location)

## Availability Set

@see [Availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/availability#availability-sets)

## Virtual Machine Scale Set

Autoscaling of virtual machines on Azure is done by using `virtual machine scale sets` (VMSS).

A virtual machine scale set may span multiple [availability zone](../az_get_started.md#availability-zones) in regions which support zones.
Additionally, a scale set can be configured to distribute the virtual machines across all availability zones.

The scaling behaviour of a virtual machine scale set is configured through monitoring scale settings.

@see [Vertical autoscale with virtual machine scale sets](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision)

@see [Overview of autoscale with Azure virtual machine scale sets](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview)

@see [Terraform azurerm_linux_virtual_machine_scale_set](https://www.terraform.io/docs/providers/azurerm/r/linux_virtual_machine_scale_set.html)

## Resources

[Linux virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/) \
Official documentation about Azure Linux Virtual Machines. Check sections `Overview` and `Concepts` first.
