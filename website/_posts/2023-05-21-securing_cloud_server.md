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

**Step 1: Check the current users**

Connect to the server using SSH:

```bash
ssh root@<ip_address>
```

Check if any pre-defined user has root access by reading the
[/etc/group](https://www.cyberciti.biz/faq/understanding-etcgroup-file/) file:

```bash
cat /etc/group | grep "sudo\|root"
```

You can also check which users can log in using SSH by reading the
[/etc/passwd](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/) file

```
cat /etc/passwd | grep /bin/
```

For example, in my instance, there is a pre-defined user with root access named `ubuntu`
Now, let's delete it first..

```bash
userdel -r ubuntu
```

The `-r` flag forces the removal of the user’s home directory.
You can also investigate other users of which you might not be aware.

**Step 2: Adding new user**

Now, let's add a new user and give it sudo execution privileges.

First, let's ensure that `sudo` is installed:

```bash
apt update
apt install sudo
```

Now, let's create the new user named `pactus` (you may choose a different name):

```bash
adduser pactus
```

Next, add the user to the `sudo` group:

```bash
adduser pactus sudo
```

Finally, ensure everything is set correctly using the
[id](https://www.cyberciti.biz/faq/unix-linux-id-command-examples-usage-syntax/) command:

```bash
id pactus
```

**Step 3: Enable SSH login for new user**

Now, we're going to connect to the server with the new user.

First, change the security context to the new account so
that the new folders and files will have the correct permissions:

```bash
sudo su - pactus
```

Create `~/.ssh` folder and authorized_keys:

```bash
mkdir ~/.ssh
chmod 700 .ssh
touch .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
```

Next, you'll need to add the public key to the `authorized_keys` file.
You can use a new public key or the current SSH key that you're using for the root account.

Finally, verify that the new user can use SSH to connect to the server.
Run this command from your local computer:

```bash
ssh pactus@<ip address>
```

**Step 4: Disable SSH login for root**

At this point, we can disable SSH remote login for the root account.

Open `sshd_config`:

```bash
sudo vim /etc/ssh/sshd_config
```

Search for the authentication options and modify the root login permission by setting it to 'no' as shown below:

```bash
PermitRootLogin no
```

Also, ensure that SSH password login is disabled:

```bash
PasswordAuthentication no
```

After making these changes, restart the `sshd` service:

```
sudo systemctl restart sshd
```

Now, you will no longer be able to log in using the root account
