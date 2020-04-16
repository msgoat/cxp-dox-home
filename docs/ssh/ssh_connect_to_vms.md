# Connecting to Linux VMs on Windows 10

## Introduction 

On Windows 10, there are actually a lot of ways to connect to your Hyper-V Linux VM (some of them even do not require to install additional software!):

* Windows Console
* PuTTY

## Windows Console

Windows 10 includes all tools (like __ssh__) you need to connect to a remote Linux system via SSH.

1\. Open a Windows console

2\. Enter __ssh ${userId}@${remoteHost}__ to login to the Linux VM. If you connect for the first time, acknowledge the host's authenticity and enter password of the given user:
```
>ssh root@172.18.46.76
The authenticity of host '172.18.46.76 (172.18.46.76)' can't be established.
ECDSA key fingerprint is SHA256:OQS9DwjvLJrqiwG/cUpqRRPyjNp4zvHR3bATEejgMx0.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '172.18.46.76' (ECDSA) to the list of known hosts.
root@172.18.46.76's password:
Last login: Thu Apr  2 11:55:36 2020
```

## PuTTY

The most common way to connect to Linux systems on Windows is probably __PuTTY__. 
PuTTY is an SSH and telnet client, developed originally by Simon Tatham for the Windows platform. PuTTY is open source software that is available with source code and is developed and supported by a group of volunteers.
 
Well there's a lot to know about PuTTY so all information has been moved to [PuTTY Primer](putty_primer.md) in section __SSH__.
All you need to know about connecting to VMs with PuTTY can be found at [Managing SSH Connections with PuTTY](putty_managing_connections.md) in section __SSH__