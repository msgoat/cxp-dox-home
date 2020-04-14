# Installing CentOS 8

## Preface

Before you start walking through this how-to I would like to confess that I am NOT a Linux expert. So if you diagree with the configuration options that I chose and which worked for me, please feel free to leave a comment - or, even better - fix this page right away. 

## Install CentOS 8 using the graphic installer

1\. Connect to your newly created VM which booted from the provided ISO file. First thing you should see is the boot menu:

![](img/centos8_install_0.png)

2\. Select option __Install CentOS Linux 8__ to start the installation process without checking the boot drive first.

![](img/centos8_install_1.png)

3\. Just stick to __English__ as language press Continue.

![](img/centos8_install_2.png)

4\. Select __Keyboard__ to add a german keyboard layout to the system.

!!! note "Add a german keyboard layout only if you have to!"
    Well it's completely up to you, if you're going to add a German keyboard layout. If you're happy with an English layout, just continue with step __9__!

![](img/centos8_install_3.png)
    
5\. Press button __+__ to add another keyboard layout.

![](img/centos8_install_4.png)

6\. Enter __German__ into the search box and select option __German__ from the result list. Push __Add__ .

![](img/centos8_install_5.png)

7\. Select layout __German__ and push button __^__ to move the German layout to the first position.

![](img/centos8_install_6.png)

8\. Press __Done__ to return to the main panel.

![](img/centos8_install_10.png)

9\. Select __Installation Destination__ to configure the partitions on the boot volume.

![](img/centos8_install_11.png)

10\. Select storage configuration option __Automatic__ (which is good enough for our test VM!). Press __Done__ to return to the main panel.

![](img/centos8_install_12.png)

11\. Select __Network & Host Name__ to setup network configuration.

![](img/centos8_install_13.png)

12\. Press button __Configure__ to start configuration of network interface eth0.

![](img/centos8_install_15.png)
 
13\. Select tab __General__ and check option __Connect automatically with priority__.

![](img/centos8_install_16.png)

14\. Select tab __Ethernet__ and set __Cloned MAC address__ to Preserve.

![](img/centos8_install_16a.png)
 
15\. Select tab __IPv4 Settings__ and make sure that __Method__ option __Automatic (DHCP)__ is set.

![](img/centos8_install_17.png)

16\. Select tab __IPv6 Settings__ and make sure that __Method__ option __Automatic__ is set. Press __Save__ to return.

![](img/centos8_install_18.png)

17\. Now, network interface eth0 is connected and already pulled an IP address from DHCP. Press __Done__ to return to the main panel.

![](img/centos8_install_19.png)

18\. Select __Time & Date__ to set the operating system time zone.

![](img/centos8_install_20.png)

19\. Select Region __Europe__  and City __Berlin__ (or any timezone you would like to use). Press __Done__ to return to the main panel.

![](img/centos8_install_21.png)

20\. Select __Software Selection__ to define which CentOS 8 variant we would like to install. Remember: we're building servers here so there`s no need to provide an UI!

![](img/centos8_install_22.png)

21\. Select __Minimal Install__ and press __Done__ to return to the main panel.

![](img/centos8_install_23.png)

22\. Now we`re DONE! Push __Begin Installation__ to continue.

![](img/centos8_install_24.png)

23\. While the operating system installation is running, we are going to select __Root Password__ to set the root user password.

![](img/centos8_install_25.png)

24\. Enter and confirm a *STRONG* root password. Press __Done__ to return to the main panel.

![](img/centos8_install_27.png)

25\. Wait until the installation process completes. Open the __Media__ menu and select __Media > DVD Drive > Eject__ to eject the virtual disk containing the ISO. Press __Reboot__ to restart the VM.

!!! important "Troubleshooting reboots"
    I was not able to reboot the VM successfully. The connection window just display a black screen. If you should encounter the same behaviour, simply shutdown the virtual machine manually and restart it.
    

