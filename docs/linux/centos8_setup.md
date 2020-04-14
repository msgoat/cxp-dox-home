# Setting up CentOS 8 after installation

## Preface

!!! important "Instructions for CentOS on AWS VMs"
    If you are using CentOS 8 on AWS EC2 instance, some of the steps mentioned below are already done (like adding non-root user etc). So if you want to walk through this tutorial, you need to remember that you don't have access to user __root__. Simply execute all root commands with a __sudo__ prefix.

## Adding a non-root user, allowing HTTP traffic

There's an excellent tutorial about initial setup of CentOS provided by DigitalOcean: [Initial Server Setup with CentOS 8](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-8).
 
Please proceed through all steps of this tutorial to create a non-root user (replace the tutorials username __sammy__ with your own user ID).

## Creating SSH keys and adding them to your VM

To be able to login to our VM without using passwords, we need to create a SSH key pair of one private SSH key which remains on your machine and one public SSH key which you need to copy to the target machine.

Again, there's an excellent tutorial coming from DigitalOcean: [How To Set Up SSH Keys on CentOS 8](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-centos-8). Proceed through all steps. All SSH commands that you need to execute are available in any Windows console on your local Windows machine!

!!! danger ""
    When asked for a location where to store the key file, don't use the default location! If you do, you will probably overwrite a previously created private SSH key file. Replace the default filename __id_rsa__ with a more meaningful filename like `${virtualMachineName_rsa}`
    
Unfortunately, Windows 10 does provide the ssh-copy-idrequired to copy the public SSH key to the remote machine. So we need use SSH to copy the file to the VM (as described in section `Copying Public Key Using SSH` of the tutorial). Following command line worked for me:

```
type %USERPROFILE%\.ssh\${publicSSHKeyFileName} | ssh ${userName}@${ipAdress} "mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh && cat >> ~/.ssh/authorized_keys"
```    

Replace __publicSSHKeyFileName__, __userName__ and __ipAdress__ with proper values.