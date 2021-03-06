# Installing Hyper-V on Windows 10

## Prerequisites

### Make sure you are running at least Window 10 1809

In order to make Hyper V work on Windows 10 you will have to run at least Windows 10 1809.
Open a simple Windows console and enter the command `winver`. You should see the following dialog `About Windows`:

![](img/hyperv_check_windows_version.png)

If the Windows version is `1809` or above, you are safe to continue. If not continue with Oracle Virtual Box.
 
### If you have Virtual Box installed, make sure no virtual machines are running
Although it is possible to install Hyper-V and VirtualBox on the same Windows machine, it is NOT possible to run Hyper-V VMs and VirtualBox VMs at the same time. Before you continue, you have to make sure, that no VirtualBox VMs are running when you install Hyper-V.

## Installing Hyper-V using Powershell

1. Open a PowerShell console as Administrator.
 
2. Run the following command: 
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All  
```

3. When the installation has completed, reboot.

Other ways to install Hyper-V are documented at [Install Hyper-V on Windows 10](https://docs.microsoft.com/de-de/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)

## Uninstalling Hyper-V using Powershell

If you need to switch back to __VirtualBox__, you need to uninstall Hyper-V first.

1\. Open a PowerShell console as Administrator.
 
2\. Run the following command: 

```
Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor 
```

3\. When the deinstallation has completed, reboot.

## Using Hyper-V Manager to administrate Hyper-V

Hyper-V comes with Hyper-V Manager which provides an UI based access to Hyper-V. So for all you PowerShell haters out there, here's the way to attach Hyper-V Manager to your Start menu:
 
1\. Enter Hyper-V into the search box in the Windows task bar:

![](img/hyperv_pin_to_start_0.png)

2\. Select Hyper-V Manager from the left hand match list

3\. Select Pin to Start from the right hand menu. Now you should find the Hyper-V Manager icon on the Start menu:

![](img/hyperv_pin_to_start_1.png)
