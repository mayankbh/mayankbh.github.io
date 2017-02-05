---
layout: post
title: "SSHing into a VirtualBox VM, because why not?"
category: technology
date: 2017-02-05
---

Now, I know VirtualBox provides a nice little GUI where you can use the terminal or GUI directly. Unfortunately, with Linux on a HiDPI system, my resolution goes all kind of whack, and all I get is a screen the size of a GameBoy Advance (ah, memories).

## Setting up Port Forwarding on the VM

Credit goes to the top answer on http://stackoverflow.com/questions/5906441/how-to-ssh-to-a-virtualbox-guest-externally-through-a-host

In the Machine Settings for the VM, enter the 'Network' section, move the the first (I used this one, I'm not entirely sure if it works with other adapters too), and assuming it's in NAT (again, this is what worked for me, can't remark on anything else). Click on the 'Port Forwarding' button and add a new rule, which forwards traffic from any host port of your choice to Guest port 22.

Alternatively, VirtualBox's command line can also be used to setup this rule. Simply type -

VBoxManage modifyvm myserver --natpf1 "ssh,tcp,,3022,,22"

Where you could replace 3022 with any port of your choice. Make sure the rule was added by running -

VBoxManage showvminfo myserver | grep 'Rule'


## Install and start the SSH server on your VM

I like using openssh-server for this, but the package name may vary based on your distribution. Unfortunately, you'll have to use VirtualBox's GUI for this part, but life is full of disappointments.

## SSH in

ssh -p \<host port\> username@127.0.0.1

Should now let you SSH in, in all your native terminal glory.

## I need GUI interface using Visual Basic to track the killer's IP!

I guess one option would be to setup a VNC server.

You could always pass the -X argument to SSH and run individual GUI applications (say, gedit) from the command line, but I'm not familiar enough with the X server to comment on how to directly connect to the X server over SSH (I'm so unsure, I'm not even sure if I've phrased the problem correctly)
