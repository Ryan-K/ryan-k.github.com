---
layout: post
title: "First Steps"
date: 2013-06-20 15:05
comments: true
categories:
---

Recently, I described [why development is best handled in a virtual environment][1]. I often hear complaints that its too difficult to setup and get running and that time spent learning something new takes away from valuable development time. So, this post will walk through the steps to go from nothing to a working VM running a very simple web service.

Installing VirtualBox and Vagrant
---------------------------------

The very first thing to do is to grab both [VirtualBox][] and [Vagrant][] and install them. They both have installers and should install like any other software on your platform of choice. Older versions of the software can have issues, so running the latest of both is pretty important for the most painless experience possible. Also, if you're working on a team with other developers, its often useful to ensure that everyone is working with the same versions of the software as compatibility issue can arise.

In the past, there was an alternative way to install Vagrant via a ruby gem. Thankfully, that's been eliminated as it has caused more confusion and conflicts and requires managing more of the ruby environment on your local workstation. That's something that I never want to have to deal with, ever.

Once those are installed, you'll find that there's a GUI interface for VirtualBox that will allow you to manage your VMs. The only time you'll need to go into there is if things get into a bad state and Vagrant gets very confused. Hopefully, that never happens but if you ever find yourself in that situation, firing up VirtualBox and powering down the VM should reset things. The main benefit of Vagrant is not having to deal with VirtualBox.

Starting your first VM
----------------------

To get started with your first Virtual Machine, all that you need to do is initialize a virtual machine, start it up and then you can connect to it. In Vagrant, this is done with a few simple commands which should be run from a empty directory where you'll be doing your work from.

```
$ vagrant init precise32 http://files.vagrantup.com/precise32.box
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
$ vagrant up
[default] Box 'precise32' was not found. Fetching box from specified URL for
the provider 'virtualbox'. Note that if the URL does not have
a box for this provider, you should interrupt Vagrant now and add
the box yourself. Otherwise Vagrant will attempt to download the
full box prior to discovering this error.
Downloading or copying the box...
Extracting box...
Successfully added box 'precise32' with provider 'virtualbox'!
[default] Importing base box 'precise32'...
[default] Matching MAC address for NAT networking...
[default] Setting the name of the VM...
[default] Clearing any previously set forwarded ports...
[default] Fixed port collision for 22 => 2222. Now on port 2200.
[default] Creating shared folders metadata...
[default] Clearing any previously set network interfaces...
[default] Preparing network interfaces based on configuration...
[default] Forwarding ports...
[default] -- 22 => 2200 (adapter 1)
[default] Booting VM...
[default] Waiting for VM to boot. This can take a few minutes.
[default] VM booted and ready for use!
[default] Configuring and enabling network interfaces...
[default] Mounting shared folders...
[default] -- /vagrant
$ vagrant ssh
```

The first line initializes a new VM, names it "precise32" and links it to the base box at that URL. There are many options for what to use for a base box. Precise is a recent LTS release of Ubuntu and is a decent choice. You can find many others at <http://www.vagrantbox.es/> or <http://cloud-images.ubuntu.com/vagrant> that you can substitute in place of the URL in the init command. All that really happens at this point is the creation of a "Vagrantfile" in the current directory which specifies everything about how your VM is configured. Feel free to look through it as common options are included and well documented.

The up command is what actually starts up your VM. Since this is the first time running the VM, it will download the VM image. Depending on your internet connection, this may take quite sometime. Once this completes, the process of actually starting the VM begins and vagrant works its magic. Primarily, this sets up network devices and mounts a shared directory. By default, this enables access of this current folder at "/vagrant" inside the virtual machine. This is how you'll be able to edit files locally on your desktop in your favorite editor, and inside the VM you'll see the changes reflected.

Finally, the ssh command will connect you to your brand new ubuntu VM. From here, you can install whatever software you'll need to run your server.

Configuring your VM
-------------------

The only change to the defaults I recommend when you're just starting, is to enable bridge networking by uncommenting this line in the auto-generated Vagrantfile by removing the # from the third line:

```
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network :private_network, ip: "192.168.33.10"
```

What this does is enable access to your VM at the specified IP address. That's 192.168.33.10 by default, so if you are running multiple VMs, you'll also want to give each one a unique IP address. This will allow you to connect to any services running on that box via that IP address. The other options are to forward ports on your local machine to the VM. The commented out example in the config would forward port 8080 on your local machine (the host) to port 80 on your VM (the guest). This would allow a webserver running in your VM to be accessed at http://localhost:8080/ in a web browser on your machine. The other option is to enable public networking, which extends the visibility of your VM outside of your machine. Generally, you won't need to do that.

Other Vagrant Commands
----------------------

Finally, there's a few other vagrant commands that are very useful. We have already seen vagrant init, vagrant up and vagrant ssh. When you are finished with this VM and want to release the resources its using, run vagrant suspend or vagrant halt. I prefer vagrant suspend as it preserves the state of the machine, rather then completely powering it off.

Remember that you'll have to exit your shell or run those commands from another terminal in the same directory as the vagrant commands do not work from within the VM. Vagrant is a tool that runs on your local machine and manages the VMs via VirtualBox. Inside the VM is a clean server environment that knows nothing about vagrant or that its running on your workstation.

Vagrant reload will reboot the VM, which is useful to allow changes in the Vagrantfile to take effect. Finally, vagrant destroy will remove this VM and the storage associated with it. Any changes you made to the system will be lost, but a vagrant up will recreate the VM and give you a clean machine again.

That's all there really is to it! In the next post, I'll introduce my favorite web framework, [Flask][], and get a very simple web service running within a new vagrant box.


[1]: http://ryan-k.github.io/blog/2013/05/29/virtually-the-only-way-to-go/
[VirtualBox]: https://www.virtualbox.org/wiki/Downloads
[Vagrant]: http://downloads.vagrantup.com
[Flask]: http://flask.pocoo.org/
