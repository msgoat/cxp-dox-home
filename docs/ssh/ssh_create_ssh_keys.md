# Creating SSH keys using ssh-keygen

## Create a SSH key pair
 
1\. Switch to the context of the target user (if your are going to create SSH keys for another user on the same machine)
and move to the target users home directory:

```shell
$ su - ${userId}
```
 
2\. Create a directory called __.ssh__ in the home directory of the target user (if not present):

```shell
$ mkdir -p .ssh
```

2\. Create a SSH key pair using __ssh-keygen__:

```shell
$ ssh-keygen -t ecdsa -b 521
```

Simple press return when asked for file location but consider adding a passphrase to the private SSH key file.

!!! caution "Adding a passphrase to your SSH key file is *highly* recommended"
    It is highly recommended to add a passphrase to your SSH key file. Otherwise, anyone who has access to your private
    SSH key file can use it to login to all authorized Linux systems assuming your identity!!!

3\. Now the __.ssh__ should contain two files:

* __id_edcsa__ is the private SSH key file that you are going to use to login to a remote machine using SSH.
* __id_edcsa.pub__ is the public SSH key file that must be added to the remote machine you want to login to using SSH.

4\. Add the public SSH key file to the list of authorized SSH keys by running the following command:

```shell
$ cat .ssh/id_edcsa.pub >> .ssh/authorized_keys
```

5\. Change ownership of the directory __.ssh__ to target user and restrict access to the directory and its contents to target user only:

```shell
chmod -R go= .ssh 
```

6\. Copy the private SSH key file __.ssh/id_edcsa__ and the public SSH key file __.ssh/id_edcsa.pub__ to the machine 
you want to login from.

!!! danger "Always be careful with your SSH key pairs"
    Treat both SSH key files with extra care! If you lose your private SSH key file you will not be able to login
    to any authorized machine anymore. Remember: there's no reset function! If you lose your public SSH key file
    you will not be able to authorize your SSH login on other machines.     

!!! question "How do I copy these files?"
    The easiest way to copy the two files is using the clipboard. Simply open a text editor on the target machine, 
    display the contents of the first file with `cat .ssh/id_edcsa`, mark the console output with your mouse and paste the 
    selected content with the right mouse button. Save the text file. Repeat with the second file...
 
7\. If you do not plan to use this machine to login to other machines using SSH, __delete__ both SSH key files from directory __.ssh__:

```shell
rm .ssh/id_edcsa* 
```
 
## Check if SSH login works

Before we continue, we have to make sure that SSH login with the generated SSH key pair actually works. In this case, we
will use our Windows machines to login to the remote Linux system.

1\. Login to the source machine you want to login from.

2\. Make sure, that you have access to the private SSH key file on the machine you are logged in to. SSH key pair files 
are usually located in your personal folder at `~/.ssh` on Linux or `%USERPROFILE\.ssh` on Windows.

3\. Login to the target machine using `ssh` specifying the private SSH key file:

```shell
> ssh -i %USERPROFILE%\.ssh\${privateKeyFileName} ${userId}@${remotePublicIP} 
```

## What's next? 






