---
layout: post
title: Tipps and Tricks for AIX
categories:
- aix
tags: 
- aix
- server admin
- tricks
---

Some of the most important tricks that saved me a lot of time. This
post is more a reference for me and might be extended over time.

#### Regenerate network interfaces

when I use _mktcpip_ I regularly see the message:

> The status of "xxx" Interface in the current running system is uncertain.

Whatever the underlying problem is (some ideas? please comment below!)
I found a simple workaround for this: delete the network interfaces
and have _cfgmgr_ recreate them.  {% highlight console linenos %}
lsdev -F name | grep ent | xargs -i -t rmdev -dl {} -R rmdev -dl inet0
-R cfgmgr {% endhighlight %}

Afterwards you can simply use _smit mktcpip_ to setup your network.

#### Cloning Logical Volumes in IBM Virtual I/O Server

IBM's VIO Server is one of the best virtualization environments. One
of the most important features in such a scenario is that I can easily
clone the virtual disks associated with my virtual machines. So I
simply install one instance of my AIX servers and copy as many as I
need.

The VIO Server comes with the _cplv_ command. But this takes forever
to clone your disk. So just use this sequence of commands to clone
_virt\_vol1_ to _virt\_vol1\_clone_: 
{% highlight console linenos %}
# Become root
oem_setup_env

# Use the raw virtual device to avopid buffering (speed!)
# set the block size to 8MB to increase speed
dd if=/dev/rvirt_vol1 if=/dev/rvirt_vol1_clone bs=8M
{% endhighlight %}

#### Starting and Shutting Down Partitions

Is rather simple in the IVM web interface. On the command line
I use:

{% highlight console linenos %}
# Shutdown using client OS
chsysstate -o osshutdown -r lpar --id <PartitionID>

# Shutdown immediately
chsysstate -o shutdown -r lpar --immed --id <PartitionID>

# Activate
chsysstate -o on -r lpar --id <PartitionID>

{% endhighlight %}

#### Grow volumes after install

After having installed a fresh AIX its volumes are way too small for
normal operation, e.g. 32 MB of disk space for all the users' home
directories. So you need to add some space to the mounted partitions.
Luckily this can easily be achieved on the fly:

{% highlight console linenos %}
# Become root
su -
# Add 1GB to the partition mounted on /home
chfs -a size=+1G /home
{% endhighlight %}

And there is no need to calculate 1024k or 512k blocks and MB or
GB. The _G_ and _M_ make AIX do this conversion for you!

#### Disclaimer

These are just some notes that are also useful for me, since I do not
have all these commandline options in my head.

A goof list of command for VIOS is here: <http://www.aixmind.com/?p=1904>
