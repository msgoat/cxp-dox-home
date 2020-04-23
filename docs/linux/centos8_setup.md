# Setting up CentOS after installation

## Preface

!!! important "Public Cloud Provider VMs come with a non-root user"
    On VMs provided by public cloud providers like AWS or Azure login with user __root__ is prohibited. But we got lucky: 
    these machines already come with a non-root user: on AWS this will be user __centos__ for CentOS VMs, user 
    __ec2-user__ for Amazon Linux VMs and user __ubuntu__ for Ubuntu VMs. Nevertheless you should be familiar with 
    adding a new user to your Linux system. Just remember to add __sudo__ as a prefix to all commands executed by a root user.

!!! note "Always replace placeholders in command before you run them"
    Most commands and templates in this article will contain placeholders (like ${userId} representing your user ID).
    Remember to replace these placeholders with appropriate values before you run the commands. 

## Before you start

1\. Make sure the text editor of your choice is installed on your system.

The authors preferred text editor is [nano](https://www.nano-editor.org/dist/v2.2/nano.html). You can install nano using the following command:

```shell
sudo yum install nano
```

## Create a non-root user

!!! info "Always try to avoid usage of user root"
    The __root__ user is the administrative user of a Linux system and has very broad privileges. Because of the power
    that comes with these privileges it is very easy to perform very destructive changes, even by accident. Thus, you are
    *strongly* discouraged from using it on a regular base. 
     
First setup of our post-installation setup will be to add a new non-root user to our Linux system.

1\. Login to your machine with the existing user at hand (most likely __root__).

2\. Create a new user with __${userId}__ (replace ${userId} with your AWS IAM user Id):

```shell
adduser ${userId}
```

3\. Next, set a password for your newly created user 
(use a *strong* password with at least 10 chars using uppercase and lowercase letter plus digits):

```shell
passwd ${userId}
```

## Grant administrative privileges to the non-root user

Since you are going to use this non-root user to administer your Linux system you will have to grant administrative 
privileges to the newly created user. Thus, by prefixing administrative commands requiring __root__ privileges with __sudo__
we are able to run this particular command in the context of user __root__.

On CentOS based Linux systems we grant administrative privileges by adding the newly created non-root user to group __wheel__.

1\. Add user ${userId} to group __wheel__:

```shell
usermod -aG wheel ${userId}
```

2\. To test if the non-root user is authorized to run privileged commands, switch to the non-root user:

```shell
su - ${userId}
```

3\. Run a privileged command: 

```shell
sudo yum update
```

You will be asked to enter the password of the non-root user which is OK for now, but not OK if you want to perform
sudo commands in shell script.

!!! note "Configuration files for sudoers"
    The permissions of sudo-capable users or *sudoers* are kept in a configuration file located at __/etc/sudoers__. 
    Although we could directly edit this file (which is actually recommended in most tutorials), it is better to add 
    a new file in the extension folder __/etc/sudoers.d__. All files in this extension folder will be included by the
    default /etc/sudoers configuration file.

4\. Create a text file named __100-non-root-users__ in folder __/etc/sudoers.d__ with the following content (using sudo):

```
# allow non-root user ${userId} to run sudo commands without password
${userId} ALL=(ALL) NOPASSWD:ALL
```

5\. Now if you run a privileged command again, you should not be asked for a password anymore: 

```shell
sudo yum update
```

6\. Switch back to the initial user you are using to setup your Linux system.

```shell
exit
```

## Create SSH keys for the non-root user

Next, we are going to create SSH keys for our newly created non-root user. Please follow all instructions at 
[Creating SSH keys using ssh-keygen](../ssh/ssh_create_ssh_keys.md) to create the SSH keys. 

## Disabling Password Authentication on your Server

After you have successfully activated SSH login with a non-root user on your Linux system, it is a good idea to 
disable logins using passwords at all.

1\. Open file __/etc/ssh/sshd_config__ using your preferred text editor (here: nano):

```shell
sudo nano /etc/ssh/sshd_config
```

2\. Locate a line starting with __PasswordAuthentication__ and make sure it is set to __no__:

```text
PasswordAuthentication no
```

3\. Close the text editor.

4\. If you made changes to the file, you will have to the sshd service:

```shell
sudo systemctl restart sshd
```

Once you have verified your SSH service is still working properly, you can safely close all current server sessions.

The SSH daemon on your CentOS server now only responds to SSH keys. Password-based authentication has successfully been disabled.

## Configure your firewall using firewalld

!!! tip "Prefer cloud-provider-defined firewalls over firewalld, if you are in a public cloud"
    Usually, inbound and outbound traffic on Linux systems is controlled via the __firewalld__ service. On a regular
    Linux system running on premise or in a "cloud" environment without cloud-provider-defined firewalls, a proper
    firewall setup using __firewalld__ is a *MUST*!!!. On AWS or Azure however, cloud-provider-defined firewalls are available
    and should be preferred over __firewalld__ for a simple reason: you can change firewall rules without having to 
    login to your Linux machine and they are effective immediately.
    
1\. Check if service __firewalld__ is enabled on your Linux machine:

```shell
sudo systemctl status firewalld
```

If the answer is `Unit firewalld.service could not be found` you're done and you can skip the next step.

2\. If service __firewalld__ is active, it's safe to switch it off by entering the following commands (since we are working 
in public cloud with cloud-provider-defined firewalls):

```shell
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

## Setup Security Enhanced Linux using selinux

__Security Enhanced Linux__ or __SELinux__ is an advanced access control mechanism built into most modern Linux 
distributions. It was initially developed by the US National Security Agency to protect computer systems from malicious 
intrusion and tampering. Over time, SELinux was released in the public domain and various distributions (like CentOS)
have since incorporated it in their code. 

!!! note "CentOS or Amazon Linux comes with selinux"
    On CentOS or Amazon Linux all packages required for selinux are already installed. On CentOS the default setting
    for selinux is __permissive__ (access violations are logged but no not enforced), on Amazon Linux the default setting is
    __disabled__.

If you want to dig deeper into SELinux, there's an excellent series of articles provided by DigitalOcean: 
[An Introduction to SELinux on CentOS 7](https://www.digitalocean.com/community/tutorial_series/an-introduction-to-selinux-on-centos-7)

To get familiar with SELinux, we are going switch to SELinux mode __Permissive__ on our Linux machines.

1\. Check the status of SELinux by running:

```shell
getenforce
```
If the current status is __Permissive__, you are done and you can skip all following steps up to the next section.

2\. To set SELinux mode __Permissive__ permanently, you have to open file __/etc/sysconfig/selinux__:

```shell
sudo nano /etc/sysconfig/selinux
```

3\. Make sure the line starting with __SELINUX__ looks like this:

```TEXT
SELINUX=permissive
```

4\. Reboot your machine to apply the changes you made to the SELinux configuration.

```shell
sudo reboot
```

## What's next?





