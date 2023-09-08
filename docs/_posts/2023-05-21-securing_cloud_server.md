---
layout: post
title: "Secure your cloud server"
date: 2023-05-21 00:00:00 +0000
categories: linux
---

One of the best practices to secure a Linux cloud server is to avoid using the root account
for day-to-day operations.
If you've just created a new server in AWS or on another cloud platform,
you'll need to create a new username and avoid connecting to the server using the root account.

Let's do this step by step.

Assume you've just created a new Linux server. This guide is based on Debian or Ubuntu systems.
However, for other distributions, most of the commands will be more or less the same.

## Step 1: Check the current users

Connect to the server using SSH:

```bash
$ ssh root@<ip_address>
```

To determine which users are members of the `root` and `sudo` groups, inspect the
[/etc/group](https://www.cyberciti.biz/faq/understanding-etcgroup-file/) file:

```bash
# cat /etc/group | grep "sudo\|root"
```

After executing the above command, you might see an output similar to:

```bash
root:x:0:
sudo:x:27:
```

This output indicates that there are currently no users assigned to the `sudo` or `root` groups.

You can check which users have SSH login capabilities by examining the
[/etc/passwd](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/) file.

```bash
# cat /etc/passwd | grep /bin/
```

Executing this command may produce an output like:

```bash
root:xxxxxxxxxxxxxx:/bin/bash
sync:xxxxxxxxxxxxxx:/bin/sync
admin:xxxxxxxxxxxx:/bin/bash
```

This indicates that the users `root`, `sync`, and `ubuntu` have access to commands like `bash` or `sync`.

To remove all users except `root`:

```bash
# userdel -r admin
# userdel -r sync
```

The `-r` flag deletes the user along with their home directory.
While it's not strictly necessary to remove the `sync` user, we will.

## Step 2: Add a New User

Let's proceed to add a new user and grant it sudo execution privileges.

First, ensure that `sudo` is installed:

```bash
# apt update
# apt install sudo
```

Now, create a new user named `pactus` (feel free to choose a different name):

```bash
# adduser pactus
```

Then, add this user to the `sudo` group:

```bash
# adduser pactus sudo
```

To confirm everything is set up correctly, use the
[id](https://www.cyberciti.biz/faq/unix-linux-id-command-examples-usage-syntax/) command:

```bash
# id pactus
```

Running `# cat /etc/group | grep "sudo\|root"` again should now display
the new user as a member of the `sudo` group.

## Step 3: Enable SSH Login for New User

Now, we'll set up the server to allow connections from the new user.

First, switch the security context to the new account to ensure new folders and files have the appropriate permissions:

```bash
# sudo su - pactus
```

Create the `~/.ssh` directory:

```bash
$ mkdir ~/.ssh
$ chmod 700 ~/.ssh
```

Currently, we are logged into the server using the `root` account.
The public key for SSH access is stored in the `/root/.ssh/authorized_keys` file.
We will copy this file to `~/.ssh/authorized_keys`, change its ownership to the new user,
and then delete the original file.
This will prevent SSH login as `root`.

```bash
$ sudo cp /root/.ssh/authorized_keys ~/.ssh
$ sudo chown pactus:pactus ~/.ssh/authorized_keys
$ sudo rm /root/.ssh/authorized_keys
```

Now, let's ensure that the new user can use SSH to connect to the server.
Open a new terminal on your local machine and run:

```bash
$ ssh pactus@<ip address>
```

## Step 4: Disable SSH login for root

At this point, we can disable SSH remote login for the root account.

Open `sshd_config`:

```bash
$ sudo vim /etc/ssh/sshd_config
```

Search for the authentication options and modify the root login permission by setting it to 'no' as shown below:

```text
PermitRootLogin no
```

Also, ensure that SSH password login is disabled:

```text
PasswordAuthentication no
```

After making these changes, restart the `sshd` service:

```bash
$ sudo systemctl restart sshd
```

Now, you will no longer be able to log in using the root account.
