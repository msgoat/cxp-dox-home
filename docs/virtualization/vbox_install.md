# Installing VirtualBox on Windows 10

## Prerequisites

### Make sure Hyper-V is deactivated

Although you will be able to install VirtualBox while Hyper-V is activated, you will not be able to run VMs with VirtualBox (well VirtualBox is supposed to have Hyper-V support since version 6 but only with serious problems).

See [Uninstalling Hyper-V using Powershell](installing_hyperv.md) for instructions to deactivate Hyper-V.

## Install VirtualBox using the installer

Installing VirtualBox is pretty straightforward. Just download the installer from the official [VirtualBox Download Page](https://www.virtualbox.org/wiki/Downloads) (Section __VirtualBox Binaries > Link "Windows Hosts"__ ) and run it. Simply follow the instructions of the installation wizard and you should be fine.

## Uninstall VirtualBox using Windows Settings

If you want or need to get rid of VirtualBox, simply open __Windows Settings > Apps > Apps & Features__. Select app __Oracle VM VirtualBox__ and push button __Uninstall__.