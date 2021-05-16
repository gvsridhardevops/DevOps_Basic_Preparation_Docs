<-- [Back](/README.md#labs)

---

### **Lab #1** - Install software, and use Vagrant to deploy 3 training VMs

---

### Overview

Time to complete:  30 minutes

In the following lab you will install the following software:

1. **Git** (should already be installed) - the version control system
2. **VirtualBox** - the hypervisor used by default by Vagrant
3. **Vagrant** - the virtual machine deployment tool

...and then create 3 VirtualBox VMs named as follows:

1. **puppet**   (Puppet Master, PE Console, etc.)
3. **agent**    (A Puppet agent)
2. **gitlab**   (GitLab, which will also run the Puppet agent)

### Steps

* Download the needed software for this tutorial...

```
[puppet-tutorial-pe]$ cd share
[puppet-tutorial-pe/share]$ cd software
[puppet-tutorial-pe/share/software]$ ./download-all.sh
```

* Install VirtualBox
      - find the appropriate installer in the `puppet-tutorial-pe/share/software/virtualbox/` directory
      - Installers for Mac OS X and Windows are provided, but others can be downloaded as well
      - other installers available here: <https://www.virtualbox.org/wiki/Downloads>

* Start VirtualBox and configure 'Default Machine Folder' to place VM's in a directory other than the default (if you wish), then exit VirtualBox again.

* Install Vagrant
      - find the appropriate installer in `puppet-tutorial-pe/share/software/vagrant`
      - others available here: <https://www.vagrantup.com/downloads.html>

* cd into `puppet-tutorial-pe/`

* We install a Vagrant plugin to manage VirtualBox guest additions

* Then `vagrant up puppet` to provision the VM

```
cd puppet-tutorial-pe
vagrant plugin install vagrant-vbguest
vagrant up puppet
```

* once the **puppet** VM is provisioned, get familiar with vagrant, and confirm the VM specs are correct:

```
    vagrant ssh puppet              # to verify that you are able to login (should not be prompted for password)
    cat /etc/centos-release         # what CentOS release are we running?
    cat /proc/cpuinfo | grep proc   # how many vCPU's does the VM have? (should see 4 processors)
    lscpu                           # another way to see CPU info
    free -h                         # how much RAM is available on this VM?
    df -h /share                    # is our shared space mounted up? (should see 'share' filesystem mounted on /share)
    ls -al /share/software          # should see our puppet/ directory in there
    exit                            # and drop out of the VM, back to host OS shell prompt
```

### Stopping your Vagrant-managed VM

When you're done working through any particular lab, and want to stop your
VM's, you'd issue a `vagrant halt` command followed by the VM name, as follows:

```
    vagrant halt puppet        # and wait for VM to shutdown
```

### Starting your Vagrant-managed VM

If you issued a `vagrant halt puppet` to test the above, you may issue a `vagrant up puppet` before continuing on through the labs to start it back up again...

If all goes well with the above, we can be confident that our VM is working as expected, and let's move on.

### Login to your VM again...

Let's login to your puppet VM again with `vagrant ssh puppet`

You should see that `/share` is in the `df` output. This is your shared filesystem space that is also accessible outside of the VM

```
$ vagrant ssh puppet
Last login: Fri Oct 21 15:14:38 2016 from 10.0.2.2
[vagrant@puppet ~]$ df -h /share
Filesystem      Size  Used Avail Use% Mounted on
none            223G  206G   18G  93% /share
```

This `/share` directory is accessible **both** within your VM and from your host system.
For example, if you copy a file to `/share` within your VM, you will be able to get to it from your host OS at `puppet-tutorial-pe/share/` (and visa versa)

If for some reason, you do not see the `/share/` filesystem mounted up, try halting your VM and starting again.  Sometimes the VMware tools need to be updated, and a stop/start will trigger that:

```
[vagrant@puppet ~]$ exit
[puppet-tutorial-pe]$ vagrant halt puppet
[puppet-tutorial-pe]$ vagrant up puppet
[puppet-tutorial-pe]$ vagrant ssh puppet
```

...and check again.

### Explore more Vagrant commands

Get comfortable with Vagrant, and connecting to your VM.  The most useful commands you'll need are:

```
vagrant up <name>          # start the VM, and provision it if it's the first time
vagrant halt <name>        # shutdown the VM cleanly
vagrant global-status      # show the status of your VMs (powered on or off)
vagrant destroy <name>     # if you want to completely destroy your VM and start over
vagrant ssh <name>         # connect to your VM with ssh
vagrant ssh-config <name>  # show ssh config in case you want to use PuTTY to connect to your VM
```

### Provision two more VMs

You'll also need to provision 2 more VMs, one for **GitLab**, and one more
which will run the puppet **agent**.  (Actually, all 3 will run the puppet
agent, but we create a separate small VM for testing with so we don't have
to worry about breaking something while we are testing.)

```
vagrant up gitlab          # provision the VM for your GitLab server
vagrant up agent           # provision your VM that will be one of your puppet agents
```

Once you provision your **gitlab** and **agent** VMs, you should see them in the output of **vagrant global-status**

```
$ vagrant global-status
id       name         provider   state    directory
------------------------------------------------------------------------------
4dd5ed7  puppet       virtualbox running  /Users/Mark/Vagrant/puppet-tutorial-pe
070258c  gitlab       virtualbox running  /Users/Mark/Vagrant/puppet-tutorial-pe
682eafe  agent        virtualbox running  /Users/Mark/Vagrant/puppet-tutorial-pe

The above shows information about all known Vagrant environments
on this machine. This data is cached and may not be completely
up-to-date. To interact with any of the machines, you can go to
that directory and run Vagrant, or you can use the ID directly
with Vagrant commands from any directory. For example:
"vagrant destroy 1a2b3c4d"
```

### Validate your training environment

We've used Vagrant to provision 3 training VMs.  If you're curious how the
VM's are configured, take a peek at the [puppet-tutorial-pe/Vagrantfile](/Vagrantfile)

We've used a custom base box that already has some things we need (packages, config, etc.)
This custom base box already has entries in `/etc/hosts` that we want/need (since we will
rely on /etc/hosts to resolve names within our training environment.)

Each VM should have the following contnets in their `/etc/hosts` file:

```
127.0.0.1       localhost
192.168.198.10  puppet.example.com  puppet
192.168.198.11  agent.example.com   agent
192.168.198.12  gitlab.example.com  gitlab
```

With all 3 of your VMs up and running, validate that you can ping each using
the FQDN as well as the short name.

For example:

```
[vagrant@puppet ~]$ ping puppet.example.com
PING puppet.example.com (192.168.198.10) 56(84) bytes of data.
64 bytes from puppet.example.com (192.168.198.10): icmp_seq=1 ttl=64 time=0.043 ms
64 bytes from puppet.example.com (192.168.198.10): icmp_seq=2 ttl=64 time=0.098 ms
64 bytes from puppet.example.com (192.168.198.10): icmp_seq=3 ttl=64 time=0.075 ms
^C
--- puppet.example.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
rtt min/avg/max/mdev = 0.043/0.072/0.098/0.022 ms

[vagrant@puppet ~]$ ping puppet
PING puppet.example.com (192.168.198.10) 56(84) bytes of data.
64 bytes from puppet.example.com (192.168.198.10): icmp_seq=1 ttl=64 time=0.052 ms
64 bytes from puppet.example.com (192.168.198.10): icmp_seq=2 ttl=64 time=0.088 ms
64 bytes from puppet.example.com (192.168.198.10): icmp_seq=3 ttl=64 time=0.098 ms
^C
--- puppet.example.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
rtt min/avg/max/mdev = 0.052/0.079/0.098/0.021 ms
```

You should also be able to ssh between each VM as either the **vagrant** user
or the **root** user.  The default password is simply **"vagrant"**.  Give it
a try if you like.  You should also be able to ssh in to any of your VMs from
the host itself.  You can learn the ssh port to use using the `vagrant ssh-config`
command.  Give it a try:

```
[puppet-tutorial-pe]$ vagrant ssh-config puppet
Host puppet
  HostName 127.0.0.1
  User vagrant
  Port 22022
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/Mark/.vagrant.d/boxes/bentlema-VAGRANTSLASH-centos-7.2-64/1.0.0/virtualbox/vagrant_private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

Take note that the NAT'ed port is 22022.

```
[puppet-tutorial-pe]$ ssh root@127.0.0.1 -p 22022
Warning: Permanently added '[127.0.0.1]:22022' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
Last login: Fri Nov 11 21:57:06 2016
[root@puppet ~]# exit
logout
Connection to 127.0.0.1 closed.
```

This ability to ssh between our VMs will become useful in a later lab
(when we configure R10K to pull code from GitLab.)

### Next up...

This concludes Lab #1.  You should now have 3 VMs up and running named as
shown above in the **global-status** output above.


If you're not planning to continue to the next lab right away, you may want
to halt all of your training VM's.  Make sure you've exited the ssh session
of each VM, and back down to the shell prompt on your workstation, then issue
the `vagrant halt` for each VM as follows:

```
vagrant halt puppet
vagrant halt agent
vagrant halt gitlab
```

Note:  Doing a **halt** will gracefully shutdown your VM, not simply power
it off.  You do **not** need to shutdown the OS running in the VM first.  If you
do that, the **vagrant halt** wont be able to shutdown gracefully, will
timeout trying, and then power off the VM.

---

Continue on to **Lab #2** --> [Prepare to Install Puppet Enterprise](02-Prep-to-Install-Puppet-Master.md#lab-2)

---

<-- [Back to Contents](/README.md)

---

<-- [Back](01-Provision-Training-VMs.md#lab-1)

---

### **Lab #2** - Prepare to Install Puppet Enterprise on the **puppet** VM

---

### Overview

Time to complete:  10 minutes

In this lab we will complete some pre-installations steps to get our VM ready to take Puppet Enterprise

### Startup your Training VMs

If not already up and running...

```
vagrant up puppet
```

If you don't know the status of your vagrant-controlled VM's, you can always check with...

```
$ vagrant global-status
id       name         provider   state    directory
--------------------------------------------------------------------------------
4dd5ed7  puppet       virtualbox running  /Users/Mark/Vagrant/puppet-tutorial-pe
070258c  gitlab       virtualbox poweroff /Users/Mark/Vagrant/puppet-tutorial-pe
682eafe  agent        virtualbox poweroff /Users/Mark/Vagrant/puppet-tutorial-pe
```

Notice that my **puppet** VM is running, but my **gitlab** and **agent** VM's are powered off.

### Login/Connect to your Training VM

```
$ vagrant ssh puppet
```

Once you're connected to the **puppet** VM, notice the bash shell pompt looks something like this:

```
[vagrant@puppet ~]$
```

Become root...

```
[vagrant@puppet ~]$ sudo su -
```

...and notice your shell prompt changes to:

```
Last login: Fri Oct 21 15:37:10 UTC 2016 on pts/0
[root@puppet ~]#
```

Now exit your root shell, and drop back to the vagrant user's shell...

```
[root@puppet ~]# exit
logout
[vagrant@puppet ~]$
```

Throughout the labs, you will need to execute many (if not most) commands as root.

Using sudo is a good habbit to develop, but starting a root shell can be more
convenient as long as you remember that you can severly damage a running system if
typing in the wrong command in the wrong place. (e.g. what if you accidently copy
and paste some text into a root shell, and it contains some commands that remove
the entire /lib directory?)

I will use `sudo` to run commands as root while in the vagrant user's shell, as
well as starting a root shell with `sudo su -` to illistrate these two options
below...

    - Use sudo to edit the /etc/hosts file
    - Start a root shell, and configure the host firewall


### Pre-installation Steps

There are a couple things we need to do to make our VM ready to take PE:

    - Add some entries to the /etc/hosts file ( if not already there )
    - Open some ports through the host firewall

### Edit /etc/hosts

The `/etc/hosts` file should already be setup correctly, but if for some reason
you find it is not, go ahead and edit it as follows.

Edit `/etc/hosts` and add entries for localhost, as well as our 3 training VMs, both
long (FQDN) and short name.

    Note: For the purposes of this training, we will not use DNS.  We will rely
          on the /etc/hosts file for name resolution.  In a production deployment
          you would almost certainly want to use fully-qualified domain names (FQDN's),
          and very likely DNS as well.

```
sudo vi /etc/hosts
```

We want our `/etc/hosts` file to look like this:

```
127.0.0.1      localhost
192.168.198.10 puppet.example.com puppet
192.168.198.11 agent.example.com  agent
192.168.198.12 gitlab.example.com gitlab
```

In a later lab, we will configure Puppet to manage these /etc/hosts entries for us.

**Configure Firewall**

Open the host firewall to allow Puppet to work as per [PE Install Guide - Firewall Config](https://docs.puppet.com/pe/latest/sys_req_sysconfig.html#for-monolithic-installs)

```shell
sudo su -
firewall-cmd --permanent --add-service=https   # PE Console (default port 443)
firewall-cmd --permanent --add-port=3000/tcp   # PE web-based installer
firewall-cmd --permanent --add-port=8080/tcp   # PuppetDB
firewall-cmd --permanent --add-port=8081/tcp   # PuppetDB
firewall-cmd --permanent --add-port=8140/tcp   # Puppet Master, Certificate Authority
firewall-cmd --permanent --add-port=8142/tcp   # Orchestration services
firewall-cmd --permanent --add-port=8143/tcp   # The Orchestrator client uses this port to communicate with the orchestration services
firewall-cmd --permanent --add-port=61613/tcp  # MCollective / ActiveMQ
firewall-cmd --permanent --add-port=4432/tcp   # Local connections for node classifier, activity service, and RBAC status checks
firewall-cmd --permanent --add-port=4433/tcp   # Classifier / Console Services API endpoint
firewall-cmd --permanent --add-port=8170/tcp   # Code Manager
firewall-cmd --reload
firewall-cmd --list-all
exit # drop out of root shell
```

The output of your `firewall-cmd --list-all` should look like this:

```
[vagrant@puppet ~]$ firewall-cmd --list-all
public (default, active)
  interfaces: enp0s3 enp0s8
  sources:
  services: dhcpv6-client https ssh
  ports: 3000/tcp 8140/tcp 8170/tcp 8080/tcp 4433/tcp 8081/tcp 4432/tcp 8143/tcp 8142/tcp 61613/tcp
  masquerade: no
  forward-ports:
  icmp-blocks:
  rich rules:
```

Okay, now we're ready to run the Puppet Enterprise Installer...

---

Continue to **Lab #3** --> [Install Puppet Master](03-Install-Puppet-Master.md#lab-3)

---

<-- [Back to Contents](/README.md)

---

<-- [Back](02-Prep-to-Install-Puppet-Master.md#lab-2)

---

### **Lab #3** - Install Puppet Enterprise

---

### Overview

Time to complete:  45 minutes

In this lab we will install Puppet Enterprise 2016.5.1

* PE is free to install and evaluate
* When running PE without a license, you're limited to 10 agents


### Get logged in to your puppet VM

We are about to install Puppet Enterprise, and make sure you're
logged into our puppet master host.

### Note about root

When connecting to a Vagrant-provisioned VM with **vagrant ssh** you will be
logged in as the **vagrant** user (a normal/unprivileged user).  However, we
need root privileges when installing Puppet Enterprise.

In order to simplify the commands we run in the following sections, I will assume
you know how to use sudo, and when to use it.  All puppet commands need to be run
as root, so the easiet thing to do would just to become root.  We're in a training
environment after all, and if you mess something up, you can just delete it and
start over.

So, if you're already root, Great!

If you're not the root user yet, then become root, and be happy!

```
     sudo su -
```

### Run The Installer

Now let's un-compress/de-archive the PE installation tarball, and install...

Change into the directory with the PE software:

```shell
     cd /share/software/puppet
     tar xzvf puppet-enterprise-2016.5.1-el-7-x86_64.tar.gz
     cd puppet-enterprise-2016.5.1-el-7-x86_64
```

Then run the installer:

```
     ./puppet-enterprise-installer
```

The installer will prompt you:


```
=============================================================
    Puppet Enterprise Installer
=============================================================
Puppet Enterprise offers two different methods of installation.

 [1] Guided install

Recommended for beginners. This method will install and configure a temporary
webserver to walk you through the various configuration options.

NOTE: This method requires you to be able to access port 3000 on this machine
from your desktop web browser.

 [2] Text-mode

Recommended for advanced users. This method will open your $EDITOR (vi)
with a PE config file (pe.conf) for you to edit before you proceed with installation.

The pe.conf file is a HOCON formatted file that declares parameters and values needed to
install and configure PE.
We recommend that you review it carefully before proceeding.

=============================================================


 How to proceed? [1]:
```

Accept the default `[1]` (just press Return/Enter).

Some packages will be installed, and then the installer will prompt you do
open a page in your web browser...

```
## We're preparing the Web Installer...

2016-11-14 17:39:20,482 Running command: mkdir -p /opt/puppetlabs/puppet/share/installer/installer
2016-11-14 17:39:20,487 Running command: cp -pR /share/software/puppet/puppet-enterprise-2016.5.1-el-7-x86_64/* /opt/puppetlabs/puppet/share/installer/installer/

## Go to https://puppet.example.com:3000 in your browser to continue installation.

## Be sure to use 'https://' and that port 3000 is reachable through the firewall.
```

Remember that we have **forwarded port 3000 to 22000** on our workstation, so...

* Use web browser to connect to: **<https://127.0.0.1:22000/>**
* Click **Let's Get Started**
* Click **Monolithic Install**
* For FQDN, enter **puppet.example.com**
* For DNS alias, enter **puppet**
* Select: Install PostgreSQL on the PuppetDB host for me.
* Enter an initial password for the admin user (we will change it later)
* Click **Submit**
* Click **Continue**

Note:  Don't worry about the memory and disk space warnings.  The amount
of memory and disk space we've provisioned will be just fine for this training
exercise.  Obviously, when deploying for a real production environment, you'd
want to pay attention to these!

You will see this:

```
     We're checking to make sure the installation will work correctly
     Verify local environment.
     Verify that 127.0.0.1 can resolve puppet.example.com.
     Verify root access on puppet.example.com.
     Verify that DNS is properly configured for puppet.example.com.
     Verify that your hardware meets requirements on puppet.example.com.
     [puppet.example.com] We found 2,847 MB RAM. We recommend at least 4096 MB.
     Verify that 127.0.0.1 has a PE installer that matches puppet.example.com's OS.
     Verify that '/opt', '/var', and '/tmp' contain enough free space on puppet.example.com.
     [puppet.example.com] Insufficient space in '/opt' (16 GB); we recommend at least 100 GB for a production environment.
```

Click **Deploy Now**

```
     Intalling your deployment
     Install Puppet Enterprise on puppet.
     Verify that Puppet Enterprise is functioning on puppet.
     [puppet] The puppet agent ran successfully.
     Verify that MCollective is functioning on puppet.
     Backup installer log files to puppet.
```

During the installation process you may click on 'Log view' to see what is
happening behind the scenes, and then click 'Summary View' to return back to
the overview.

![PE Install Finished](images/PE-Install-Finished.png)

Note:  Once the installation completes, clicking the 'Start Using Puppet
Enterprise' button will **not** work, as we are port-forwarding from a
VM to our localhost.  Use the link below instead.

---

### Login to the PE Console

We've forwarded port 443 from our puppet VM to port 22443 on our hosting workstation, so you should be able to connect to the PE Console via the URL:

* PE Console URL:  **<https://127.0.0.1:22443/>**

![PE Console Login](images/PE-Console-Login.png)

Login as **admin** and enter the admin password you chose during the install.

If you forgot (or not sure what you typed) you can find the password in the answers file:

```
     grep console_admin_password /opt/puppetlabs/puppet/share/installer/conf.d/puppet.example.com.conf
```

Probabbly a good idea to **change it**...!  (You can change the admin password via the PE Console Web-Interface)

### Log files

All log files for Puppet Enterprise and its components can be found in:  `/var/log/puppetlabs/`

If you would like to look at the install logs, they are available within `/var/log/puppetlabs/installer/`

---

Believe it or not, that's all there is to installing a **Monolithic** puppet
server.

Although we wont cover the **Split** install, nor the **Large Environment Install**
in this tutorial, it may be helpful to understand what they are:

### What is a Split / LEI Install ?

A Split install would consist of 3 hosts (or VMs) with the various components split across them as follows:

* Host 1
    - Puppet Master (Compile Master)
    - Certificate Authority (CA)
    - ActiveMQ Broker (used by MCollective)
* Host 2
    - Puppet Enterprise Console
* Host 3
    - Puppet DB
    - PostgreSQL Database (backing for PuppetDB)

A **Large Environment Install** takes it a step further and scales out the ActiveMQ infrastructure used
by MCollective, and more importantly, the **Puppet Compile Masters**.  In this example we've added multiple
ActiveMQ Spokes and multiple Compile Masters (which the Puppet agents would point to).   Only the PE
infrastructure hosts would point to the MoM for their configuration.  Additional Compile Masters and
ActiveMQ Spoke Brokers could be added to scale out.

* Host 1
    - Puppet Master of Masters (MoM)
    - Certificate Authority (CA)
    - ActiveMQ Spoke Broker (used by MCollective)
* Host 2
    - Puppet Enterprise Console
* Host 3
    - Puppet DB
    - PostgreSQL Database (backing for PuppetDB)
* Host 4
    - ActiveMQ Hub Broker
* Host 5
    - ActiveMQ Spoke Broker
* Host 6
    - ActiveMQ Spoke Broker
* Host 7
    - Puppet Compile Master (load balanced)
* Host 8
    - Puppet Compile Master (load balanced)
* Host 9
    - Puppet Compile Master (load balanced)

A **Split** and/or **Large Environment Install** is a bit more work, as we'd be
splitting up the various parts of PE on to separate VM's, as well as deploying
multiple compile masters behind a loadbalancer.  If you're interested
in learning more about a split install, you may read about it in the
[PE Installation Guide](https://docs.puppet.com/pe/2016.5/install_pe_mono.html).
Here are the relevant pages:

* [Split Installation](https://docs.puppetlabs.com/pe/2016.5/install_pe_split.html)
* [Scale Compile Masters](https://docs.puppet.com/pe/2016.5/install_multimaster.html)
* [Scale ActiveMQ](https://docs.puppet.com/pe/2016.5/install_add_activemq.html)

Okay, let's get back to the lab...

---

### Back to the PE Console...

Let's play around in the PE Console a bit.  To change the 'admin' account
password, click on 'admin' in the top right corner, and select 'My Account'.
You should find a 'Reset password' link near the top/right of that page.

Look around the PE console.  Try clicking on the **Events**, **Nodes**,
and **Classification** tabs to see what you can see.

You should see 1 agent is registered called
**puppet.example.com**.  This is your Puppet Master!


### Test your Puppet installation...

Test your PE installation by running the agent manually from the shell prompt like this:

```
     puppet agent -t
```

Note:  Even on the Puppet Master, the Agent runs regularly in the background
as a service.  The Master configures itself through Puppet.  Be aware, if you
make a puppet change that affects all nodes, you will be affecting the master
config as well (e.g. a global change to /etc/hosts).

Do not be fearful!  Just be careful, and think about how your changes will
affect every node in your ecosystem.

Do **not** disable the puppet agent on the master thinking that you're guarding
against accidental changes that could break your puppet infrastructure.  The
agent runs on the master are required for mcollective key distribution (they
get stored in the PuppetDB and then installed on the other nodes of puppet infra
via exported resources).  Also, there are certain configuration params
in the puppet.conf on the master that are managed by puppet itself.


```shell
[root@puppet ~]# puppet agent -t
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for puppet.example.com
Info: Applying configuration version '1479158750'
Notice: Applied catalog in 15.20 seconds
```

Not too exciting.  We have confirmed that the agent run succeeds, so we know the puppetmaster is up and running and able
to build a catalog for itself. Good.

If you're on a terminal that supports ANSI color, you'll notice that the text
is in **GREEN**.  If there are any puppet errors during a puppet run, those errors
would show up in **RED** text.

![Puppet Agent Clean Run](images/Puppet-Agent-Clean-Run-puppet.png)

Okay, let's move on...

---

Continue to **Lab #4** --> [Install Puppet Agent on agent node, and do test puppet run](04-Install-Puppet-Agent.md#lab-4)

---

### Further Reading

These links are not needed for this Lab, but for reference here's the PE Install Guide at the PuppetLabs web site:

Quick Start Guide:  <https://docs.puppetlabs.com/pe/2016.5/quick_start_install_mono.html>

Detailed Install Guide:  <https://docs.puppetlabs.com/pe/2016.5/install_basic.html>

Split Install:   <https://docs.puppetlabs.com/pe/2016.5/install_pe_split.html>

LEI Install:   <https://docs.puppetlabs.com/pe/2016.5/install_multimaster.html>

---

<-- [Back to Contents](/README.md)

---
<-- [Back](03-Install-Puppet-Master.md#lab-3)

---

### **Lab #4** - Install the Puppet Agent

---

### Overview

Time to complete:  10-15 minutes

In this lab we will:

* Manually edit the /etc/hosts to prepare for the agent install (if not already done)
* Install the Puppet Agent on the **agent** node

### Pre-installation Steps

Make sure your **agent** VM is started, and get logged in.

```
     vagrant up agent
     vagrant ssh agent
```

### Edit the hosts file

Again, our `/etc/hosts` file should already be setup, as we've used a pre-configured
Vagrant base box that already has the entries we want, but for the sake of completness,
let's double check...

View and/or edit `/etc/hosts` and make sure it looks exactly like this (delete any other lines):
(This should be the exact same hosts file as we created on the **puppet** master node)

```
127.0.0.1      localhost
192.168.198.10 puppet.example.com puppet
192.168.198.11 agent.example.com  agent
192.168.198.12 gitlab.example.com gitlab
```

In a later lab we will write the puppet code to maintain the /etc/hosts entries,
but for now we add them manually.

### Install the Agent

To install the puppet agent on the **agent** node, we can take advantage of
a feature of the PE Master:  The PE Master makes available the agent installer
behind its own web server.  You can use **wget** or **curl** to download the
installer script and then pipe it through bash.

* To use **wget**

```
     wget --no-check-certificate --secure-protocol=TLSv1 -O - https://puppet:8140/packages/current/install.bash | bash -s main:certname=agent.example.com
```

* To use **curl**

```
     curl -k --tlsv1 https://puppet:8140/packages/current/install.bash | bash -s main:certname=agent.example.com
```

If you'd like to browse what else is accessible via that web server, try
opening <https://localhost:22140/packages> in your workstation's web browser.

(Remember we port-forwarded **8140** <--> **22140** on our hosting workstation)

Go ahead an install the agent if you haven't already done so, and then
try running the puppet agent...

* Puppet Install Output:  [04-Puppet-Agent-Install-Output.md](04-Puppet-Agent-Install-Output.md)


### Run the Puppet Agent

Run the puppet agent manually.  This will cause an SSL certificate request
to be generated and sent to the puppetmaster (if it hasn't already happened
in the background.)

```
     [root@agent ~]# puppet agent -t
     Info: Creating a new SSL key for agent.example.com
     Info: Caching certificate for ca
     Info: Caching certificate_request for agent.example.com
     Info: Caching certificate for ca
     Exiting; no certificate found and waitforcert is disabled
```

Note:  the puppet agent is also running in the background, so it may have already generated the SSL key,
so you may not see that in your output.  In that case, you would see just this:

```
[root@agent ~]# puppet agent -t
Exiting; no certificate found and waitforcert is disabled
```

### Sign the Certificate

Next, We need to sign the agent's cert on the master, so switch to your **puppet**
window/terminal and issue the following commands on the puppet master as root:

```
     puppet cert list
     puppet cert sign agent.example.com
```

The **puppet cert list** command shows any outstanding certificate signing requests.  You should see the one that was just generated by your agent run.

```
     [root@puppet ~]# puppet cert list
       "agent.example.com" (SHA256) 31:EA:4D:60:DE:44:E8:E1:A1:1A:2E:48:1E:81:CA:40:43:4A:A7:39:E8:B9:61:63:F3:0F:CF:2E:B7:CC:98:22
```

The **puppet cert sign agent.example.com** command signs the cert, and removes the signing request.

```
     [root@puppet ~]# puppet cert sign agent.example.com
     Notice: Signed certificate request for agent.example.com
     Notice: Removing file Puppet::SSL::CertificateRequest agent.example.com at '/etc/puppetlabs/puppet/ssl/ca/requests/agent.example.com.pem'
```

### Run puppet...

Now, back on the agent node:  Let's run puppet again (be sure you're running as root)

```
     puppet agent -t
```

You should see a lot of output to the screen showing the changes that are being applied.
(Puppet is installing and configuring MCollective on the agent.)
However, because puppet runs automatically in the background every 5 minutes prior to
its certificate being signed, there is a small chance that the first puppet run will
occur before you're able to do a manual run.  In that case, you should see a little output
as in the second puppet run (no changes made.)

### Run the Puppet Agent Again

Run puppet a **second** time, and you should get a clean run with no changes.

```
     puppet agent -t
```

![Puppet Agent Clean Run](images/Puppet-Agent-Clean-Run-agent.png)

For brevity, I've not included the output on this page, but it's available for viewing
here:

* Puppet Run Output:  [04-Puppet-Agent-Run-Output.md](04-Puppet-Agent-Run-Output.md)

### The Certname

The **certname** is the name assigned to the puppet agent.  It's how the puppet
master identifies each node.  Most of the time, the certname will be the same
as the host's FQDN, but it could be anything you choose.  Best practice is to
use the FQDN.

You may have noticed that when we installed the puppet agent, we passed in
an additional parameter using the `bash -s <key:value>` construct.  If you've
had any exposure to Puppet before, you may be asking yourself "Way did we have
to do that?"  The answer lies in how puppet determines the certname if you
dont provide one.  By default, puppet will take the hostname, and append the
domain name as configured in the /etc/resolv.conf.  We've not configured
the /etc/resolv.conf at all, and Vagrant automatically configures it to when
the VM is started up each time. It will inherit the DNS config from the host
system.  The inherited config will be different for everyone, and since the
agent installer would normally attempt to determine the **certname** from the
hostname and DNS domain, we don't want to rely on it here as we'd get
unpredictable results.  By manually specifying the certname, we guarantee we
will always get the name we want, rather than letting puppet try and pick
it for us.

There are other params we may wish to pass in to the install script as well.
You can pass in any config item that makes sense.  Just run `puppet config print`
to see the possibilities.

Also, to read more about passing parameters to the agent installer, see:  https://docs.puppet.com/pe/latest/install_agents.html#passing-configuration-parameters-to-the-install-script

### Summary

At this point we have:

- a **Puppet Master node** (hostname **puppet.example.com**) that also runs an agent to configure itself
- a **Puppet Agent node** (hostnamne **agent.example.com**) that runs an agent, and where we will test code and learn more about PE

If you login to the [PE Console](https://127.0.0.1:22443/), you should see these two agents on the 'Nodes' page

---

Continue to **Lab #5** --> [Get familiar with puppet config files, and puppet code, and CLI](05-Puppet-Config-and-Code.md#lab-5)

---

<-- [Back to Contents](/README.md)

---


Here's the output from the agent install...

```
[root@agent ~]# wget --no-check-certificate --secure-protocol=TLSv1 -O - https://puppet:8140/packages/current/install.bash | bash -s agent:certname=agent.example.com
--2016-11-14 21:43:12--  https://puppet:8140/packages/current/install.bash
Resolving puppet (puppet)... 192.168.198.10
Connecting to puppet (puppet)|192.168.198.10|:8140... connected.
WARNING: cannot verify puppet's certificate, issued by ‘/CN=Puppet Enterprise CA generated on puppet.example.com at +2016-11-14 18:16:25 +0000’:
  Unable to locally verify the issuer's authority.
HTTP request sent, awaiting response... 200 OK
Length: 20106 (20K)
Saving to: ‘STDOUT’

100%[======================================================================>] 20,106      --.-K/s   in 0s

2016-11-14 21:43:12 (345 MB/s) - written to stdout [20106/20106]

Loaded plugins: fastestmirror
Cleaning repos: pe_repo
Cleaning up everything
Cleaning up list of fastest mirrors
Loaded plugins: fastestmirror
base                                                                                     | 3.6 kB  00:00:00
extras                                                                                   | 3.4 kB  00:00:00
pe_repo                                                                                  | 2.5 kB  00:00:00
updates                                                                                  | 3.4 kB  00:00:00
pe_repo/primary_db                                                                       |  24 kB  00:00:00
Determining fastest mirrors
 * base: mirror.millry.co
 * extras: mirror.eboundhost.com
 * updates: mirror.atlantic.net
Error: No matching Packages to list
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.millry.co
 * extras: mirror.eboundhost.com
 * updates: mirror.atlantic.net
Resolving Dependencies
--> Running transaction check
---> Package puppet-agent.x86_64 0:1.7.1-1.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================================================
 Package                      Arch                   Version                      Repository               Size
================================================================================================================
Installing:
 puppet-agent                 x86_64                 1.7.1-1.el7                  pe_repo                  24 M

Transaction Summary
================================================================================================================
Install  1 Package

Total download size: 24 M
Installed size: 114 M
Downloading packages:
warning: /var/cache/yum/x86_64/7/pe_repo/packages/puppet-agent-1.7.1-1.el7.x86_64.rpm: Header V4 RSA/SHA1 Signature, key ID ef8d349f: NOKEY
Public key for puppet-agent-1.7.1-1.el7.x86_64.rpm is not installed
puppet-agent-1.7.1-1.el7.x86_64.rpm                                                      |  24 MB  00:00:00
Retrieving key from https://puppet.example.com:8140/packages/GPG-KEY-puppetlabs
Importing GPG key 0x4BD6EC30:
 Userid     : "Puppet Labs Release Key (Puppet Labs Release Key) <info@puppetlabs.com>"
 Fingerprint: 47b3 20eb 4c7c 375a a9da e1a0 1054 b7a2 4bd6 ec30
 From       : https://puppet.example.com:8140/packages/GPG-KEY-puppetlabs
Retrieving key from https://puppet.example.com:8140/packages/GPG-KEY-puppet
Importing GPG key 0xEF8D349F:
 Userid     : "Puppet, Inc. Release Key (Puppet, Inc. Release Key) <release@puppet.com>"
 Fingerprint: 6f6b 1550 9cf8 e59e 6e46 9f32 7f43 8280 ef8d 349f
 From       : https://puppet.example.com:8140/packages/GPG-KEY-puppet
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : puppet-agent-1.7.1-1.el7.x86_64                                                              1/1
  Verifying  : puppet-agent-1.7.1-1.el7.x86_64                                                              1/1

Installed:
  puppet-agent.x86_64 0:1.7.1-1.el7

Complete!
service { 'puppet':
  ensure => 'stopped',
}
Notice: /Service[puppet]/ensure: ensure changed 'stopped' to 'running'
service { 'puppet':
  ensure => 'running',
  enable => 'true',
}
service { 'puppet':
  ensure => 'running',
  enable => 'true',
}
Notice: /File[/usr/local/bin/facter]/ensure: created
file { '/usr/local/bin/facter':
  ensure => 'link',
  target => '/opt/puppetlabs/puppet/bin/facter',
}
Notice: /File[/usr/local/bin/puppet]/ensure: created
file { '/usr/local/bin/puppet':
  ensure => 'link',
  target => '/opt/puppetlabs/puppet/bin/puppet',
}
Notice: /File[/usr/local/bin/pe-man]/ensure: created
file { '/usr/local/bin/pe-man':
  ensure => 'link',
  target => '/opt/puppetlabs/puppet/bin/pe-man',
}
Notice: /File[/usr/local/bin/hiera]/ensure: created
file { '/usr/local/bin/hiera':
  ensure => 'link',
  target => '/opt/puppetlabs/puppet/bin/hiera',
}
[root@agent ~]# puppet agent -t

```


---

Back to **Lab #4** --> [Install Puppet Agent](04-Install-Puppet-Agent.md#install-the-agent)

---

<-- [Back to Contents](/README.md)

---


Here's the output from the inital puppet run on the **agent** VM...

```
[root@agent ~]# puppet agent -t
Info: Caching certificate for agent.example.com
Info: Caching certificate_revocation_list for ca
Info: Caching certificate for agent.example.com
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/aio_agent_build.rb]/ensure: defined content as '{md5}cdcc1ff07bc245c66cc1d46be56b3af5'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/aio_agent_version.rb]/ensure: defined content as '{md5}d05c8cbf788f47d33efd46a935dda61e'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/pe_build.rb]/ensure: defined content as '{md5}ee54c728457b32d6622c3985448918fa'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/pe_concat_basedir.rb]/ensure: defined content as '{md5}0ccd3500f29b9dd346a45a61268c7c18'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/pe_razor_server_version.rb]/ensure: defined content as '{md5}ec91d8b92e03d5f952c789308d26dcd0'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/pe_server_version.rb]/ensure: defined content as '{md5}17c2795fe8a56b731ae0fc81ba147e6a'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/pe_version.rb]/ensure: defined content as '{md5}b0cd9b5b3fed73bc0d6424d8ac1d6639'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/platform_symlink_writable.rb]/ensure: defined content as '{md5}fc1e2766ff9994fa5df95cdc14b9bcd2'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/platform_tag.rb]/ensure: defined content as '{md5}ba51554600d31251f66baaf81b00639a'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/puppet_files_dir_present.rb]/ensure: defined content as '{md5}3900e124be2f377638dd1522079856bf'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/staging_http_get.rb]/ensure: defined content as '{md5}2c27beb47923ce3acda673703f395e68'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/windows.rb]/ensure: defined content as '{md5}d8880f6f32905f040f3355e2a40cf088'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/face]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/face/node]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/face/node/purge.rb]/ensure: defined content as '{md5}2dc21d637a51cdb5b4e7997409eee1fc'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/feature]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/feature/pe_hocon.rb]/ensure: defined content as '{md5}bbd4eca7117850bcef6f3be059cf250c'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/feature/pe_puppet_authorization.rb]/ensure: defined content as '{md5}c673198b8d2117318558170c0a7f5ced'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/build_mcollective_metadata_cron_minute_array.rb]/ensure: defined content as '{md5}d657907920f0d58902578b23b93a7aab'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/cookie_secret_key.rb]/ensure: defined content as '{md5}a1a48191d1f0cb934b0c63d8fec70566'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/create_java_args_subsettings_hash.rb]/ensure: defined content as '{md5}b54be02c9f0b0eeee699764df57a2db3'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_any2array.rb]/ensure: defined content as '{md5}3384ea4d25dc66d898717c9ca6bb5507'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_bool2str.rb]/ensure: defined content as '{md5}f6189451331df6fd24ec69d7cdc76abe'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_build_version.rb]/ensure: defined content as '{md5}cc956210f4ef17fb396513dffdee1ed7'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_chomp.rb]/ensure: defined content as '{md5}b4f0cb35578710dc4ac315d35e9571a2'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_compile_master.rb]/ensure: defined content as '{md5}73a4baa192f4461b5d82856094bc6ffb'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_compiling_server_aio_build.rb]/ensure: defined content as '{md5}d01e0cac62411df5140da0956a79544c'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_compiling_server_version.rb]/ensure: defined content as '{md5}dfa2285cae91d2985408274b24692d69'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_concat_getparam.rb]/ensure: defined content as '{md5}46df3de760f918b120fb2254f85eff2a'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_concat_is_bool.rb]/ensure: defined content as '{md5}b511d7545ede5abae00951199b67674d'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_count.rb]/ensure: defined content as '{md5}eba067719da25b908662eec256ebc9b4'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_create_amq_augeas_command.rb]/ensure: defined content as '{md5}a62e6f52c8a5bdc002436dc6c292fd48'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_current_server_version.rb]/ensure: defined content as '{md5}f08526ad8a79c173dc0759584fb2e397'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_delete_undef_values.rb]/ensure: defined content as '{md5}c25bbcdfc6bca2d219e5f42f3eb8fa0b'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_directory_exists.rb]/ensure: defined content as '{md5}18df1a47e5e04af8278b937953bf3179'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_empty.rb]/ensure: defined content as '{md5}01a6574fab1ed1cf94ef1fea4954eeca'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_flatten.rb]/ensure: defined content as '{md5}c781954451d1860ca8f63fd6d2b6cf76'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_format_urls.rb]/ensure: defined content as '{md5}b03ddf9f0600bc0122dfe3ba814c3ba7'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_getvar.rb]/ensure: defined content as '{md5}f445be97e6541d0ae577ea5450479067'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_grep.rb]/ensure: defined content as '{md5}ae92a94aa1c964ff2af58d74df115af8'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_is_array.rb]/ensure: defined content as '{md5}b451e133e015fc7e8dc4dcfdf059a8d8'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_is_bool.rb]/ensure: defined content as '{md5}2dfe2be70aaff951b59e9fba3e85aa5d'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_is_integer.rb]/ensure: defined content as '{md5}a410ba3f3586b7df90e532b2eb99da37'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_is_string.rb]/ensure: defined content as '{md5}5fe6741e70f2bbb93a0ae43d233eeebc'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_join.rb]/ensure: defined content as '{md5}4f433ea29dffc79247671fb4271d0a10'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_join_keys_to_values.rb]/ensure: defined content as '{md5}3066c3bd5e181a996729691a57cf3d21'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_loadyaml.rb]/ensure: defined content as '{md5}6adef0c167fbe0167a8e7aec7c65317b'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_max.rb]/ensure: defined content as '{md5}e04acd17070545133c83ec5a0e11c022'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_merge.rb]/ensure: defined content as '{md5}0971d635342b84a3a5c5a40ac36d9807'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_min.rb]/ensure: defined content as '{md5}e6d2b8c614168f4224e3f76f32d9f9cb'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_pick.rb]/ensure: defined content as '{md5}06b3a9e63faf3ca5d64c65fa14803cdf'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_postgresql_acls_to_resources_hash.rb]/ensure: defined content as '{md5}851d972daf92e9e0600f8991a15311be'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_postgresql_escape.rb]/ensure: defined content as '{md5}cc58b659957328d9577336353bc246b2'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_postgresql_password.rb]/ensure: defined content as '{md5}72c33c3b7e4a6e8128fbb0a52bf30282'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_prefix.rb]/ensure: defined content as '{md5}554fcaf9362a544f91bb192047dd5341'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_puppetserver_static_content_list.rb]/ensure: defined content as '{md5}3955ef0b5cd765be892a3043386cf91f'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_servername.rb]/ensure: defined content as '{md5}c60209498856941f6f794d4c3cfb5d1f'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_size.rb]/ensure: defined content as '{md5}876097c7df6d07296524bb2236f60a1d'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_sort.rb]/ensure: defined content as '{md5}26047743025f2fdcf5e7b5420d5382ea'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_strip.rb]/ensure: defined content as '{md5}8d60a607f04fc6622eca8ae46e2fef2f'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_suffix.rb]/ensure: defined content as '{md5}a44749c5ef30e258866cb18fd83f77d2'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_to_bytes.rb]/ensure: defined content as '{md5}6ee36cabe336db4c281c7d3b1b1d771e'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_union.rb]/ensure: defined content as '{md5}da3ea966f5468bbdb8420975576d4a3f'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_unique.rb]/ensure: defined content as '{md5}5edb2c537d80f003d71a250bf203c79e'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_upcase.rb]/ensure: defined content as '{md5}4ea67e96c4da45092fb70fcdf1f0692f'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_validate_absolute_path.rb]/ensure: defined content as '{md5}d4bd539a3d7db93d4563cbbe571a16ac'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_validate_array.rb]/ensure: defined content as '{md5}0ab10b81de351aa9f6114c1880cc7155'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_validate_bool.rb]/ensure: defined content as '{md5}4a74954e0502837f11d2eadedb71bc1f'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_validate_hash.rb]/ensure: defined content as '{md5}36a223b1648dec8cb30fd229e3bb74c6'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_validate_re.rb]/ensure: defined content as '{md5}bccac35a2607bf15f1e7d1c565c1d98b'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_validate_single_integer.rb]/ensure: defined content as '{md5}ef8c455e5d58954bc4e78e36534a340c'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_validate_string.rb]/ensure: defined content as '{md5}c4d83c3ef14c2e3e47c4408d49c22437'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/pe_zip.rb]/ensure: defined content as '{md5}77f42491eaa279803a36b02601206b33'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/scope_defaults.rb]/ensure: defined content as '{md5}da916d46f3ff3be8359f75c93c2b5532'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/parser/functions/staging_parse.rb]/ensure: defined content as '{md5}605c4de803c65f2c3613653b68921002'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_file_line]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_file_line/ruby.rb]/ensure: defined content as '{md5}79d77c28f8a311684aceec3e08c1a084'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_hocon_setting]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_hocon_setting/ruby.rb]/ensure: defined content as '{md5}c0bad8a42357896675e209aab7ee6a0d'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_ini_setting]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_ini_setting/ruby.rb]/ensure: defined content as '{md5}d0520f108a6f0e55320a97f8285a0843'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_ini_subsetting]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_ini_subsetting/ruby.rb]/ensure: defined content as '{md5}7245892fe493f361b4f2fb34188e71db'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_java_ks]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_java_ks/keytool.rb]/ensure: defined content as '{md5}6e16c71a9e74550cfe9aad4ecbb8fd22'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_postgresql_conf]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_postgresql_conf/parsed.rb]/ensure: defined content as '{md5}f0e7fc6f14420d46ebf64635939243af'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_postgresql_psql]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_postgresql_psql/ruby.rb]/ensure: defined content as '{md5}68b0e90ab501fc36b821c4c27c74fb17'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_puppet_authorization_hocon_rule]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/pe_puppet_authorization_hocon_rule/ruby.rb]/ensure: defined content as '{md5}1aaaa7466dc6d830cb25332cc2910c07'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/pe_anchor.rb]/ensure: defined content as '{md5}5505f2e5850c0dd2e56583d214baf197'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/pe_file_line.rb]/ensure: defined content as '{md5}5cecf4e63d31bc89f31a9be54a248359'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/pe_hocon_setting.rb]/ensure: defined content as '{md5}204b2889d1c6db8f986f02ed17239ef5'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/pe_ini_setting.rb]/ensure: defined content as '{md5}5a6ac7186c2c2be8008f128a971b104e'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/pe_ini_subsetting.rb]/ensure: defined content as '{md5}022dc2b30ed8daa8ce2226017bc95a38'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/pe_java_ks.rb]/ensure: defined content as '{md5}9bb59c04ff805eb7e824fc4e5b4c9767'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/pe_postgresql_conf.rb]/ensure: defined content as '{md5}140d5cb21ae1b8554f40c71f2b73b332'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/pe_postgresql_psql.rb]/ensure: defined content as '{md5}ffabdb11eb481e45c76795195672436c'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/pe_puppet_authorization_hocon_rule.rb]/ensure: defined content as '{md5}2a9e64fd982a8c0b118d0ce803c92bfb'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/util]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/util/external_iterator.rb]/ensure: defined content as '{md5}69ad1eb930ca6d8d6b6faea343b4a22e'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/util/pe_ini_file]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/util/pe_ini_file.rb]/ensure: defined content as '{md5}9ba01a79162a1d69ab8e90e725d07d3a'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/util/pe_ini_file/section.rb]/ensure: defined content as '{md5}652d2b45e5defc13fb7989f020e6080f'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/util/setting_value.rb]/ensure: defined content as '{md5}a649418f4c767d976f4bf13985575b3c'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/shared]/ensure: created
Notice: /File[/opt/puppetlabs/puppet/cache/lib/shared/aio_build.rb]/ensure: defined content as '{md5}d0fe0c2b31687ea03c1ede01a460f3a0'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/shared/pe_build.rb]/ensure: defined content as '{md5}4f4652af20c4f0391b9ca2976940a710'
Notice: /File[/opt/puppetlabs/puppet/cache/lib/shared/pe_server_version.rb]/ensure: defined content as '{md5}f3d3fc8776512ae73d3293c97b8f3dfe'
Info: Loading facts
Info: Caching catalog for agent.example.com
Info: Applying configuration version '1479159910'
Notice: /Stage[main]/Puppet_enterprise::Symlinks/File[/usr/local/bin/facter]/target: target changed '/opt/puppetlabs/puppet/bin/facter' to '/opt/puppetlabs/bin/facter'
Notice: /Stage[main]/Puppet_enterprise::Symlinks/File[/usr/local/bin/puppet]/target: target changed '/opt/puppetlabs/puppet/bin/puppet' to '/opt/puppetlabs/bin/puppet'
Notice: /Stage[main]/Puppet_enterprise::Symlinks/File[/usr/local/bin/pe-man]/target: target changed '/opt/puppetlabs/puppet/bin/pe-man' to '/opt/puppetlabs/bin/pe-man'
Notice: /Stage[main]/Puppet_enterprise::Symlinks/File[/usr/local/bin/hiera]/target: target changed '/opt/puppetlabs/puppet/bin/hiera' to '/opt/puppetlabs/bin/hiera'
Notice: /Stage[main]/Puppet_enterprise::Pxp_agent/File[/etc/puppetlabs/pxp-agent/pxp-agent.conf]/ensure: defined content as '{md5}7ad41dbd7e2345c888107f6496fd5afe'
Info: /Stage[main]/Puppet_enterprise::Pxp_agent/File[/etc/puppetlabs/pxp-agent/pxp-agent.conf]: Scheduling refresh of Service[pxp-agent]
Notice: /Stage[main]/Puppet_enterprise::Pxp_agent::Service/Service[pxp-agent]/ensure: ensure changed 'stopped' to 'running'
Info: /Stage[main]/Puppet_enterprise::Pxp_agent::Service/Service[pxp-agent]: Unscheduling refresh on Service[pxp-agent]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/agent]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/agent/package.ddl]/ensure: defined content as '{md5}ae1d49824b9b84d1a8f617c317147bea'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/agent/package.rb]/ensure: defined content as '{md5}60d4b37d3844fc379a99d2b17a243620'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/agent/puppet.ddl]/ensure: defined content as '{md5}69e63795545712fd9e3d75ea1ae1d1d4'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/agent/puppet.rb]/ensure: defined content as '{md5}8f4839ea11e4c4911e531f77f94b033b'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/agent/puppetral.ddl]/ensure: defined content as '{md5}7f06f13953847e60818a681c1f2f168b'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/agent/puppetral.rb]/ensure: defined content as '{md5}686272ee73d966e3f1d3482d7d7b61a8'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/agent/service.ddl]/ensure: defined content as '{md5}3471e24142773d1bb7769c250e6b63d3'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/agent/service.rb]/ensure: defined content as '{md5}cbf84ed615eeda9789650b05ec504566'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/aggregate]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/aggregate/boolean_summary.ddl]/ensure: defined content as '{md5}aa581c71a6c7658bffdbaec81590f65d'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/aggregate/boolean_summary.rb]/ensure: defined content as '{md5}0546063313508d8aff603be320af3c44'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/application]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/application/package.rb]/ensure: defined content as '{md5}4e6571cdac3f6aa322c9f195693e1dbe'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/application/puppet.rb]/ensure: defined content as '{md5}e8085d91ddaa1f92984bde5d34cc47d5'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/application/service.rb]/ensure: defined content as '{md5}c95359f947af5f0d904fa3df80cb9820'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/data]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/data/puppet_data.ddl]/ensure: defined content as '{md5}5c9912bf5ae5dbc8762109a40c027c63'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/data/puppet_data.rb]/ensure: defined content as '{md5}606e87cd509addf22dd8e93d503d8262'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/data/resource_data.ddl]/ensure: defined content as '{md5}c4e3a46fd3c0b5d3990db0b8af1c747f'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/data/resource_data.rb]/ensure: defined content as '{md5}49be769fb403191af41f1b89697ce4cc'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/data/service_data.ddl]/ensure: defined content as '{md5}e7f7e0bc65ede56fc636505a400b1700'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/data/service_data.rb]/ensure: defined content as '{md5}bc651898c7dcd373d609c933fbd6021f'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/mco_plugin_versions]/ensure: defined content as '{md5}c96b3a9206e43f8c3a0550731dd3b739'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/registration]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/registration/meta.rb]/ensure: defined content as '{md5}e939958bbbc0817e1779c336037e1849'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/security]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/security/sshkey.ddl]/ensure: defined content as '{md5}e92b26732d03496fb61ad3a1ed623f56'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/security/sshkey.rb]/ensure: defined content as '{md5}c3933bda744b78dd857f20aa5b61f75b'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/actionpolicy.rb]/ensure: defined content as '{md5}e4d6a7024ad7b28e019e7b9931eac027'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/package]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/package/base.rb]/ensure: defined content as '{md5}1bdb7e7a6dcfea6fd2a06c5dc39b7276'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/package/packagehelpers.rb]/ensure: defined content as '{md5}312aecc3b1ee75f97a989fea3e7a221d'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/package/puppetpackage.rb]/ensure: defined content as '{md5}865eec36ae05c30b072d3f5bd871fb52'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/package/yumHelper.py]/ensure: defined content as '{md5}40fa99ef10b84c38517f6b695a0af533'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/package/yumpackage.rb]/ensure: defined content as '{md5}256bde1567d8794ca929092462f5ae03'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/puppet_agent_mgr]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/puppet_agent_mgr.rb]/ensure: defined content as '{md5}4dbafcaa02334c2d76665cd23ca29688'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/puppet_agent_mgr/mgr_v2.rb]/ensure: defined content as '{md5}9a00171022ddb12d0a463e9cefeba481'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/puppet_agent_mgr/mgr_v3.rb]/ensure: defined content as '{md5}b5cb1a9b7311fc3769a3ccaabadeb694'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/puppet_agent_mgr/mgr_windows.rb]/ensure: defined content as '{md5}79a6cf3dac0177f6b9c22d5085324676'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/puppet_server_address_validation.rb]/ensure: defined content as '{md5}1c78390e33e71773e121a902ae91bfd4'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/puppetrunner.rb]/ensure: defined content as '{md5}a4fade81457455fbca9370249defbdf1'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/service]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/service/base.rb]/ensure: defined content as '{md5}abea7b8fadbf3425a7b68b49b9435ff6'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/util/service/puppetservice.rb]/ensure: defined content as '{md5}905db93e1c06ad5a7154fa2f9199f31c'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator/puppet_resource_validator.ddl]/ensure: defined content as '{md5}3e45a28e1ba6c8d22ce40934c04b30b4'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator/puppet_resource_validator.rb]/ensure: defined content as '{md5}567c7dc4d70ed0db7fd2626c77f6df41'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator/puppet_server_address_validator.ddl]/ensure: defined content as '{md5}323e0b9647639fdf32cfbc63a82860f7'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator/puppet_server_address_validator.rb]/ensure: defined content as '{md5}e84a56187809c5181b78b2819ee149fe'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator/puppet_tags_validator.ddl]/ensure: defined content as '{md5}7ed95b2e5b210db83d12d5034f1ecb0f'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator/puppet_tags_validator.rb]/ensure: defined content as '{md5}40b29498e867ba2ecf21dc08bc457d4e'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator/puppet_variable_validator.ddl]/ensure: defined content as '{md5}58c9db4ca4503e4d692a016743e01627'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator/puppet_variable_validator.rb]/ensure: defined content as '{md5}3cbca3af2e5884f2a807ef005a87151b'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator/service_name.ddl]/ensure: defined content as '{md5}2812afa15108103042f706c2201e286b'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppetlabs/mcollective/plugins/mcollective/validator/service_name.rb]/ensure: defined content as '{md5}3f501a9ed252ce2dfe06a2e1e53845ab'
Info: /opt/puppetlabs/mcollective/plugins/mcollective: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Logs/File[/var/log/puppetlabs/mcollective.log]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Logs/File[/var/log/puppetlabs/mcollective-audit.log]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl]/ensure: created
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/clients]/ensure: created
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/clients]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/ca.cert.pem]/ensure: defined content as '{md5}e3aa47a940ec4d56b40cdd3a67f9a60e'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/ca.cert.pem]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/agent.example.com.cert.pem]/ensure: defined content as '{md5}0f3acc9a7204aa22df878bfc46a401e8'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/agent.example.com.cert.pem]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/agent.example.com.private_key.pem]/ensure: defined content as '{md5}532136e0394bdd61314273a6f905f5e7'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/agent.example.com.private_key.pem]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/mcollective-private.pem]/ensure: defined content as '{md5}63d793c924bcc349d5c3632a35d980b6'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/mcollective-private.pem]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/mcollective-public.pem]/ensure: defined content as '{md5}bfed7e0a0c10eb1120ae265cd60e55fd'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/mcollective-public.pem]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/clients/puppet-dashboard-public.pem]/ensure: defined content as '{md5}d41d8cd98f00b204e9800998ecf8427e'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/clients/puppet-dashboard-public.pem]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/clients/peadmin-public.pem]/ensure: defined content as '{md5}81569970c95d5fc36161532eb93cef18'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/clients/peadmin-public.pem]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Facter/File[/opt/puppetlabs/puppet/bin/refresh-mcollective-metadata]/ensure: defined content as '{md5}6daa27a4146a20dea32f525f725563a1'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Facter/Exec[bootstrap mcollective metadata]/returns: executed successfully
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Facter/Cron[pe-mcollective-metadata]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Facter/File[/etc/puppetlabs/mcollective/facts-bootstrapped]/ensure: created
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server::Facter/File[/etc/puppetlabs/mcollective/facts-bootstrapped]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]/content:
--- /etc/puppetlabs/mcollective/server.cfg    2016-10-12 02:58:21.000000000 +0000
+++ /tmp/puppet-file20161114-4747-13xcmul    2016-11-14 21:45:16.440205999 +0000
@@ -1,27 +1,83 @@
-main_collective = mcollective
-collectives = mcollective
-
-libdir = /opt/puppetlabs/mcollective/plugins
-
-# consult the "classic" libdirs too
-libdir = /usr/share/mcollective/plugins
-libdir = /usr/libexec/mcollective

-logfile = /var/log/puppetlabs/mcollective.log
-loglevel = info
-daemonize = 1
-
-# Plugins
-securityprovider = psk
-plugin.psk = unset
+# Centrally managed by Puppet version 4.7.0
+# https://docs.puppetlabs.com/mcollective/configure/server.html

+# Connector settings (required):
+# -----------------------------
 connector = activemq
+direct_addressing = 1
+
+# ActiveMQ connector settings:
+plugin.activemq.randomize = false
 plugin.activemq.pool.size = 1
-plugin.activemq.pool.1.host = stomp1
-plugin.activemq.pool.1.port = 6163
+plugin.activemq.pool.1.host = puppet.example.com
+plugin.activemq.pool.1.port = 61613
 plugin.activemq.pool.1.user = mcollective
-plugin.activemq.pool.1.password = marionette
+plugin.activemq.pool.1.password = QqSrsBgYn01bfW6RmX0m3Q
+plugin.activemq.pool.1.ssl = true
+plugin.activemq.pool.1.ssl.ca = /etc/puppetlabs/mcollective/ssl/ca.cert.pem
+plugin.activemq.pool.1.ssl.cert = /etc/puppetlabs/mcollective/ssl/agent.example.com.cert.pem
+plugin.activemq.pool.1.ssl.key = /etc/puppetlabs/mcollective/ssl/agent.example.com.private_key.pem
+plugin.activemq.heartbeat_interval = 120
+plugin.activemq.max_hbrlck_fails = 0
+
+# Security plugin settings (required):
+# -----------------------------------
+securityprovider           = ssl
+
+# SSL plugin settings:
+plugin.ssl_server_private  = /etc/puppetlabs/mcollective/ssl/mcollective-private.pem
+plugin.ssl_server_public   = /etc/puppetlabs/mcollective/ssl/mcollective-public.pem
+plugin.ssl_client_cert_dir = /etc/puppetlabs/mcollective/ssl/clients
+plugin.ssl_serializer      = yaml

-# Facts
+# Facts, identity, and classes (recommended):
+# ------------------------------------------
 factsource = yaml
 plugin.yaml = /etc/puppetlabs/mcollective/facts.yaml
+fact_cache_time = 300
+
+identity = agent.example.com
+
+classesfile = /opt/puppetlabs/puppet/cache/state/classes.txt
+
+# Registration (recommended):
+# -----------------------
+registration = Meta
+registerinterval = 600
+
+# Subcollectives (optional):
+# -------------------------
+main_collective = mcollective
+collectives     = mcollective
+
+# Auditing (optional):
+# -------------------
+plugin.rpcaudit.logfile = /var/log/puppetlabs/mcollective-audit.log
+rpcaudit = 1
+rpcauditprovider = logfile
+
+# Authorization (optional):
+# ------------------------
+plugin.actionpolicy.allow_unconfigured = 1
+rpcauthorization = 1
+rpcauthprovider = action_policy
+
+# Logging:
+# -------
+logfile  = /var/log/puppetlabs/mcollective.log
+loglevel = info
+
+# Platform defaults:
+# -----------------
+daemonize = 1
+libdir = /opt/puppet/libexec/mcollective:/opt/puppetlabs/mcollective/plugins
+
+# Puppet Agent plugin configuration:
+# ---------------------------------
+plugin.puppet.splay = true
+plugin.puppet.splaylimit = 120
+plugin.puppet.signal_daemon = 0
+plugin.puppet.command = /opt/puppetlabs/bin/puppet agent
+plugin.puppet.config  = /etc/puppetlabs/puppet/puppet.conf
+

Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]/content: content changed '{md5}73e68cfd79153a49de6f5721ab60657b' to '{md5}cb398b5c245a57431f44a9b941bd246d'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]/mode: mode changed '0644' to '0660'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]: Scheduling refresh of Service[mcollective]
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]: Scheduling refresh of Service[mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Service/Service[mcollective]/ensure: ensure changed 'stopped' to 'running'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Service/Service[mcollective]: Unscheduling refresh on Service[mcollective]
Notice: Applied catalog in 4.21 seconds
```

Followed by another run...  (clean, and no changes.)

```
[root@agent ~]# puppet agent -t
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for agent.example.com
Info: Applying configuration version '1479159923'
Notice: Applied catalog in 0.63 seconds
```

---

Back to **Lab #4** --> [Install Puppet Agent](04-Install-Puppet-Agent.md#run-the-puppet-agent-again)

---

Continue to **Lab #5** --> [Get familiar with puppet config files, and puppet code, and CLI](05-Puppet-Config-and-Code.md#lab-5)

---

<-- [Back to Contents](/README.md)

---

<-- [Back](04-Install-Puppet-Agent.md#lab-4)

---

### **Lab #5** - Get Familiar with Puppet Config Files, Code, and CLI

---

### Overview

Time to complete:  60 minutes

In this lab, we will familiarize ourselves with:

- Puppet config file:  **puppet.conf**
- Puppet from the command line
- Puppet code basics
- Tieing code to a node (**Node Classification**)

In the previous labs we deployed a Puppet Master node and another with a
Puppet Agent.  Now what can we do with them?  Let's start by looking at
where puppet got installed, how we can use puppet to make a config change
on an agent system, and then we'll dig into puppet config and coding in
more depth...

### Where is Puppet Enterprise Installed?

Let's look at where PE installs itself, the config files
we should know about, and how to get started writing some
puppet code to do real work...

Let's start by looking at the **Puppet Master** (puppet.example.com)

The PE config file for Puppet, as well as the various
other components that come with PE (such as MCollective)
are stored under **/etc/puppetlabs**

As of PE 2015.x.y, the default location of puppet code is `/etc/puppetlabs/code`
(Historically, code has been in `/etc/puppetlabs/puppet/environments`, but this
is no longer the case.)

Let's look at Puppet and its config and code under:  **/etc/puppetlabs/code**

Here are the files and directories we will start looking at.

```
     [root@puppet ~]# tree /etc/puppetlabs/code

     /etc/puppetlabs/code
     ├── environments
     │   └── production
     │       ├── environment.conf
     │       ├── hieradata
     │       ├── manifests
     │       │   └── site.pp
     │       └── modules
     └── modules
```

The **site.pp** is the **main manifest** (also called the **site manifest**)
that puppet reads first.  It's the first bit of code that puppet parses, or
the point of entry into our Puppet codebase.  Every other bit of code "hangs off"
of the `site.pp`.  We will talk more about what things you can put in the `site.pp`
in the next lab.

Notice that there is a **production** directory, which corresponds to the
puppet **production environment**.  Out of the box, there is a single
environment called **production** although others can be created and used.

Within the `production/` directory, we have `manifests/`, `modules/`, and `hieradata/`.
There could also be directories for `files/` and `templates/` but they haven't been
created quite yet.

Some of these same directories are also used within each puppet module.  (A
module is just a bit of puppet code bundled up in a well-defined way, so that
it can be distributed, and used by other people easily.)  We'll learn more
about modules later, but for now just remember that the use of each directory
is well defined, and you shouldn't just use them for whatever you want:

* **files** - used for static files that your puppet code may reference
* **manifests** - where your `site.pp` lives, and potentially other site-developed code
* **modules** - any puppet modules you use go here, including site-developed modules
* **templates** - similar to the files dir, but holds marked-up files in ERB format

### The environments directory

Out-of-the-box, PE comes with a single environment setup, called the **production**
environment.   Environments are useful for containing different sets of modules,
code, and site data.  It's possible, for example, to
test a newer version of a module, or some puppet code you're actively developing,
in a different environment, totally seprate from the **production** environment.
This way, you can make changes to your code, test it on a test system, all without
ever having to worry about affecting production systems.  We will come back to
this topic in more depth in a subsequent lab...For now, just know this: Each
environment gets its own directory within the **environments** directory,
and each environment contains it's own set of manifests, modules, files,
templates, and Hiera data.

### The modules directory

The modules directory can contain additional Puppet code from PuppetLabs or
other third parties, or even in-house-developed  modules.  One common module
that is used by a lot of other Puppet modules is **stdlib**.  It is a sort of
"utility module" that adds on additional blades to your swiss army knife in
the form of resource types and functions.  It is also a PuppetLabs-supported
module.

Let's start to look at some of the things we might want to do from the
command line with respect to modules...

### List Installed Modules

Puppet Enterprise comes with some modules pre-installed.  To see what's installed,
run the `puppet module list` command as follows on your puppet node (The "Puppet Master"):

```
     puppet module list
```

And you should see something like this:

```
     [root@puppet puppetlabs]# puppet module list
     /etc/puppetlabs/code/environments/production/modules (no modules installed)
     /etc/puppetlabs/code/modules (no modules installed)
     /opt/puppetlabs/puppet/modules
     ├── puppetlabs-pe_accounts (v2.0.2-6-gd2f698c)
     ├── puppetlabs-pe_concat (v1.1.2-7-g77ec55b)
     ├── puppetlabs-pe_console_prune (v0.1.1-9-gfc256c0)
     ├── puppetlabs-pe_hocon (v2016.2.0)
     ├── puppetlabs-pe_infrastructure (v2016.4.0)
     ├── puppetlabs-pe_inifile (v2016.2.1-rc0)
     ├── puppetlabs-pe_java_ks (v1.2.4-37-g2d86015)
     ├── puppetlabs-pe_nginx (v2016.4.0)
     ├── puppetlabs-pe_postgresql (v2016.2.0)
     ├── puppetlabs-pe_puppet_authorization (v2016.2.0-rc1)
     ├── puppetlabs-pe_r10k (v2016.2.0)
     ├── puppetlabs-pe_razor (v1.0.0)
     ├── puppetlabs-pe_repo (v2016.4.0)
     ├── puppetlabs-pe_staging (v2015.3.0)
     └── puppetlabs-puppet_enterprise (v2016.4.0)
```

Notice that there are 3 different directories that puppet is looking for modules in:

* `/etc/puppetlabs/code/environments/production/modules`
* `/etc/puppetlabs/code/modules`
* `/opt/puppetlabs/puppet/modules`   <-- Modules used by PE itself are installed here

Since Puppet Enterprise uses itself to configure itself (e.g. Installing MCollective,
installing the PostgreSQL instance that sits behind PuppetDB, setup of the agent
installer repo, etc.), it makes sense that it would come with modules to help it do that.

If you're really curious, take a look at the installer log, and you'll see where
the puppet installer is using puppet to install and configure other components:

```
     more /var/log/puppetlabs/installer/installer.log
```

### Install a Puppet Module

What if you want to install a Puppet Module from the Puppet Forge?
(Follow along, and go ahead and run each command as we talk about them...)

```
     [root@puppet ~]# puppet module install puppetlabs/stdlib
     Notice: Preparing to install into /etc/puppetlabs/code/environments/production/modules ...
     Notice: Downloading from https://forgeapi.puppetlabs.com ...
     Notice: Installing -- do not interrupt ...
     /etc/puppetlabs/code/environments/production/modules
     └── puppetlabs-stdlib (v4.13.1)
```

Now look at your installed modules again, and you should see it in the list:

```
     [root@puppet ~]# puppet module list
     /etc/puppetlabs/code/environments/production/modules
     └── puppetlabs-stdlib (v4.13.1)
     [snip]
```

Notice that when you installed the Puppet Module, it was automatically
installed within the **production** environemnt.  By default modules
get installed in to the **first element of the modulepath**:

```
     [root@puppet ~]#  puppet config print modulepath
     /etc/puppetlabs/code/environments/production/modules:/etc/puppetlabs/code/modules:/opt/puppetlabs/puppet/modules
```

The modulepath contains colon-separated absolute paths to the locations where
puppet should search for puppet modules.  When making use of a module, the
Puppet Master will look in each directory from left to right until it finds
the module.  It will use the first module found if you have the same module
installed in multiple locations.

What if you want to use that same module in a different environment? And a
different version?

```
     [root@puppet environments]# cd /etc/puppetlabs/code/
     [root@puppet code]# mkdir -p environments/development/modules
     [root@puppet code]# puppet module install --environment development puppetlabs/stdlib --version 4.9.1
     Notice: Preparing to install into /etc/puppetlabs/code/environments/development/modules ...
     Notice: Downloading from https://forgeapi.puppetlabs.com ...
     Notice: Installing -- do not interrupt ...
     /etc/puppetlabs/code/environments/development/modules
     └── puppetlabs-stdlib (v4.9.1)
```

Notice now there are two environment directories (**development** and **production**), and
we've installed two different versions of the **stdlib** module.

```
     [root@puppet code]# tree -L 3 environments
     environments
     ├── development
     │   └── modules
     │       └── stdlib
     └── production
         ├── environment.conf
         ├── hieradata
         ├── manifests
         │   └── site.pp
         └── modules
             └── stdlib

     [root@puppet code]# grep '"version":' environments/*/modules/stdlib/metadata.json
     environments/development/modules/stdlib/metadata.json:  "version": "4.9.1",
     environments/production/modules/stdlib/metadata.json:  "version": "4.13.1",
```

* The development environment has v4.9.1
* The production environment has v4.13.0

What if we want to install a module in the "site modules" directory at /etc/puppetlabs/code/modules ?
Modules installed here would/could be used by any/every agent, regardless of environment.  It's like
a **common** or **global** modules repository.  Just remember that because puppet will search the
modulepath, if an environment's modules directory contains the same module, that will be used instead
of those found further down the module search path, such as in this case.

Let's try installing a different version of stdlib here...

```
     [root@puppet code]# puppet module install --target-dir /etc/puppetlabs/code/modules puppetlabs/stdlib --version 4.12.0
     Notice: Preparing to install into /etc/puppetlabs/code/modules ...
     Error: Could not install module 'puppetlabs-stdlib' (v4.12.0)
       Module 'puppetlabs-stdlib' (v4.13.1) is already installed
         Use `puppet module upgrade` to install a different version
         Use `puppet module install --force` to re-install only this module
```

Oops.  Puppet warns you that you're trying to install a module that is already installed and is newer.  Let's force the install...

```
     [root@puppet code]# puppet module install --target-dir /etc/puppetlabs/code/modules puppetlabs/stdlib --version 4.12.0 --force
     Notice: Preparing to install into /etc/puppetlabs/code/modules ...
     Notice: Downloading from https://forgeapi.puppetlabs.com ...
     Notice: Installing -- do not interrupt ...
     /etc/puppetlabs/code/modules
     └── puppetlabs-stdlib (v4.12.0)
```

Now, let's grep recursively through the files starting at our current-working-dir
and look for the string **"version"**.  This is a quick and easy way to see the
versions of all of the installed modules (every module should have a metadata.json
file with this string.)

```
     [root@puppet code]# grep -r '"version":' .
     ./environments/production/modules/stdlib/metadata.json:  "version": "4.13.1",
     ./environments/development/modules/stdlib/metadata.json:  "version": "4.9.1",
     ./modules/stdlib/metadata.json:  "version": "4.12.0",
```

You can also use the **puppet module list** command to see what modules are
installed for a particular environment, and their versions:

```
     puppet module list --environment=production
     puppet module list --environment=development
     puppet help module
```

We've just seen how to install a module manually.  Later on we will look at how
to use R10K to download and install puppet modules for us.

We will learn more about how to use different environments later.  For now let's
just use the default **production** environment.

before we move on to the next section, let's do a manual puppet run again on
both the puppet master and the agent node.

```
     puppet agent -t
```

Notice that after installing a module, that during the puppet run a whole bunch
of Ruby files are downloaded and cached on the host.  Some of these are the
ruby code for custom facter facts, and others are ruby code for custom types
and providers that were implemented in the module.  Even before using the module,
these bits are downloaded and cached on the agent-side.

Also be aware that if the module is already in use, and you've just upgraded to
a newer version, the new module could behave differently than the old module, and
changes **could** be made.  This is why you should always test a new version of a
module in a different environment (other than production) prior to installing it
in production.  You could test on a test host to make sure any changes are correct,
before promoting the module to the production environment.  This practice of
testing code on a test host and in a test environment will be covered in a later
lab.


---

What other ways might we need/want to envoke puppet from the command line? ...

---

### Puppet from the command line

Explore the 'puppet' command on both the master and the agent.  Run the following commands
and take note of what they do.

```
     puppet help
     puppet help agent
     puppet agent -t
     puppet agent -t --noop
     puppet agent -t --debug
     puppet help config
     puppet config print
     puppet config print manifest
     puppet config print environment
     puppet config print modulepath
     puppet config print certname
```

---

### The Puppet Config File

Let's look at the **puppet.conf** a bit... Here's what a minimal agent-side
puppet.conf would look like:

```
     [main]
        server = puppet.example.com

     [agent]
        environment = production
        certname = agent.example.com
```

When you run puppet from the command line, how does it know what master
to talk to?  By default, puppet will look for a puppetmaster simply
called 'puppet'.  If the puppet.conf doesn't specify the server to talk to,
the agent will use the systems resolver to look for just 'puppet', and if
it's able to resolve that name, it will use the returned IP.

Note:  the puppet.conf conforms to the "INI File" format found historically
on MS-DOS/Windows systems.  It has sections.  Each **section** name
is enclosed in square brackets (e.g. **[main]**)  Each section can contain
any number of **name/value** pairs, which are the settings mapped within that
section.  Name/value pairs are seprated with an equal sign.

Rather than depend on the default behavior, we can explicitly set the
server name in the puppet.conf as follows:

```
     [main]
     server = puppet.example.com
```

Another important config item in the puppet.conf is the **certname**.
The certname typically corresponds to the FQDN of the host, but
you can choose to set it to whatever you like.

```
     [agent]
     certname = agent.example.com
```

The certname is what would be used when the agent contacts the master
for the first time, and submits a cert signing request for it's
SSL cert.  It's the name that would show up when you run a `puppet cert list`
on the master as follows:

```
     puppet cert list --all
```

By default, the agent will run in the 'production' environment. In
Puppet Enterprise, the environment is communicated to the agent by
the PE Console, which functions as an ENC (External Node Classifier).

The environment can be overridden in the puppet.conf as well, though
additional steps are necessary to disable the PE Console ENC.  We will
cover this process in a later lab.  If the PE Console ENC is disabled,
we would then be able to specify the environment on the agent-side
in it's /etc/puppetlabs/puppet/puppet.conf as follows:

```
     [agent]
     environment = production
```

Again, the Puppet Enterprise Console controls what environment is assigned
to a node/agent "Out of the Box".  If you try to set the environment to
something other than what is set in the PE Console, the next puppet run
will cause the environemnt to be set back to what is configured by the
PE Console. For example, setting the environment to **development** as
follows would not work as intended (without some additional steps):

```
     [agent]
     environment = development
```

In fact, depending on the version of puppet you're running, this could
break your puppet agent, requiring you to manually edit the `puppet.conf`
to change the environment back to **production**.

In a later lab we will cover how we can change the environment
via the PE Console, as well as by editing the `puppet.conf`.

### Sections of the puppet.conf

The main, master, and agent sections control where a setting
is applicable.  The **[agent]** section applies to any puppet agent,
including the agent running on the master.  However, many of the settings
you typically find in the [agent] section on an agent, can also be
specified in the [main] or [master] section as well.  Anything you put
in the **[main]** section will apply to both the master and the agent.
Any settings you put in the **[master]** section will apply only to the
puppet master service, and any settings in the [agent] section will
apply just to the puppet agent itself.

If you change the puppet.conf, you should also **restart** the puppet
service so that it re-reads its config.


### Okay, so how do we run puppet manually?

The puppet agent will run automatically in the background
* Every 30 minutes if its cert has been signed, or...
* Every 5 minutes if it's waiting for its cert to be signed

However, you can also run the puppet agent manually.  You're going to
find out this is something you'll do a lot when you're developing
puppet code, and testing that it does what you want.  Simply run:

```
puppet agent -t
```

The **-t** or **--test** option tells the puppet agent to run in "test mode" which
enables several other options you'd want for testing your code: 'onetime',
'verbose', 'ignorecache', 'no-daemonize', 'show_diff', and some others.

Let's talk about what happens when you run **puppet agent -t**.  These
are the key things that happen, and in this order:

  1. Agent reads its puppet.conf (if running manually)
  2. Agent downloads and caches custom types and custom facts for the installed modules from the master
  3. Agent sends **facts** about itself to back to the puppet master (including the environment it is a part of)
  4. Puppet master uses variables (Facts and Puppet variables) along with puppet code to compile catalog for agent
  5. Master sends catalog back to agent to be applied
  6. Agent applies the catalog to the system

If you ever have to troubleshoot why a puppet run is failing, it will be
very useful to be able to identify at what point the run is failing.  I
have seen issues right up front when puppet is calling facter to collect
local systems facts, as well as when puppet is compiling the catalog on
the master, as well as when the catalog is being applied on the agent. In
each case, you'd focus on different things to fix the issue.   We'll talk
more about troubleshooting and tracing puppet runs later...

---

### Okay, Let's get our hands dirty...

---

### Let's make puppet manage the /etc/hosts file

One annoying thing about Vagrant is that if you configure the VM with a hostname,
Vagrant will automatically edit /etc/hosts to make sure the hostname entry is in
there, but it adds the hostname to the '127.0.0.1 localhost' line.  That's not
what we want.  And every time we stop and start the VM with Vagrant, it re-writes
the hosts file this way!

UPDATE:  this **was** the case for older versions of Vagrant, but newer versions
appear to fix this issue. (I'm using v1.8.4 on Mac OS X)  In any case, we will
continue with our example, because it's still a good example...

We want our /etc/hosts to have the following 4 lines, and only these 4 lines:

```
127.0.0.1      localhost
192.168.198.10 puppet.example.com puppet
192.168.198.11 agent.example.com  agent
192.168.198.12 gitlab.example.com gitlab
```

We know that puppet starts at the **site.pp** for every puppet run.  The site.pp
contains node definitions (that tells puppet what code to apply to what nodes),
as well as top-scope variables, and a little bit of puppet code to control
some of puppet's behavior.  The node definition controls what node, or set of
nodes, gets certain puppet code applied to it/them.  This idea of applying certain
bits of puppet code (or classes) to a node is called **Node Classification**.

Later on, we'll see how we can use Hiera to "classify" nodes as well, but for
now we will use good-old node definitions.

Login to the puppet master, become root, and cd in to:
/etc/puppetlabs/code/environments/production/manifests

```
     cd /etc/puppetlabs/code/environments/production/manifests
```

Edit the **site.pp** and add the following at the end of the file in the **node default** section:

```
     vi site.pp
```

Note:  if you don't know **vi** then you may **yum install** the editor of
your choice, and use that instead.

```
node default {

  # remove all unmanaged resources
  resources { 'host': purge => true }

  # add some host entries
  host { 'localhost':             ip => '127.0.0.1', }
  host { 'puppet.example.com':    ip => '192.168.198.10', host_aliases => [ 'puppet' ] }
  host { 'agent.example.com':     ip => '192.168.198.11', host_aliases => [ 'agent' ] }
  host { 'gitlab.example.com':    ip => '192.168.198.12', host_aliases => [ 'gitlab' ] }
}
```

Then do a **puppet agent -t** on both the master and agent.  Because we put this code
in the 'node default' definition, it will be applied to any node that isn't matched
by a more specific node definition (such as 'node agent { }' or 'node puppet { }').

```
     [root@puppet manifests]# puppet agent -t
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for puppet.example.com
     Info: Applying configuration version '1477078262'
     Notice: Finished catalog run in 5.23 seconds
```

Nothing changed?

Remember that in a previous lab we setup our hosts file already.  Puppet looked at it
and saw that it was already correct (as per the config we put in the site.pp) so it
didn't make any changes.

Take a look at the /etc/hosts file on the **puppet** node, and notice it looks the same
as before:

```
127.0.0.1         localhost
192.168.198.10    puppet.example.com    puppet
192.168.198.11    agent.example.com     agent
192.168.198.12    gitlab.example.com    gitlab
```

The host entries may be in a different order, but the same 4 entries should be there, and hopefully none others.
The `purge => true` option we used in our code tells puppet to remove any un-managed host entries.  If you add
a new host entry outside of the puppet code, the next time puppet runs it will remove it.

Try this:  add the following to /etc/hosts manually using your favorite text editor:

```
1.2.3.4 unmanaged-host
```

Then run `puppet agent -t` and see what Puppet does...


```
[root@agent ~]# echo '1.2.3.4 unmanaged-host' >> /etc/hosts
```

Notice that we can resolve that name now:

```
[root@agent ~]# getent hosts unmanaged-host
1.2.3.4         unmanaged-host
```

Now run Puppet and watch what it does...

```
[root@agent ~]# puppet agent -t
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for agent.example.com
Info: Applying configuration version '1477078872'
Notice: /Stage[main]/Main/Node[default]/Host[unmanaged-host]/ensure: removed
Info: Computing checksum on file /etc/hosts
Notice: Finished catalog run in 0.40 seconds
```

It removed the rogue entry we added! Nice!

```
[root@agent ~]# cat /etc/hosts
# HEADER: This file was autogenerated at 2016-10-21 19:41:12 +0000
# HEADER: by puppet.  While it can still be managed manually, it
# HEADER: is definitely not recommended.
127.0.0.1    localhost
192.168.198.10    puppet.example.com    puppet
192.168.198.11    agent.example.com    agent
192.168.198.12    gitlab.example.com    gitlab
```

So we've seen that Puppet recognized a change to /etc/hosts and un-did that change.

### The host resource

The `host { }` definition is called a **puppet resource type**, or just
the 'host resource' or simply a 'type'.  Depending on what documentation your're reading,
the author may call it a 'type' or a 'resource' or a 'resource type'.  It's all
the same thing.  To make things even more confusing (or interesting!), you can even define your
own custom resource types with the 'define' function.

For a complete list of all of the available built-in resource
types, see this page:

<http://docs.puppetlabs.com/puppet/latest/reference/type.html>

You may also use the **puppet describe** command to get info about any
resource from the command line.  For example, if you want to see the
parameters/attributes available to the host resource type, simply run:

```
     puppet describe host
```

Compare that with the **types reference** here:  <http://docs.puppetlabs.com/puppet/latest/reference/type.html#host>

If you want to see the current state of a particular resource type,
you can use the **puppet resource** command.  For example, if you want
to see all host resources on a system (not necessarily managed by
the puppet master), you could do:

```
     puppet resource host
```

...and you'd see something like this:

```
     host { 'gitlab.example.com':
       ensure       => 'present',
       host_aliases => ['gitlab'],
       ip           => '192.168.198.12',
       target       => '/etc/hosts',
     }
     host { 'agent.example.com':
       ensure       => 'present',
       host_aliases => ['agent'],
       ip           => '192.168.198.11',
       target       => '/etc/hosts',
     }
     host { 'localhost':
       ensure => 'present',
       ip     => '127.0.0.1',
       target => '/etc/hosts',
     }
     host { 'puppet.example.com':
       ensure       => 'present',
       host_aliases => ['puppet'],
       ip           => '192.168.198.10',
       target       => '/etc/hosts',
     }
```

If you want to see the resource state for a particular user,  you
might run:

```
     puppet resource user root
```

...and you'd see something like this:

```
     user { 'root':
       ensure           => 'present',
       comment          => 'root',
       gid              => '0',
       home             => '/root',
       password         => '$1$v4K9E7xJ$gZIhJ5Jtqg5ZgZXeqSShd0',
       password_max_age => '99999',
       password_min_age => '0',
       shell            => '/bin/bash',
       uid              => '0',
}
```

Again, these resources arn't necessarily managed by the puppet master (but
they could be).  We just don't know.  When you execute puppet in this way,
you're simply querying the state of the system at that moment in time.
This is how Puppet **sees** the state of the system.  It's like looking
through Puppet's eyes, or how Puppet would see the system if it looked.

It's useful to show resources in this way when you are writing some puppet
code to enforce a config state, as you can easily cut-and-paste the code
for the resource into a manifest and modify it accordingly.

To further illistrate this, let's look at a service resource, and how the
output of 'puppet resource service puppet' changes as we stop/start or
enable/disable a service.

Let's see the current state of the puppet service on our puppetmaster:

```
     [root@puppet ~]# puppet resource service puppet
     service { 'puppet':
       ensure => 'running',
       enable => 'true',
     }
```

We see that the puppet service is both enabled, and running.

We can confirm that with the **systemctl** (on RHEL7) command as well:

```
     [root@puppet ~]#  systemctl status puppet
     ● puppet.service - Puppet agent
        Loaded: loaded (/usr/lib/systemd/system/puppet.service; enabled; vendor preset: disabled)
        Active: active (running) since Mon 2016-11-14 18:17:39 UTC; 5h 11min ago
      Main PID: 8308 (puppet)
        CGroup: /system.slice/puppet.service
                └─8308 /opt/puppetlabs/puppet/bin/ruby /opt/puppetlabs/puppet/bin/puppet agent --no-daemonize
```

Now, let's disable the service, but leave it running, and then show the output
of the puppet resource again.  Notice that the **enabled** attribute has changed to **false** now.

```
     [root@puppet ~]# systemctl disable puppet
     Removed symlink /etc/systemd/system/multi-user.target.wants/puppet.service.
```

Now check the puppet service resource:

```
     [root@puppet ~]# puppet resource service puppet
     service { 'puppet':
       ensure => 'running',
       enable => 'false',
     }
```

Let's stop the service, and then look again.  Notice that the **ensure** attribute has changed to **stopped** now.

```
     [root@puppet ~]# systemctl stop puppet
```

Now check the puppet service resource again:

```
     [root@puppet ~]# puppet resource service puppet
     service { 'puppet':
       ensure => 'stopped',
       enable => 'false',
     }
```

Now re-enable, and re-start the service, and look again...

```
     [root@puppet ~]# systemctl enable puppet
     Created symlink from /etc/systemd/system/multi-user.target.wants/puppet.service to /usr/lib/systemd/system/puppet.service.
     [root@puppet ~]# systemctl start puppet
```

Now let's check again...

```
     [root@puppet ~]# puppet resource service puppet
     service { 'puppet':
       ensure => 'running',
       enable => 'true',
     }
```

...and we are back to where we started.  Notice that the **puppet resource service puppet** command
simply shows the current state of that resoure at that moment in time.

---

### Okay, getting back to the host resource...

Remember that `puppet describe host` will show you all of the available
parameters for a host resource.

A host resource has a title, and several parameters/attributes.  The first item
in the host resource definition is the title (e.g. `puppet.example.com`) and the
subsequent name/value pairs are the parameters (e.g. `ip => 192.168.198.10`).
In this context, the term **attributes** may be better than **parameters**, as we
also use parameters when defining classes, and class parameters and type
attributes are two different things.

Note that the **host_aliases** attribute accepts an array (enclosed in square brackets `[ ]`),
while the **ensure** and **ip** attributes accept a string.

If you haven't already, take a look at the contents of the /etc/hosts file:

```
cat /etc/hosts
```

And we should see something like this...

```
     # HEADER: This file was autogenerated at 2016-01-19 18:25:19 +0000
     # HEADER: by puppet.  While it can still be managed manually, it
     # HEADER: is definitely not recommended.
     127.0.0.1       localhost
     192.168.198.10  puppet.example.com  puppet
     192.168.198.11  agent.example.com   agent
     192.168.198.12  gitlab.example.com  gitlab
```

At this point, every existing line in /etc/hosts is managed by puppet.
The host resource type manages each line individually, and we've added
each of the existing lines with puppet code.

Also, remember that we added the following bit of code to remove
any host entry that isn't managed by puppt:

```
  resources { 'host': purge => true }
```

...in other words, we now have a fully-managed /etc/hosts file.  If
someone comes along and adds a line to the /etc/hosts file without
realizing Puppet is managing it, the next time Puppet runs, they
will be in for a surprise!

The nice thing about managing every entry of /etc/hosts with puppet
is if our hosts file is deleted or corrupted in some way, Puppet will
re-write it entirely, and we dont have to worry about any entries
missing.  Let's try that on the **agent** node...

```
[root@agent ~]# rm -f /etc/hosts
```

Note:  We need at least one entry for the Puppet Master itself, otherwise the agent wont know what IP to talk to.
This is a great reason to be using DNS (so that our puppet master's IP would **always** be resolvable).

```
[root@puppet ~]# echo '192.168.198.10 puppet.example.com puppet' > /etc/hosts
[root@puppet ~]# puppet agent -t
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for puppet.example.com
Info: Applying configuration version '1479166434'
Notice: /Stage[main]/Main/Node[default]/Host[localhost]/ensure: created
Info: Computing checksum on file /etc/hosts
Notice: /Stage[main]/Main/Node[default]/Host[agent.example.com]/ensure: created
Notice: /Stage[main]/Main/Node[default]/Host[gitlab.example.com]/ensure: created
Notice: Applied catalog in 13.83 seconds
```

See how Puppet added back all of the missing host entries?

```
[root@puppet ~]# cat /etc/hosts
# HEADER: This file was autogenerated at 2016-11-14 23:34:05 +0000
# HEADER: by puppet.  While it can still be managed manually, it
# HEADER: is definitely not recommended.
192.168.198.10  puppet.example.com  puppet
127.0.0.1       localhost
192.168.198.11  agent.example.com   agent
192.168.198.12  gitlab.example.com  gitlab
```

Go ahead and try adding some additional entries to /etc/hosts, and then run puppet again.  You should see puppet remove them:

I added this line:

```
1.2.3.4   foo.example.com foo
```

Then puppet ran an removed it:

```
     [root@agent /]# puppet agent -t
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for agent.example.com
     Info: Applying configuration version '1472432976'
     Notice: /Stage[main]/Main/Node[default]/Host[foo.example.com]/ensure: removed
     Info: Computing checksum on file /etc/hosts
     Notice: Finished catalog run in 0.55 seconds
```

If you dont see Puppet remove the entry, it could be that Puppet wasn't able to parse the hosts file, and
decided to silently do nothing.  I've observed that with PE 2016.4.0, if you have any whitespace before
your entry, Puppet will just ignore it, even though the system can still resolve it.  This is a bug if
you ask me, as Puppet is allowing an out-of-compliance file to exist without saying anything.  Is this
better or worse than what Puppet used to do?  In older versions of Puppet, if there was invalid content
in your /etc/hosts that Puppet couldn't parse, it would give up after having already started to re-write
the file with its header.  This left the /etc/hosts file truncated without any entries at all.  This
could obviously break the system, if critical entries exited in /etc/hosts that the application relies on.

Anyway...

---

Okay, that's enough for now.  Take a break, and let's move on to the next lab...

---

Let's re-visit what our goals were in this section:

* Puppet config file:  we looked at some of the important config
* Puppet from the command line: we used 'puppet help', 'puppet agent -t', etc.
* Puppet code basics: we got a little taste of puppet code
* Tieing code to a node via "classification": we used the default node definition in the `site.pp`

---

Continue to **Lab #6** --> [Practice doing some puppet code, and puppet runs](06-Puppet-Code-Practice.md#lab-6)

---

### Further Reading

For more info on how the site.pp **main manifest** is configured and used,
see the PuppetLabs docs at:

<https://docs.puppetlabs.com/puppet/latest/reference/dirs_manifest.html>

For more info on how the modulepath is used:

<https://docs.puppetlabs.com/puppet/latest/reference/dirs_modulepath.html#loading-content-from-modules>

---

<-- [Back to Contents](/README.md)

---

<-- [Back](05-Puppet-Config-and-Code.md#lab-5)

---

### **Lab #6** - Some More Puppet Code Practice

---


### Overview

Time to complete:  60 minutes

The goal of this section is **NOT** to learn how to code.  We will write some
puppet code, but the **primary goal** of this lab is to learn:

 - More about tieing code to a node (**Node Classification**)
 - Where do you put your code on the master?
 - Why put code in a `class { }` ?
 - Defining code vs Declaring code
 - What is **the catalog** ?
 - How does puppet process your code files (manifests) and build the catalog?
 - Notice that we are editing code files directly on the master (Bad? Why?)
 - Notes on the `site.pp` and **top-scope variables** (will become important later)

### Node Classification

Remember from Lab #5, we talked about **classes** and **node classification**?

To review:

  - a class is simply a named block of puppet code that we can refer to by the class name.
  - we "tie" a class to a node through **node classification**, which simply says "apply this **class** to this **node**"

### The Catalog

We're going to start using the term **catalog** in the following sections, but what is the catalog?
Simply put, it's just the **compiled** version of your puppet code (manifests).  When the puppet
agent runs, it sends all of the facts about the system it's running on back to the puppet master.
All of the puppet code is sitting on the puppet master, and the master takes all of the built-in
puppet variables, facts, and code (conditional logic, functions, etc.) and compiles them into a
static set of instructions to be executed on the agent side.  All of these instructions are
executed on the agent side in a certain order as determined by the dependencies set forth in
your puppet code.  You'll learn more about ordering with dependencies later on, but for now,
just remember that the catalog is just all of your puppet code resolved down to the specific
instructions for the specific node it's being compiled for.

Another important concept to grasp is that when the catalog is compiled, all of the functions
and conditional logic you have in your manifests are evaluated on the master side, not the agent
side.  Puppet variables such as system facts are interpolated at this time as well.  All of the
decision-making within a manifest has already happened when the agent receives its catalog, so the
agent just executes the instructions it receives in the correct order.

Understanding what happens on the master side vs the agent side will become very important
later on, so just keep this in mind for now, and hopefully some questions will be triggerd
later on as we work through the tutorial.

### Defining Classes and Declaring Classes

First, we need to understand the difference between **defining** a class and **declaring** a class.

**Defining Classes**

- named blocks of Puppet code that are usually stored in modules for later use
- are NOT applied until they are invoked by name (declared)
- they can be added to a node’s catalog by either declaring them in your manifests
- or assigning them from an ENC (external node classifier.)

We've not yet talked about **Hiera**, an add-on to Puppet which is included in
Puppet Enterprise out-of-the-box.  Hiera, though not technically an ENC, can be
used like one.  We will learn more about Hiera in a later lab.  For now, we will
use node definitions along with the include or resource-style class declaration (with params)

Also, the PE Console comes configured OOTB as an ENC.  In a later lab
we will cover how this works, and we'll also disable this functionality in
order to enable Hiera to be used like an ENC.

**Declaring a class in a Puppet manifest**

- adds all of its resources to the catalog.

You can declare classes

- in node definitions at top scope in the site manifest (site.pp)
- in other classes (e.g. profile classes when using the Roles & Profiles paradigm)
- via an ENC (or use Hiera as an ENC)

The official PuppetLabs Docs talk at length about [Declaring Classes](https://docs.puppet.com/puppet/3.8/reference/lang_classes.html#declaring-classes)

Example of declaring a class at top-scope (in the site.pp) using the **include** statement:

```puppet
    include common_hosts
```

The class **common_hosts** would have to be defined in a file named `common_hosts.pp` and be found within the puppet codebase.
The **include** simply reads the manifest file, and declares (adds to the catalog) the resources in that class.

The nice thing about the **include** is that if the contained class has already been declared somewhere else, it wont be added to the catalog again (which would be a puppet error.)  Resources can only be declared **once** and only once.  If you try to declare the same resource (identified by its name) two or more times, puppet will throw an error.

If you put this include statement in your `site.pp`, outside of any
node definition, it would apply to every node.  (Note:  this is not the
same as putting it in the `node default { }` section, which only applies if
no other node definition matches.)

There is another way of declaring a class called **Resource-like Declarations** which we will ignore for now, as this style of declaration really wont be needed once we get to our section on **Hiera**.

You can read more about [Resource-like Declarations](https://docs.puppet.com/puppet/3.8/reference/lang_classes.html#using-resource-like-declarations) in the PuppetLabs documentation.

### More about Node Classification

Example of declaring a class for a specific node:

```puppet
    node foo_server {
      include foo_server_code
    }
```

This would look for a class called **foo_server_code** and declare it
ONLY for the node named **foo_server**.

Note:  Do not use the dash character in class names.  Underscore is allowed.

You can also specify multiple nodes in a single node definition, or even
use a regular expression to match multiple nodes.
You can read more about [Node Definitions](https://docs.puppetlabs.com/puppet/3.8/reference/lang_node_definitions.html)
in the official PuppetLabs docs.

...but guess what?  You probably wont care once you're introduced to **Hiera**, as
we'll be doing all of our node classification, and parameter passing via Hiera.

**Hiera Quick Preview**

Example of declaring a class via Hiera:

The `site.pp` would contain a call to Hiera like this:

```puppet
    hiera_include('classes')
```

Although node definitions could still be used, the power of Hiera is that you can
have puppet search a hierarchy of yaml files looking for a set of classes to be
applied on a per-node basis, as well as many other levels (or groupings).  For
example, you could specify a particular class be declared for all Linux systems,
or all Solaris systems, or all systems at a particular location, or that are a part
of a particular department.  The call to hiera_include('classes') will build up
a list of classes to apply for a node, but it can assemble this list of classes from
all levels throughout your hierarchy if you've declared classes at multiple levels.

With your Hiera config (`hiera.yaml`) already setup, you'd need to put somewhere within
your hierarchy a yaml file containing:

```yaml
    classes:
      - bar_server_code
      - some_other_class
```

This would cause the two classes to be declared for whatever level in the hierarchy
they have been declared.  For example, if you put this in a "node-level yaml" file,
they would get declared for the corresponding node.  If you put it in a yaml file
for a specific OS, they would get declared only for systems running that particular OS.

For now, we will ignore Hiera, but will come back to it in a later lab.  Hiera is
used almost universally these days, so it's not something to forget about.  Have
patience, and we'll get to it very soon!

### Let's write some code

Okay, let's shift gears now... We've talked a lot about how to declare classes, but
it may not make any sense until you start writing some code.

So let's start looking at some code to see if we can make these ideas make more sense...

Let's do a quick survey of what Puppet can do.

What if you want to install some packages?

```puppet

  package { 'bind-utils':
    ensure => 'installed'
  }

  package { 'dstat':
    ensure => 'installed'
  }

  package { 'git':
    ensure => 'installed'
  }

  package { 'tcpdump':
    ensure => 'installed'
  }

  package { 'net-tools':
    ensure => 'installed'
  }

```

This bit of code will install the latest version of these 5 packages, and
puppet will **NOT** track the versions installed.  If a newer version becomes
available in the host's software repos (e.g. configured yum repos), then
puppet wont notice, and will NOT do anything.  If you want puppet to install
the latest package, and track the version, and update when new version are
released, then you should use:

```puppet
    ensure => 'latest'
```

...in your package resource.

You can also specify a version of the package if you want to pin
the version to a very specific release, but this only works for systems
that have a 'versionalble' provider.  Yum allows this, but some older
package systems such as 'up2date' do not.

Let's add this code to a manifest file, and wrap it in a class definition.
A class allows us to refer to some code by name, and optionally pass in
parameters that the code within the class could access to further control
what the code does.

### Install Some Packages

Let's configure puppet to make sure certain packages are installed on our agent node.

Login to your puppet master node and become root

```
     cd /etc/puppetlabs/code/environments/production/manifests
     vi common_packages.pp
```

We will be adding this new manifest in the **production** environment in the **manifests** directory.

Notice that Puppet looks for code in the manifests directory based upon the environment it is a part of,
and the 'manifest' configuration value.

```
     [root@puppet]# puppet config print environment
     production

     [root@puppet]# puppet config print manifest
     /etc/puppetlabs/puppet/environments/production/manifests
```

In your common_packages.pp file add your code wrapped in a class with the same
name as the file, less the .pp extension, so like this:

```puppet
class common_packages {

  package { 'bind-utils':
    ensure => 'installed'
  }

  package { 'dstat':
    ensure => 'installed'
  }

  package { 'git':
    ensure => 'installed'
  }

  package { 'tcpdump':
    ensure => 'installed'
  }

  package { 'net-tools':
    ensure => 'installed'
  }

}
```

We've now **defined** a class, but this code will not do anything until we pin it to a node.
As soon as you save the manifest, puppet will *"see"* it on its next run.  It will know that
the class has been **defined**, but wont apply the code to any node until we **declare**
the class for a node.  Remember that thing called 'Node Classification'?

Let's classify the **agent node** with the **common_packages class**.

We will edit our `site.pp`

```
     vi site.pp
```

At the end of the `site.pp` add the following:

```puppet
node 'agent.example.com' {
   include common_packages
}
```

Save it, and then run 'puppet agent -t' on your agent VM, and you should see the packages get installed...

```
     [root@agent ~]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for agent.example.com
     Info: Applying configuration version '1479239887'
     Notice: /Stage[main]/Common_packages/Package[bind-utils]/ensure: created
     Notice: /Stage[main]/Common_packages/Package[dstat]/ensure: created
     Notice: /Stage[main]/Common_packages/Package[git]/ensure: created
     Notice: /Stage[main]/Common_packages/Package[tcpdump]/ensure: created
     Notice: Applied catalog in 19.05 seconds
```

Did you notice that only some of the packages were installed?  If you do a `yum info net-tools`
you'll notice that it was already installed, so puppet didn't do
anything for that package.  Note:  net-tools is required for puppet, so when we
installed puppet, that package was automatically installed.

Now that all 5 of these packages have been installed, if you run puppet again, no
additional changes will be made.  The host is as it should be.

### The default node

Now, I want to re-visit something we mentioned earlier.  Remember, the **node default**
only applies to a host **if no other** node definition matches it.  We've just added
a node definition for 'agent.example.com', so the default node definition will no longer apply.
The order of the node definitions doesn't matter.  To prove that this is the case,
go ahead and edit your `/etc/hosts` file on the agent.  DELETE the line with gitlab
on it, and then re-run 'puppet agent -t'.   Notice that puppet didn't re-add the line.

Let's take this opportunity to pull the code out of the 'node default' definition,
and put it in another manifest file called `common_hosts.pp`

Wrap that code in a class definition like this:

```puppet
class common_hosts {

  # remove all unmanaged resources
  resources { 'host': purge => true }

  # add some host entries
  host { 'localhost':             ip => '127.0.0.1', }
  host { 'puppet.example.com':    ip => '192.168.198.10', host_aliases => [ 'puppet' ] }
  host { 'agent.example.com':     ip => '192.168.198.11', host_aliases => [ 'agent' ] }
  host { 'gitlab.example.com':    ip => '192.168.198.12', host_aliases => [ 'gitlab' ] }

}
```

...save your new **common_hosts.pp** and let's, go back in to our site.pp and include it
for the agent node again.

Remove the host resources from the site.pp, and instead include the new class
contained in your new common_hosts.pp as follows:

```puppet
node default {
   include common_hosts
}

node 'agent.example.com' {
  include common_hosts
  include common_packages
}
```

Now re-run puppet agent -t on your agent node, and notice that the gitlab entry was added back.

```
     [root@agent ~]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for agent.example.com
     Info: Applying configuration version '1479240149'
     Notice: /Stage[main]/Common_hosts/Host[gitlab.example.com]/ensure: created
     Info: Computing checksum on file /etc/hosts
     Notice: Applied catalog in 0.69 seconds
```

The important point we're illustrating here is that the default node definition ONLY applies
if NO OTHER node definition matches.

Also, since we are including the common_hosts in both the default and the agent node
definitions, we could choose to move that include out of both so that it applies globally,
even if we add new hosts in the future with their own node definitions.

Let's edit our site.pp to look like this:

```puppet
node default {
}

node agent {
   include common_packages
}

include common_hosts
```


Notice now that the default node definition is empty, and only the agent
node definition includes the common_packages class.  Any and every host
will get the common_hosts class.  Let's run 'puppet agent -t' again
to prove that our code still works.

Again, remove the gitlab line in /etc/hosts on both agent and master, and
run puppet again, and you should see puppet add it back on both.

We are going to eventually want git on the master, so let's make one final
edit to our site.pp and move the common_packages include outside of the
node agent definition, leaving both 'node default' and 'node agent' definitions
empty.  Let's also add an empty node definition for the puppet master in case we want
to classify it uniquely later.  Finally, let's also re-order the code in to
a more intuitive order.  It makes no difference to puppet, but I think it's
more intuitive to humans this way:

```puppet
#
# Global - All code outside a node definition gets applied to all nodes
#

include common_packages
include common_hosts

#
# Node-specific
#

node 'puppet.example.com' {
}

node 'agent.example.com' {
}

#
# Default - if no other Node-specific definition matched
#

node default {
}
```


Next time puppet runs on the puppet master, you'll notice that those packages get installed.

### Top-level Scope

Note:  node definitions can only be made at the top-level in the site.pp

**Top-Scope** is another way of saying **Globally-visible**.

Any variable declared in the `site.pp` is automatically a top-scope variable.
Top-scope variables are accessible throughout all of your puppet code by referencing them with '$::varname'

New in Puppet 3.8 (and later) is that any manifest at the top level is automatically read,
in additional to the `site.pp`, though I would recommend having ONLY a site.pp, and not
introducing other manifests at the top level as this can cause ordering issues if
you are reading in hiera data to set top-scope variables, and trying to use those
variables in other manifests at the top-scope.

### Affecting resource parameter defaults

Something else to simplify the code for our common_packages class:  Notice that we
have several packages, and we are doing the "ensure => 'installed'" for all of them.
To reduce the duplicated code, we could set a default attribute value for the Package
type like this:

```puppet
Package { ensure => 'installed' }
```

This default would apply to any/every other package resource we declare within
the manifest. We can still override the default on a per-resource basis if we
want to.  So our code could look like this:

```puppet
class common_packages {

  Package { ensure => 'installed' }

  package { 'bind-utils': }
  package { 'dstat': }
  package { 'git': }
  package { 'tcpdump': }
  package { 'net-tools': }

}
```

Notice that there is a definite difference when you use the lower-case version of
the resource type, vs using the upper-cased version.  Lower case declares the
resource, while uppercase refers to the resource type, but does NOT declare it.

When we say:

```puppet
   Package { ensure => 'installed' }
```

...what we are really saying is:  for every package resource declared, take these
attributes as defaults unless they are overridden by the individual resource declaration.

We could even put the Package ensure installed bit in the site.pp, and then it would
be a global default for package resources everywhere.  Pretty cool, huh?

We can override the default we just set by specifying the version string to the ensure parameter.
Example of pinning to a specific version/release of a package:

```puppet

  if ( $::operatingsystemmajrelease == '6' ) {
    package { 'nc': ensure => '1.84-24.el6' }
    package { 'nmap': ensure => '5.51-4.el6' }
  }

  if ( $::operatingsystemmajrelease == '7' ) {
    package { 'whois': ensure => '5.1.1-2.el7' }
  }

```

We also threw in a conditional statement there that checks the **OS Major Release** version **fact**.
If we are running an EL6 system, the **nc** and **nmap** packages will get installed to the specific version we've specified.
If we are running an EL7 system, the **whois** package will get installed to the specific version we've specified.
(This all assumes that the versions we've specified are available in our package repository.)

The slightly annoying thing here would be that if you were maintaining this package
across multiple platforms and versions, you'd have to manage the version/release
strings for each. For example, for the **whois** package, we might have several
different versions for each platform:

```
     5.1.0-1.el5
     5.1.1-3.el6
     5.1.1-2.el7
```

It's annoying that the platform (el5, el6, or el7) must be included in the
release for example.  If you don't include it, Puppet will not recognize the
version.  It would be preferable to be able to just say '5.1.1' and have the
provider figure out if we're running that version, but this wouldn't help us
if the '5.1.1' version isn't available on EL5, and we want to install '5.1.0'
there.

One solution to this problem could be using Hiera along with **$::operatingsystemmajrelease**
in the hierarchy, and then specifying the specific package version string for
each in the appropriate yaml file.  We will look at configuring Hiera in the
next lab, and show how to do this.

---

Continue to **Lab #7** --> [Config Hiera](07-Config-Hiera.md)

---

Further Reading:


[The Puppet Cookbook](http://www.puppetcookbook.com/) has many examples for
doing simple things, and is a nice place to visit to get code fragments to
accomplish many of the common things you'll want to do.

[The Puppet Language Reference about Classes](https://docs.puppetlabs.com/puppet/latest/reference/lang_classes.html)

[More about Catalog Compilation](https://docs.puppet.com/puppet/latest/reference/subsystem_catalog_compilation.html)

---

<-- [Back to Contents](/README.md)

---

<-- [Back](06-Puppet-Code-Practice.md#lab-6)

---

### **Lab #7** - Configure Hiera

---

### Overview

Time to complete:  60 minutes

In this lab we will:

 - Configure Hiera
 - Use Hiera to classify our nodes

### What is Hiera?

Hiera is a database that you can query in your Puppet code.  Hiera stores
data in YAML or JSON formatted flat text files.  We refer to the data as
**Hiera Data** and Hiera allows us to store data in a hierarchical fashion
such that a query can return different results depending on the value of
system facts or other puppet variables.


### Why is Hiera useful, and why do we want to use it?

The two key reasons we like to use Hiera are:

  - Node Classification (Potentially much nicer than using **node** definitions in the site.pp)
  - Module Portability (Keeping site/company-specific data separate from the actual code)

Hiera, though not technically an [External Node Classifier](https://docs.puppet.com/guides/external_nodes.html)
(ENC), can be used like one.  Not only can you
declare classes inside a node definition (as we've seen in previous labs) you
can use Hiera to tie a class to a node.  You can also use Hiera to pass class
parameters in to a class (including classes within modules).  Or you can use
Hiera to store arbitrary data as name/value pairs, or even data as an array
or hash or hash of hashes, etc.

We will see how to do all three of these things in the following lab...

1. Classify a node with Hiera
2. Pass class params to a class with Hiera (automatic class parameter lookup)
3. Define and use arbitrary data in Hiera

But first we need to configure Puppet to use Hiera...

### The Hiera Config File

Puppet knows about Hiera, and you'll see in a bit that there are some hiera
function calls we can use in our puppet code to search our Hiera data.  But
first, we need to configure the main Hiera config file:

```
     [root@puppet ~]# puppet config print hiera_config
     /etc/puppetlabs/puppet/hiera.yaml
```

Puppet comes with an example **hiera.yaml** as follows:

```
     [root@puppet ~]# cd /etc/puppetlabs/puppet
     [root@puppet puppet]# cat hiera.yaml
     ---
     :backends:
       - yaml
     :hierarchy:
       - "nodes/%{::trusted.certname}"
       - common

     :yaml:
     # datadir is empty here, so hiera uses its defaults:
     # - /etc/puppetlabs/code/environments/%{environment}/hieradata on *nix
     # - %CommonAppData%\PuppetLabs\code\environments\%{environment}\hieradata on Windows
     # When specifying a datadir, make sure the directory exists.
       :datadir:

```

In addition to the **'yaml'** backend option, you can use **'json'** if you like.
*See Also:  [Hiera Data Sources](https://docs.puppetlabs.com/hiera/3.2/data_sources.html#yaml) )

The **hiera.yaml** provided is actually fully usable as-is except for that lack of **datadir** definition.  Let's define that, and make some other minor changes as follows:

1. Set the **datadir** to the `%{environment}/hieradata` directory as we will eventually have multiple environments
2. Let's also add a couple other levels to our hierarchy for **role** and **location** (we will use them later)
3. Also change **nodes** (plural) to just **node** (singular) to be consistant

That should be good to get us started looking at how Hiera can be used...so our hiera.yaml
will look like this:

```
[root@puppet manifests]# cd /etc/puppetlabs/puppet
[root@puppet puppet]# cp hiera.yaml hiera.yaml.orig
[root@puppet puppet]# vi hiera.yaml
[root@puppet puppet]# cat hiera.yaml
---
:backends:
  - yaml

:hierarchy:
  - "node/%{::trusted.certname}"
  - "role/%{::role}"
  - "location/%{::location}"
  - common

:yaml:
  :datadir: "/etc/puppetlabs/code/environments/%{environment}/hieradata"

```

Go ahead and edit your hiera.yaml if you haven't already.


### There's just one hiera.yaml

Notice that there is just one **hiera.yaml** for all of our puppet environments,
**NOT** separate hiera.yaml files per-environment.  This is important to
realize, because in a later lab we will move our hiera.yaml under Git control,
and it will be sitting within the production environment.  This could become
confusing as we branch off other environments, which would also contain
the hiera.yaml.  We might be tempted to edit the hiera.yaml differently in
each environment, but this will not work as expected.  Puppet will always
look for the hiera.yaml where the **hiera_config** tells it to look--it's
not a dynamically-evaluated config parameter, so you can't include a variable
in the config and expect puppet to re-evaluate that config item every run.
Remember that we have to re-start Puppet after editing the hiera.yaml? So we
shouldn't expect puppet to automatically re-read different versions of the
hiera.yaml every time it runs.  Let's **test this** later to verify that this
is indeed the case.  For now don't worry about it... just remember that there's
just one hiera.yaml, and the same one is used by every puppet run reguardless
of the environment.

**Key Points:**

- Puppet uses just one hiera.yaml for all environments
- Each environment **can** have its own unique Hiera Data



### Setup the Hiera Data directory(s)

We've configured our hiera.yaml, but we still need to create the **datadir** as
we've defined it within the hiera.yaml.  This is where Hiera will look for
data when we make use of any of the hiera functions calls within our puppet code.

Make sure you're still sitting in **/etc/puppetlabs/code** and make the following directories:

```
     [root@puppet code]# pwd
     /etc/puppetlabs/code
     [root@puppet code]# mkdir -p environments/production/hieradata
     [root@puppet code]# mkdir -p environments/production/hieradata/node
     [root@puppet code]# mkdir -p environments/production/hieradata/role
     [root@puppet code]# mkdir -p environments/production/hieradata/location
     [root@puppet code]# tree -L 2 environments/production
     environments/production
     ├── environment.conf
     ├── hieradata
     │   ├── location
     │   ├── node
     │   └── role
     ├── manifests
     │   ├── common_hosts.pp
     │   ├── common_packages.pp
     │   └── site.pp
     └── modules
         └── stdlib

```

### Update the puppet.conf to know about the hiera.yaml

Even though the default value for **hiera_config** is correct, let's put it
in the puppet.conf **[main]** section just to have it explicitely defined.
This makes it more obvious to someone how puppet knows to read the hiera.yaml
in that location.  So the [main] section of your puppet.conf (on the Puppet
Master) should look like this (I've only added one new line for **hiera_config**
and the rest of these options should have already been there):

```ini
     [master]
     node_terminus = classifier
     storeconfigs = true
     storeconfigs_backend = puppetdb
     reports = puppetdb
     certname = puppet.example.com
     always_cache_features = true
     hiera_config = /etc/puppetlabs/puppet/hiera.yaml
```

Again, this is the default, but we will change the location of the hiera.yaml
in a later lab (when we move our code under Git control) so let's get the default
in there now, so later on when we change it we can easily see what we're changing
it **from** and changing it **to**.

### Restart the Puppet Master

After making any changes to the **puppet.conf** and/or the **hiera.yaml**,
you must **re-start** the Puppet Master so that it re-reads those config files.

Each time you run puppet from the command line, it will re-read the puppet.conf,
but keep in mind that the Puppet Master reads it too, and it's running daemonized
in the background.  This is why we need to restart the service so that the
Puppet Master re-reads the [main] and [master] sections of the puppet.conf.

Same deal for the hiera.yaml.  Even though you can run hiera from the command
line to test data lookups, and it re-reads the hiera.yaml each time, the Puppet
Master does not.  It reads it only once when it starts, and keeps that snapshot
of the config it memory for fast access.


```
     systemctl restart pe-puppetserver
```


### So are we ready to use Hiera yet?

**Yes!**   Here's what we've done to get to this point:

1. Created a **hiera.yaml**
2. Created a **hieradata/** directory within the existing production environment directory
3. Created a few hiera data sub-directories to align with our hiera.yaml
4. Updated the **puppet.conf** with the **hiera_config** option and value
5. **Restarted** the Puppet Master so that it would re-read the puppet.conf and hiera.yaml

### Using Hiera in your Puppet Code

Again, there are 3 ways we can use Hiera:

1. Classify a node
2. Pass class params in to a class (also called automatic parameter lookup)
3. Define and use arbitrary data as puppet variables

Let's start looking at the first one:  Classify a node

### Node Classification with Hiera

Instead of using node definitions in the **site.pp**, let's use the Hiera
function call **hiera_include('classes')** as follows...

```puppet
    hiera_include('classes')
```

We can simply add that one line to the end of our site.pp and Puppet will search
through every level of our Hiera data for the **key** with the name **classes**
then take the **value** and append it on a list of classes.  The end result will
be a list of classes to apply to the node.

Although node definitions could still be used, the power of Hiera is that you can
have puppet search a hierarchy of yaml files looking for a set of classes to be
applied on a per-node basis, as well as many other levels (or groupings).  For
example, you could specify a particular class be declared for all Linux systems,
or all Solaris systems, or all systems at a particular location, or that are a part
of a particular department.  The call to hiera_include('classes') will build up
a list of classes to apply for a node, but it can assemble this list of classes from
all levels throughout your hierarchy if you've declared classes at multiple levels.

With your Hiera config (hiera.yaml) already setup, you'd need to put somewhere within
your hierarchy a yaml file containing such as the following:

```yaml
---
    classes:
      - some_cool_class
      - some_other_class
      - and_yet_another_class

```

This would cause these classes to be declared for whatever **level** in the hierarchy
they have been specified.  For example, if you put the above in a **"node-level yaml"** file,
they would get declared for the corresponding node.  If you put it in a yaml file
for a specific **OS Family**, they would get declared only for systems running that particular OS Family.
Etc.  Notice in the first case the classes would be applied to one specific node, while in the
second case the classes could be be applied to a set of nodes.


### Hiera Examples

Let's work through an example to get a better understanding how all of this works.

Here's the overview of what we are about to do:

1. Install the ntp module
2. Create a "node-level" yaml file for the **agent** node and assign the **ntp** class to it
3. Create a **common.yaml** and put in NTP server parameters to illistrate the **[auto-parameter lookup](https://docs.puppetlabs.com/hiera/1/puppet.html#automatic-parameter-lookup)** feature of Hiera
4. Show how we can define multiple locations, and override the NTP servers for each location
5. Start to talk about facter just a little bit, and show how we can set the location in two ways: a Hiera key or a Fact on the agent side and explain the security implications
   (Facts come from the agent/node side, while Hiera data is only on the Master)

Now, follow along as we walk through each step...

On our Puppet Master, the end of the **site.pp** looks like this:

```puppet
#
# Global - All code outside a node definition gets applied to all nodes
#

include common_packages
include common_hosts

#
# Node-specific
#

node 'puppet.example.com' {
}

node 'agent.example.com' {
}

#
# Default - if no other Node-specific definition matched
#

node default {
}
```


Let's **delete** the node definitions for 'puppet.example.com' and 'agent.example.com'...They're not even used anyway.  And then let's add our call to **hiera_include**

(Remember, the **site.pp** is in `/etc/puppetlabs/code/environments/production/manifests/` )

We want the end of our site.pp to end up looking like this:

```puppet
#
# Global - All code outside a node definition gets applied to all nodes
#

include common_packages
include common_hosts

#
# Default - Not used, but Puppet requires at least an empty **node default**
#

node default {
}

#
# Use Hiera to classify nodes
#

hiera_include('classes')

```


Go ahead and try running puppet on the Puppet Master now, and see what happens...


```
     [root@puppet ~]# cd /etc/puppetlabs/code/environments/production/manifests/
     [root@puppet manifests]# vi site.pp
     [root@puppet manifests]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Error: Could not retrieve catalog from remote server: Error 500 on SERVER:
          Server Error: Evaluation Error: Error while evaluating a Function Call, Could
          not find data item classes in any Hiera data file and no default supplied at
          /etc/puppetlabs/code/environments/production/manifests/site.pp:41:1
          on node puppet.example.com
     Warning: Not using cache on failed catalog
     Error: Could not retrieve catalog; skipping run
```

Notice that Puppet couldn't find the key **"classes"** anywhere within our Hiera data hierarchy.
That is to be expected!  We told Hiera to look for the key **classes** in our Hiera Data, but
we have not created any Hiera Data yet!

```
     [root@puppet manifests]# pwd
     /etc/puppetlabs/code/environments/production/manifests
     [root@puppet manifests]# cd ../hieradata
     [root@puppet hieradata]# vi common.yaml
```


Let's add the following to the **common.yaml**:


```yaml
---

classes:
   - common_hosts
   - common_packages

```

Try running puppet again...and you should get a clean run.

Did you notice that we already had the **common_hosts** and **common_packages**
declared via an include in the site.pp?  Now that we've put those in the common.yaml,
we can remove them from the site.pp, as they're not needed in both places.

Go ahead and edit your site.pp and remove them, so you're left with...


```puppet
#
# Global - All code outside a node definition gets applied to all nodes
#

#
# Default - if no other Node-specific definition matched
#

node default {
}

hiera_include('classes')
```

Now run puppet again, and you should notice no changes at all.  Good?

So all we've done is configured Hiera and used it as a pseudo-ENC, created our first hiera data file **common.yaml** and changed the way we are declaring the 'common_hosts' and 'common_packages' classes.

### Install a module and classify a node with it

**Next**, let's install a module, and show how we can declare that class on only the agent node using a "node-level" yaml file.

```
     [root@puppet manifests]# puppet module install puppetlabs/ntp
     Notice: Preparing to install into /etc/puppetlabs/code/environments/production/modules ...
     Notice: Downloading from https://forgeapi.puppetlabs.com ...
     Notice: Installing -- do not interrupt ...
     /etc/puppetlabs/code/environments/production/modules
     └─┬ puppetlabs-ntp (v6.0.0)
       └── puppetlabs-stdlib (v4.13.1)

```

We now have the PuppetLab's NTP module installed.

Next, let's tell Puppet to declare this class for node **agent.example.com**


```
     [root@puppet production]# pwd
     /etc/puppetlabs/code/environments/production
     [root@puppet production]# cd hieradata
     [root@puppet hieradata]# tree
     .
     ├── common.yaml
     ├── location
     ├── node
     └── role

     3 directories, 1 file
     [root@puppet hieradata]# cd node
     [root@puppet node]# vi agent.example.com.yaml
```


Add the following in to the **agent.example.com.yaml**


```yaml
---

classes:
   - ntp


```


Now on **agent.example.com** run puppet, and you should see Puppet configure NTP on that node only.


```
     [root@agent ~]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     [snip] - removed all the output from the pluginfacts download
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for agent.example.com
     Info: Applying configuration version '1457048266'
     Notice: /Stage[main]/Ntp::Install/Package[ntp]/ensure: created
     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content:
     --- /etc/ntp.conf 2016-01-25 14:15:26.000000000 +0000
     +++ /tmp/puppet-file20160303-5563-1bz48di 2016-03-03 23:38:23.047844481 +0000
     @@ -1,58 +1,36 @@
     -# For more information about this file, see the man pages
     -# ntp.conf(5), ntp_acc(5), ntp_auth(5), ntp_clock(5), ntp_misc(5), ntp_mon(5).
     +# ntp.conf: Managed by puppet.
     +#
     +# Enable next tinker options:
     +# panic - keep ntpd from panicking in the event of a large clock skew
     +# when a VM guest is suspended and resumed;
     +# stepout - allow ntpd change offset faster
     +tinker panic 0

     -driftfile /var/lib/ntp/drift
     +disable monitor

      # Permit time synchronization with our time source, but do not
      # permit the source to query or modify the service on this system.
     -restrict default nomodify notrap nopeer noquery
     +restrict default kod nomodify notrap nopeer noquery
     +restrict -6 default kod nomodify notrap nopeer noquery
     +restrict 127.0.0.1
     +restrict -6 ::1
     +
     +
     +
     +# Set up servers for ntpd with next options:
     +# server - IP address or DNS name of upstream NTP server
     +# iburst - allow send sync packages faster if upstream unavailable
     +# prefer - select preferrable server
     +# minpoll - set minimal update frequency
     +# maxpoll - set maximal update frequency
     +server 0.centos.pool.ntp.org
     +server 1.centos.pool.ntp.org
     +server 2.centos.pool.ntp.org
     +
     +
     +# Driftfile.
     +driftfile /var/lib/ntp/drift
     +
     +
     +

     -# Permit all access over the loopback interface.  This could
     -# be tightened as well, but to do so would effect some of
     -# the administrative functions.
     -restrict 127.0.0.1
     -restrict ::1
     -
     -# Hosts on local network are less restricted.
     -#restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap
     -
     -# Use public servers from the pool.ntp.org project.
     -# Please consider joining the pool (http://www.pool.ntp.org/join.html).
     -server 0.centos.pool.ntp.org iburst
     -server 1.centos.pool.ntp.org iburst
     -server 2.centos.pool.ntp.org iburst
     -server 3.centos.pool.ntp.org iburst
     -
     -#broadcast 192.168.1.255 autokey # broadcast server
     -#broadcastclient     # broadcast client
     -#broadcast 224.0.1.1 autokey   # multicast server
     -#multicastclient 224.0.1.1   # multicast client
     -#manycastserver 239.255.254.254    # manycast server
     -#manycastclient 239.255.254.254 autokey # manycast client
     -
     -# Enable public key cryptography.
     -#crypto
     -
     -includefile /etc/ntp/crypto/pw
     -
     -# Key file containing the keys and key identifiers used when operating
     -# with symmetric key cryptography.
     -keys /etc/ntp/keys
     -
     -# Specify the key identifiers which are trusted.
     -#trustedkey 4 8 42
     -
     -# Specify the key identifier to use with the ntpdc utility.
     -#requestkey 8
     -
     -# Specify the key identifier to use with the ntpq utility.
     -#controlkey 8
     -
     -# Enable writing of statistics records.
     -#statistics clockstats cryptostats loopstats peerstats
     -
     -# Disable the monitoring facility to prevent amplification attacks using ntpdc
     -# monlist command when default restrict does not include the noquery flag. See
     -# CVE-2013-5211 for more details.
     -# Note: Monitoring will not be disabled with the limited restriction flag.
     -disable monitor

     Info: Computing checksum on file /etc/ntp.conf
     Info: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]: Filebucketed /etc/ntp.conf to main with sum dc9e5754ad2bb6f6c32b954c04431d0a
     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content: content changed '{md5}dc9e5754ad2bb6f6c32b954c04431d0a' to '{md5}c1d0e073779a9102773754cf972486be'
     Info: Class[Ntp::Config]: Scheduling refresh of Class[Ntp::Service]
     Info: Class[Ntp::Service]: Scheduling refresh of Service[ntp]
     Notice: /Stage[main]/Ntp::Service/Service[ntp]/ensure: ensure changed 'stopped' to 'running'
     Info: /Stage[main]/Ntp::Service/Service[ntp]: Unscheduling refresh on Service[ntp]
     Notice: Finished catalog run in 35.90 seconds
```

In the above output, where the ntp.conf edit is shown, any line with a minus sign (-) in front is being removed by puppet, and any line with a plus sign (+) in front is being added. (Similar to what you would see when use use the **diff** command)

Notice that Puppet did a few things:

1. It installed the NTP package
2. It configured the /etc/ntp.conf
3. It enabled and started the NTP service

Notice that if you use **puppet resource** to check the current state of the ntpd it shows it's running and enabled:

```puppet
     [root@agent ~]# puppet resource service ntpd
     service { 'ntpd':
       ensure => 'running',
       enable => 'true',
     }
```

Has our time sync'ed yet?

```
[root@agent ~]# date
Tue Nov 15 21:15:30 UTC 2016

[root@agent ~]# timedatectl
      Local time: Tue 2016-11-15 21:15:37 UTC
  Universal time: Tue 2016-11-15 21:15:37 UTC
        RTC time: Tue 2016-11-15 21:15:36
       Time zone: UTC (UTC, +0000)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: yes
      DST active: n/a
```

After a minute or so, NTP shows that it is synchronized, but I can't easily tell what time it is because the timezone is set to UTC
How about we install a puppet module to manage the timezone for us?

I found the **saz-timezone** module on the Puppet Forge ( <http://forge.puppetlabs.com/> )
and even though it's not a puppetlabs-authored module, nor a puppetlabs-supported module,
it does seem to be the most popular, and has a good user rating.  It also supports
pretty much any/all linux flavor out there.  (Always make sure to check that
the module you're installing supports the platforms you will be using, or forsee
yourself using in the future.)

Let's go ahead and install the saz-timezone module...

```
     [root@puppet node]# puppet module install saz-timezone
     Notice: Preparing to install into /etc/puppetlabs/code/environments/production/modules ...
     Notice: Downloading from https://forgeapi.puppetlabs.com ...
     Notice: Installing -- do not interrupt ...
     /etc/puppetlabs/code/environments/production/modules
     └─┬ saz-timezone (v3.3.0)
       └── puppetlabs-stdlib (v4.13.1)
```

### Hiera auto-parameter lookup

Next, edit your node yaml for **"agent.example.com"** again, add the new class, and add the class parameter we'd like to pass in:

```yaml
---

classes:
   - ntp
   - timezone

timezone::timezone: 'US/Pacific'

```

Notice that funny **timezone::timezone** line in there.  This is the second
way we can use Hiera.  When we put in a line of the form  **CLASS**::**PARAM**: **VALUE**
Puppet will automatically look for and find the value for that parameter.
This is a nice way to keep your site-specific data separated out from your code/modules.


```
     [root@agent ~]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for agent.example.com
     Info: Applying configuration version '1479244724'
     Notice: /Stage[main]/Timezone/File[/etc/localtime]/target: target changed '../usr/share/zoneinfo/UTC' to '/usr/share/zoneinfo/US/Pacific'
     Notice: Applied catalog in 0.71 seconds

     [root@agent ~]# date
     Tue Nov 15 13:19:50 PST 2016

     [root@agent ~]# timedatectl
           Local time: Tue 2016-11-15 13:18:56 PST
       Universal time: Tue 2016-11-15 21:18:56 UTC
             RTC time: Tue 2016-11-15 21:18:54
            Time zone: US/Pacific (PST, -0800)
          NTP enabled: yes
     NTP synchronized: yes
      RTC in local TZ: yes
           DST active: no
      Last DST change: DST ended at
                       Sun 2016-11-06 01:59:59 PDT
                       Sun 2016-11-06 01:00:00 PST
      Next DST change: DST begins (the clock jumps one hour forward) at
                       Sun 2017-03-12 01:59:59 PST
                       Sun 2017-03-12 03:00:00 PDT
```

Okay, that looks great!  Time date/time is correct, timezone has been set, and NTP is sync'ed.

Notice that we're pointing at the following NTP servers:

```
     [root@agent ~]# grep 'server ' /etc/ntp.conf
     # server - IP address or DNS name of upstream NTP server
     server 0.centos.pool.ntp.org
     server 1.centos.pool.ntp.org
     server 2.centos.pool.ntp.org
```

What if we have our own internal NTP servers that we'd prefer to point at?  Or maybe we just wanted to change to use a different set of external NTP servers?

Let's add the appropriate class params in our hiera data to set the NTP servers to the following:

```
     server 0.pool.ntp.org
     server 1.pool.ntp.org
     server 2.pool.ntp.org
     server 3.pool.ntp.org
```

Well how would we know what class and param name to use?  You can do one of the following:

1. Read the documentation for the module at:   <https://forge.puppetlabs.com/puppetlabs/ntp>
2. Click on [Project URL]<https://github.com/puppetlabs/puppetlabs-ntp/tree/master/manifests> link at the top of the Puppet Forge page for the module, and then read the actual puppet code.
3. Find the module on your Puppet Master, and browse the code there.

Hopefully the module is well documented.  In the case of puppetlab-ntp, it is very well documented, and says to specify the servers like this:

```puppet
     class { '::ntp':
       servers => [ '0.pool.ntp.org', '1.pool.ntp.org', '2.pool.ntp.org', '3.pool.ntp.org' ],
     }
```

That is a special syntax (which we've not seen yet) for declaring a class and
passing parameters in.  This is not how we do it with Hiera, but at least we
can see the name of the class is just **'ntp'** and the parameter name is
**'servers'** and expects an array of server names.

The Hiera data to specify this would look like this:

```yaml

     ntp::servers:
       - '0.pool.ntp.org'
       - '1.pool.ntp.org'
       - '2.pool.ntp.org'
       - '3.pool.ntp.org'

```

Let's add this to our **common.yaml** with the idea being we'd want these NTP servers set for any and every node using the ntp module.

Run puppet on your agent VM, and you'll notice that puppet updated your /etc/ntp.conf as well as restarted ntpd:

```
     [root@agent ~]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for agent.example.com
     Info: Applying configuration version '1479245016'
     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content:
     --- /etc/ntp.conf    2016-11-15 13:13:09.543703681 -0800
     +++ /tmp/puppet-file20161115-7122-y5vagy    2016-11-15 13:23:38.173358409 -0800
     @@ -23,9 +23,10 @@
      # prefer - select preferrable server
      # minpoll - set minimal update frequency
      # maxpoll - set maximal update frequency
     -server 0.centos.pool.ntp.org
     -server 1.centos.pool.ntp.org
     -server 2.centos.pool.ntp.org
     +server 0.pool.ntp.org
     +server 1.pool.ntp.org
     +server 2.pool.ntp.org
     +server 3.pool.ntp.org


      # Driftfile.

     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content: content changed '{md5}1f44e40bd99abd89f0a209e823285332' to '{md5}0921dc972e65220981482cbcbb31fb3c'
     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp/step-tickers]/content:
     --- /etc/ntp/step-tickers    2016-11-15 13:13:09.552699182 -0800
     +++ /tmp/puppet-file20161115-7122-h0a82c    2016-11-15 13:23:38.188365298 -0800
     @@ -1,5 +1,6 @@
      # List of NTP servers used by the ntpdate service.

     -0.centos.pool.ntp.org
     -1.centos.pool.ntp.org
     -2.centos.pool.ntp.org
     +0.pool.ntp.org
     +1.pool.ntp.org
     +2.pool.ntp.org
     +3.pool.ntp.org

     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp/step-tickers]/content: content changed '{md5}413c531d0533c4dba18b9acf7a29ad5d' to '{md5}f60f392b1f3e1da01e2769e7d8a2a015'
     Info: Class[Ntp::Config]: Scheduling refresh of Class[Ntp::Service]
     Info: Class[Ntp::Service]: Scheduling refresh of Service[ntp]
     Notice: /Stage[main]/Ntp::Service/Service[ntp]: Triggered 'refresh' from 1 events
     Notice: Applied catalog in 0.76 seconds
```

If you run puppet on your master, you'll notice that puppet doesn't make any changes...

```
[root@puppet hieradata]# puppet agent -t
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for puppet.example.com
Info: Applying configuration version '1479245117'
Notice: Applied catalog in 16.01 seconds
```

...that because we've not configured the Puppet Master to use the NTP module.
The thing to notice here is that we can define class parameters at the "common level"
even if only a subset of our nodes will query for them.  It's just data sitting out
there in Hiera, and puppet will query it when needed, but it doesnt' hurt to be
there for nodes that arn't using those parameters.  Make sense?

Next, let's enable the NTP module on our Puppet Master **puppet.example.com**.  To
do this, we will create a "node-level" YAML file, and then add the class to the
array of classes under the **classes** key.

Get into the right directory, and create your yaml file as follows...

```
     [root@puppet ~]# cd /etc/puppetlabs/code/environments/production/hieradata/node/
     [root@puppet node]# vi puppet.example.com.yaml
```

Then put the following in your YAML file...

```yaml
---

classes:
  - ntp

```

Now, run **puppet agent -t** and see what happens...

```
     [root@puppet node]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for puppet.example.com
     Info: Applying configuration version '1479245256'
     Notice: /Stage[main]/Ntp::Install/Package[ntp]/ensure: created
     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content:
     --- /etc/ntp.conf    2016-05-31 10:11:10.000000000 +0000
     +++ /tmp/puppet-file20161115-17085-15svzqc    2016-11-15 21:27:55.209687311 +0000
     @@ -1,58 +1,41 @@
     -# For more information about this file, see the man pages
     -# ntp.conf(5), ntp_acc(5), ntp_auth(5), ntp_clock(5), ntp_misc(5), ntp_mon(5).
     +# ntp.conf: Managed by puppet.
     +#
     +# Enable next tinker options:
     +# panic - keep ntpd from panicking in the event of a large clock skew
     +# when a VM guest is suspended and resumed;
     +# stepout - allow ntpd change offset faster
     +tinker panic 0

     -driftfile /var/lib/ntp/drift
     +disable monitor

      # Permit time synchronization with our time source, but do not
      # permit the source to query or modify the service on this system.
     -restrict default nomodify notrap nopeer noquery
     +restrict default kod nomodify notrap nopeer noquery
     +restrict -6 default kod nomodify notrap nopeer noquery
     +restrict 127.0.0.1
     +restrict -6 ::1
     +
     +
     +
     +# Set up servers for ntpd with next options:
     +# server - IP address or DNS name of upstream NTP server
     +# iburst - allow send sync packages faster if upstream unavailable
     +# prefer - select preferrable server
     +# minpoll - set minimal update frequency
     +# maxpoll - set maximal update frequency
     +server 0.pool.ntp.org
     +server 1.pool.ntp.org
     +server 2.pool.ntp.org
     +server 3.pool.ntp.org
     +
     +
     +# Driftfile.
     +driftfile /var/lib/ntp/drift
     +
     +
     +
     +
     +
     +
     +

     -# Permit all access over the loopback interface.  This could
     -# be tightened as well, but to do so would effect some of
     -# the administrative functions.
     -restrict 127.0.0.1
     -restrict ::1
     -
     -# Hosts on local network are less restricted.
     -#restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap
     -
     -# Use public servers from the pool.ntp.org project.
     -# Please consider joining the pool (http://www.pool.ntp.org/join.html).
     -server 0.centos.pool.ntp.org iburst
     -server 1.centos.pool.ntp.org iburst
     -server 2.centos.pool.ntp.org iburst
     -server 3.centos.pool.ntp.org iburst
     -
     -#broadcast 192.168.1.255 autokey    # broadcast server
     -#broadcastclient            # broadcast client
     -#broadcast 224.0.1.1 autokey        # multicast server
     -#multicastclient 224.0.1.1        # multicast client
     -#manycastserver 239.255.254.254        # manycast server
     -#manycastclient 239.255.254.254 autokey # manycast client
     -
     -# Enable public key cryptography.
     -#crypto
     -
     -includefile /etc/ntp/crypto/pw
     -
     -# Key file containing the keys and key identifiers used when operating
     -# with symmetric key cryptography.
     -keys /etc/ntp/keys
     -
     -# Specify the key identifiers which are trusted.
     -#trustedkey 4 8 42
     -
     -# Specify the key identifier to use with the ntpdc utility.
     -#requestkey 8
     -
     -# Specify the key identifier to use with the ntpq utility.
     -#controlkey 8
     -
     -# Enable writing of statistics records.
     -#statistics clockstats cryptostats loopstats peerstats
     -
     -# Disable the monitoring facility to prevent amplification attacks using ntpdc
     -# monlist command when default restrict does not include the noquery flag. See
     -# CVE-2013-5211 for more details.
     -# Note: Monitoring will not be disabled with the limited restriction flag.
     -disable monitor

     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content: content changed '{md5}dc9e5754ad2bb6f6c32b954c04431d0a' to '{md5}0921dc972e65220981482cbcbb31fb3c'
     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp/step-tickers]/content:
     --- /etc/ntp/step-tickers    2016-05-31 10:11:10.000000000 +0000
     +++ /tmp/puppet-file20161115-17085-x7tamq    2016-11-15 21:27:55.302687311 +0000
     @@ -1,3 +1,6 @@
      # List of NTP servers used by the ntpdate service.

     -0.centos.pool.ntp.org
     +0.pool.ntp.org
     +1.pool.ntp.org
     +2.pool.ntp.org
     +3.pool.ntp.org

     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp/step-tickers]/content: content changed '{md5}9b77b3b3eb41daf0b9abb8ed01c5499b' to '{md5}f60f392b1f3e1da01e2769e7d8a2a015'
     Info: Class[Ntp::Config]: Scheduling refresh of Class[Ntp::Service]
     Info: Class[Ntp::Service]: Scheduling refresh of Service[ntp]
     Notice: /Stage[main]/Ntp::Service/Service[ntp]/ensure: ensure changed 'stopped' to 'running'
     Info: /Stage[main]/Ntp::Service/Service[ntp]: Unscheduling refresh on Service[ntp]
     Notice: Applied catalog in 21.47 seconds
```

NTP has been configured, enabled, and started on the Puppet Master.

```
[root@puppet node]# timedatectl
      Local time: Tue 2016-11-15 21:29:26 UTC
  Universal time: Tue 2016-11-15 21:29:26 UTC
        RTC time: Tue 2016-11-15 21:29:24
       Time zone: UTC (UTC, +0000)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: yes
      DST active: n/a

```

Oh, we forgot to set the timezone.  Let's do that now.  Add the **timezone** class to the array of classes (causing that class to be declared for the puppet.example.com node) and also set the timezone parameter to 'US/Pacific' as follows...

```yaml
---

classes:
  - ntp
  - timezone

timezone::timezone: 'US/Pacific'

```

Okay, that looks better...

```
[root@puppet node]# puppet agent -t
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for puppet.example.com
Info: Applying configuration version '1479245400'
Notice: /Stage[main]/Timezone/File[/etc/localtime]/target: target changed '../usr/share/zoneinfo/UTC' to '/usr/share/zoneinfo/US/Pacific'
Notice: Applied catalog in 17.03 seconds

[root@puppet node]# timedatectl
      Local time: Tue 2016-11-15 13:30:54 PST
  Universal time: Tue 2016-11-15 21:30:54 UTC
        RTC time: Tue 2016-11-15 21:30:52
       Time zone: US/Pacific (PST, -0800)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: yes
      DST active: no
 Last DST change: DST ended at
                  Sun 2016-11-06 01:59:59 PDT
                  Sun 2016-11-06 01:00:00 PST
 Next DST change: DST begins (the clock jumps one hour forward) at
                  Sun 2017-03-12 01:59:59 PST
                  Sun 2017-03-12 03:00:00 PDT

```

Okay, Let's summarize what we've done so far...

1. Enabled the use of Hiera as a pseudo-ENC with **hiera_include('classes')** in our site.pp
2. Installed two modules on our Puppet Master:  **puppetlabs-ntp** and **saz-timezone**
3. Created a **common.yaml** and declared the **common_hosts** and **common_packages** classes
4. Created a **"node-level"** YAML file for both **agent.example.com** and **puppet.example.com**
5. Declared the **ntp** and **timezone** modules for both **agent.example.com** and **puppet.example.com**
6. We also configured the list of ntp servers and the timezone in the common.yaml using auto-parameter lookup

Our Hiera data YAML files look like this:

```
     [root@puppet hieradata]# pwd
     /etc/puppetlabs/code/environments/production/hieradata
     [root@puppet hieradata]# tree
     .
     ├── common.yaml
     ├── location
     ├── node
     │   ├── agent.example.com.yaml
     │   └── puppet.example.com.yaml
     └── role

     3 directories, 3 files
     [root@puppet hieradata]# cat common.yaml
```

```yaml
---

classes:
   - common_hosts
   - common_packages

ntp::servers:
  - '0.pool.ntp.org'
  - '1.pool.ntp.org'
  - '2.pool.ntp.org'
  - '3.pool.ntp.org'
```

```
     [root@puppet hieradata]# cat node/puppet.example.com.yaml
```

```yaml
---

classes:
   - ntp
   - timezone

timezone::timezone: 'US/Pacific'
```

```
     [root@puppet hieradata]# cat node/agent.example.com.yaml
```

```yaml
---

classes:
   - ntp
   - timezone

timezone::timezone: 'US/Pacific'
```

### Important Note about Hiera Lookups

So far we've looked at two ways Hiera can be used:

1.  to classify nodes with the **hiera_include('classes')** function call
2.  to provide class parameter values through Hiera's auto-parameter lookup functionality

It is critically important to understand that these two Hiera features work
differently on the hierarchy of data

The **hiera_include('classes')** function searches **all** hiera data at
**every level** for the key **"classes"** and builds an array of classes.
So you can specify **classes** at the node level or common level and/or every
level in-between, and the `hiera_include()` function will pickup them all.

Hiera behaves differently for auto-parameter lookup, as well as arbitrary data
lookup function **hiera()** which we are about to look at in the next section.

When looking up class parameters, Hiera searches through your hierarcy of
data from the **top down** (as each data source appears in the hiera.yaml)
and takes the first occurance of the parameter found.  In other words,
Hiera takes the **most specific** paramater value, with the understanding
that the top level (**node level**) is the most specific, and every level
below that a less and less specific level until you reach the bottom
level **common**.

Knowing this, you may specify the same class parameter value at a higher
level to override the value set at a lower level.

For example, if you have the timezone set to 'US/Pacific' in the common.yaml,
but want to override this value for a particular node, you can specify that
same parameter in the node-level yaml file as a different value, and Hiera
will use it instead, because Hiera will encounter it first in its search
for the value.

The same behavior is followed for the **hiera()** lookup function we will
cover in the following section...


### Hiera lookup functions for arbitrary data

The third way we can use Hiera is to lookup arbitrary data in our hierarchy.
We can define key/value pairs within our hierarchy and then use the hiera
lookup functions to query the value(s) within our puppet code.

The **hiera()** function behaves the same way as auto-parameter lookup.
It gets the most specific value for a given key. It can retrieve values
of any data type including strings, arrays, or hashes, or even multi-level
data structures, such as hashes of strings, or arrays of hashes, etc.

One use case for this that I've seen in the wild is to store certain bits
of static data about a node in Hiera, rather than rely on a agent-side custom
defined fact.

### Facter

But what is a fact?  We haven't talked about what a **"fact"** is yet, so let's
take this opportunity to introduce **facter** and than show two ways that we
can cause puppet to **know** certain facts about a agent/node.

Facter presents many common system parameters to you as top-scope puppet
variables.  It provides a common namespace for many useful facts across all
platforms so you can reference the same fact in your code without worrying
what the platform is where the agent is running.

You can use the **facter** command to see the facts on an agent node
by simply running **facter** without any arguments.   You can tell facter
to include some extra puppet-specific facts with the **-p** option:

```
[root@puppet node]# facter -p
aio_agent_build => 1.7.1
aio_agent_version => 1.7.1
augeas => {
  version => "1.4.0"
}
disks => {
  sda => {
    model => "VBOX HARDDISK",
    size => "20.00 GiB",
    size_bytes => 21474836480,
    vendor => "ATA"
  }
}
dmi => {
  bios => {
    release_date => "12/01/2006",
    vendor => "innotek GmbH",
    version => "VirtualBox"
  },
  board => {
    manufacturer => "Oracle Corporation",
    product => "VirtualBox",
    serial_number => "0"
  },
  chassis => {
    type => "Other"
  },
  manufacturer => "innotek GmbH",
  product => {
    name => "VirtualBox",
    serial_number => "0",
    uuid => "A717DB68-F0CF-4E12-8A40-3D6AEFB737F3"
  }
}
facterversion => 3.4.1
filesystems => xfs
identity => {
  gid => 0,
  group => "root",
  privileged => true,
  uid => 0,
  user => "root"
}
is_pe => false
is_virtual => true
kernel => Linux
kernelmajversion => 3.10
kernelrelease => 3.10.0-327.36.3.el7.x86_64
kernelversion => 3.10.0
load_averages => {
  15m => 0.22,
  1m => 0.04,
  5m => 0.23
}
memory => {
  swap => {
    available => "299.02 MiB",
    available_bytes => 313540608,
    capacity => "70.80%",
    total => "1.00 GiB",
    total_bytes => 1073737728,
    used => "724.98 MiB",
    used_bytes => 760197120
  },
  system => {
    available => "225.34 MiB",
    available_bytes => 236290048,
    capacity => "92.09%",
    total => "2.78 GiB",
    total_bytes => 2986229760,
    used => "2.56 GiB",
    used_bytes => 2749939712
  }
}
mountpoints => {
  / => {
    available => "15.49 GiB",
    available_bytes => 16635203584,
    capacity => "16.05%",
    device => "/dev/mapper/centos-root",
    filesystem => "xfs",
    options => [
      "rw",
      "relatime",
      "attr2",
      "inode64",
      "noquota"
    ],
    size => "18.46 GiB",
    size_bytes => 19815989248,
    used => "2.96 GiB",
    used_bytes => 3180785664
  },
  /boot => {
    available => "330.43 MiB",
    available_bytes => 346484736,
    capacity => "33.47%",
    device => "/dev/sda1",
    filesystem => "xfs",
    options => [
      "rw",
      "relatime",
      "attr2",
      "inode64",
      "noquota"
    ],
    size => "496.67 MiB",
    size_bytes => 520794112,
    used => "166.23 MiB",
    used_bytes => 174309376
  }
}
networking => {
  dhcp => "10.0.2.2",
  domain => "example.com",
  fqdn => "puppet.example.com",
  hostname => "puppet",
  interfaces => {
    enp0s3 => {
      bindings => [
        {
          address => "10.0.2.15",
          netmask => "255.255.255.0",
          network => "10.0.2.0"
        }
      ],
      bindings6 => [
        {
          address => "fe80::a00:27ff:feb7:f3af",
          netmask => "ffff:ffff:ffff:ffff::",
          network => "fe80::"
        }
      ],
      dhcp => "10.0.2.2",
      ip => "10.0.2.15",
      ip6 => "fe80::a00:27ff:feb7:f3af",
      mac => "08:00:27:b7:f3:af",
      mtu => 1500,
      netmask => "255.255.255.0",
      netmask6 => "ffff:ffff:ffff:ffff::",
      network => "10.0.2.0",
      network6 => "fe80::"
    },
    enp0s8 => {
      bindings => [
        {
          address => "192.168.198.10",
          netmask => "255.255.255.0",
          network => "192.168.198.0"
        }
      ],
      bindings6 => [
        {
          address => "fe80::a00:27ff:fec3:1131",
          netmask => "ffff:ffff:ffff:ffff::",
          network => "fe80::"
        }
      ],
      dhcp => "192.168.56.100",
      ip => "192.168.198.10",
      ip6 => "fe80::a00:27ff:fec3:1131",
      mac => "08:00:27:c3:11:31",
      mtu => 1500,
      netmask => "255.255.255.0",
      netmask6 => "ffff:ffff:ffff:ffff::",
      network => "192.168.198.0",
      network6 => "fe80::"
    },
    lo => {
      bindings => [
        {
          address => "127.0.0.1",
          netmask => "255.0.0.0",
          network => "127.0.0.0"
        }
      ],
      bindings6 => [
        {
          address => "::1",
          netmask => "ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff",
          network => "::1"
        }
      ],
      ip => "127.0.0.1",
      ip6 => "::1",
      mtu => 65536,
      netmask => "255.0.0.0",
      netmask6 => "ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff",
      network => "127.0.0.0",
      network6 => "::1"
    }
  },
  ip => "10.0.2.15",
  ip6 => "fe80::a00:27ff:feb7:f3af",
  mac => "08:00:27:b7:f3:af",
  mtu => 1500,
  netmask => "255.255.255.0",
  netmask6 => "ffff:ffff:ffff:ffff::",
  network => "10.0.2.0",
  network6 => "fe80::",
  primary => "enp0s3"
}
os => {
  architecture => "x86_64",
  family => "RedHat",
  hardware => "x86_64",
  name => "CentOS",
  release => {
    full => "7.2.1511",
    major => "7",
    minor => "2"
  },
  selinux => {
    enabled => false
  }
}
package_provider => yum
partitions => {
  /dev/mapper/centos-root => {
    filesystem => "xfs",
    mount => "/",
    size => "18.46 GiB",
    size_bytes => 19826475008,
    uuid => "c5e9538a-9f0f-4666-88e7-28bb52b62e43"
  },
  /dev/mapper/centos-swap => {
    filesystem => "swap",
    size => "1.00 GiB",
    size_bytes => 1073741824,
    uuid => "8e3c7c45-a31b-479f-bd47-25764cf80fab"
  },
  /dev/sda1 => {
    filesystem => "xfs",
    mount => "/boot",
    size => "500.00 MiB",
    size_bytes => 524288000,
    uuid => "5dc0799f-b2a8-465e-8a47-60b677be09b3"
  },
  /dev/sda2 => {
    filesystem => "LVM2_member",
    size => "19.51 GiB",
    size_bytes => 20949499904,
    uuid => "PpSFVZ-SS3P-n3a6-ctPF-sb9H-6M85-i0TqBv"
  }
}
path => /usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/opt/puppetlabs/bin:/root/bin
pe_build => 2016.4.0
pe_concat_basedir => /opt/puppetlabs/puppet/cache/pe_concat
pe_razor_server_version => package pe-razor-server is not installed
pe_server_version => 2016.4.0
platform_symlink_writable => true
platform_tag => el-7-x86_64
processors => {
  count => 2,
  isa => "x86_64",
  models => [
    "Intel(R) Core(TM) i7-2720QM CPU @ 2.20GHz",
    "Intel(R) Core(TM) i7-2720QM CPU @ 2.20GHz"
  ],
  physicalcount => 1
}
puppet_environmentpath => /etc/puppetlabs/code/environments
puppet_files_dir_present => false
puppet_vardir => /opt/puppetlabs/puppet/cache
puppetversion => 4.7.0
root_home => /root
ruby => {
  platform => "x86_64-linux",
  sitedir => "/opt/puppetlabs/puppet/lib/ruby/site_ruby/2.1.0",
  version => "2.1.9"
}
service_provider => systemd
ssh => {
   [snip]
}
staging_http_get => curl
system_uptime => {
  days => 0,
  hours => 3,
  seconds => 14113,
  uptime => "3:55 hours"
}
timezone => PST
virtual => virtualbox

```

You can query the value of a fact from the command line like this:

```
[root@puppet node]# facter -p service_provider
systemd

[root@puppet node]# facter -p os
{
  architecture => "x86_64",
  family => "RedHat",
  hardware => "x86_64",
  name => "CentOS",
  release => {
    full => "7.2.1511",
    major => "7",
    minor => "2"
  },
  selinux => {
    enabled => false
  }
}

[root@puppet node]# facter -p os.family
RedHat

[root@puppet node]# facter -p os.release
{
  full => "7.2.1511",
  major => "7",
  minor => "2"
}

[root@puppet node]# facter -p os.release.major
7

```

When puppet runs, it takes all of these facts about the system, and makes them available as top-scope variables.  Pretty nice of it, eh?

So if you want to use any of those facts in your puppet code, you can simply take the fact name and use it like you would a top-scope puppet variable like this:

```puppet
     $::fact_name
```

So if you wanted to use the **service_provider** fact, you could reference:

```puppet
     $::service_provider
```

...in your puppet code.  This could be very useful to a module author who wants to support many different OS Families/Platforms.
Take a look at how many possible service providers there are:  [Service Providers](https://docs.puppetlabs.com/puppet/latest/reference/type.html#service-attribute-provider)

What if you want to define a custom fact for use in your puppet code?
Puppet+Facter allows you to write custom code to create a custom fact, but
even simpler, you can also create a yaml file with [static facts](http://docs.puppetlabs.com/facter/3.4/custom_facts.html#external-facts) for that node
that would show up as top-scope puppet variables.

The best way to define custom static agent-side facts is by including them in the facts.d directory within a module.
However, we've not yet learned how to write a module, and to keep things simple for now, let's use the next best
method of creating a yaml file in **/etc/puppetlabs/facter/facts.d/** containing the key/value pair we want to set.

```
     [root@agent ~]# mkdir -p /etc/puppetlabs/facter/facts.d

     [root@agent ~]# vi /etc/puppetlabs/facter/facts.d/static-facts.yaml

     [root@agent ~]# cat /etc/puppetlabs/facter/facts.d/static-facts.yaml
     ---
     location: woodinville

     [root@agent ~]# facter location
     woodinville
```

It's as simple as that!  Now, within your puppet code, you could refer to the
top-scope variable **$::location** and you'd get the value **woodinville**.
If you have servers in multiple datacenters in different cities, you could use
such a fact to keep track of where each node/agent is running, and then make
different decisions in your puppet code based on location.  Since facts become
top-scope puppet variables, you can also refer to them in your hiera.yaml!
Remember that we included a **location/${location}** in our hiera.yaml before?
Well, now is our opportunity to use it...

Let's take the time to setup the same custom fact on our Puppet Master as well,
but let's give it a different location of 'seattle' ...

```
     [root@puppet ~]# mkdir -p /etc/puppetlabs/facter/facts.d

     [root@puppet ~]# vi /etc/puppetlabs/facter/facts.d/static-facts.yaml

     [root@puppet ~]# cat /etc/puppetlabs/facter/facts.d/static-facts.yaml
     ---
     location: seattle

     [root@puppet ~]# facter location
     seattle
```

At this point we have a custom fact called **"location"** setup on both our
**puppet** node and **agent** node, and have tested that facter returns the
correct value from the command line.  Let's try to see if we can access
that variable from within a puppet manifest...

For the sake of simplicity, let's just edit our site.pp, and see if we can
access the location fact as a top-scope puppet variable...Add the single
**notify** resource to the site.pp as follows:

```
     #
     # Global - All code outside a node definition gets applied to all nodes
     #

     notify{ "Location is: ${::location}": }

```

Then run **puppet agent -t** on both the **puppet** node and the **agent** node.

On **puppet.example.com** node we see:

```
     [root@puppet manifests]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for puppet.example.com
     Info: Applying configuration version '1479246560'
     Notice: Location is: seattle
     Notice: /Stage[main]/Main/Notify[Location is: seattle]/message: defined 'message' as 'Location is: seattle'
     Notice: Applied catalog in 16.07 seconds
```

And on **agent.example.com** node we see:

```
     [root@agent ~]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for agent.example.com
     Info: Applying configuration version '1479246571'
     Notice: Location is: woodinville
     Notice: /Stage[main]/Main/Notify[Location is: woodinville]/message: defined 'message' as 'Location is: woodinville'
     Notice: Applied catalog in 0.77 seconds
```

Now let's take advantage of this new top-scope variable with Hiera.  Create a **woodinville.yaml** and **seattle.yaml** in the data/location directory as follows:

```
     [root@puppet puppet]# cd /etc/puppetlabs/code/environments/production/hieradata/location/
     [root@puppet location]# vi woodinville.yaml
     [root@puppet location]# cp woodinville.yaml seattle.yaml
```

Let's change the NTP servers we are pointing at to the following:

```yaml
     [root@puppet location]# cat woodinville.yaml
     ---

     ntp::servers:
       - '0.us.pool.ntp.org'
       - '1.us.pool.ntp.org'
       - '2.us.pool.ntp.org'
       - '3.us.pool.ntp.org'

```

(We've just added the **us** in there to use US NTP servers)

Then run puppet, and see what happens...

```
     [root@puppet location]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for puppet.example.com
     Info: Applying configuration version '1479246688'
     Notice: Location is: seattle
     Notice: /Stage[main]/Main/Notify[Location is: seattle]/message: defined 'message' as 'Location is: seattle'
     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content:
     --- /etc/ntp.conf    2016-11-15 13:27:55.261687311 -0800
     +++ /tmp/puppet-file20161115-19736-n0ofpo    2016-11-15 13:51:41.292578048 -0800
     @@ -23,10 +23,10 @@
      # prefer - select preferrable server
      # minpoll - set minimal update frequency
      # maxpoll - set maximal update frequency
     -server 0.pool.ntp.org
     -server 1.pool.ntp.org
     -server 2.pool.ntp.org
     -server 3.pool.ntp.org
     +server 0.us.pool.ntp.org
     +server 1.us.pool.ntp.org
     +server 2.us.pool.ntp.org
     +server 3.us.pool.ntp.org


      # Driftfile.

     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content: content changed '{md5}0921dc972e65220981482cbcbb31fb3c' to '{md5}7647ce38234dc60f92b74fadcfe1a49f'
     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp/step-tickers]/content:
     --- /etc/ntp/step-tickers    2016-11-15 13:27:55.315687311 -0800
     +++ /tmp/puppet-file20161115-19736-dan8wj    2016-11-15 13:51:41.333574867 -0800
     @@ -1,6 +1,6 @@
      # List of NTP servers used by the ntpdate service.

     -0.pool.ntp.org
     -1.pool.ntp.org
     -2.pool.ntp.org
     -3.pool.ntp.org
     +0.us.pool.ntp.org
     +1.us.pool.ntp.org
     +2.us.pool.ntp.org
     +3.us.pool.ntp.org

     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp/step-tickers]/content: content changed '{md5}f60f392b1f3e1da01e2769e7d8a2a015' to '{md5}7fe1770afdb4f2d3f50a25dad7fd92e9'
     Info: Class[Ntp::Config]: Scheduling refresh of Class[Ntp::Service]
     Info: Class[Ntp::Service]: Scheduling refresh of Service[ntp]
     Notice: /Stage[main]/Ntp::Service/Service[ntp]: Triggered 'refresh' from 1 events
     Notice: Applied catalog in 14.79 seconds
```

Notice that our **"location-level"** hiera data has overridden the hiera data in **common.yaml**

What if we want to introduce a new location?  We simply create a new YAML file named to match the location name, and useing the .yaml extension.

Let's create a new location for Amsterdam like this:

```
     [root@puppet location]# pwd
     /etc/puppetlabs/code/environments/production/hieradata/location
     [root@puppet location]# cp woodinville.yaml amsterdam.yaml
     [root@puppet location]# vi amsterdam.yaml
     [root@puppet location]# cat amsterdam.yaml
     ---

     ntp::servers:
       - '0.nl.pool.ntp.org'
       - '1.nl.pool.ntp.org'
       - '2.nl.pool.ntp.org'
       - '3.nl.pool.ntp.org'

```

Notice we've also changed the NTP servers to use one in the Netherlands.

We now have a new location setup in Hiera, so let's try changing our **agent** node to this new location, and then run puppet again...

```
     [root@agent ~]# vi /etc/puppetlabs/facter/facts.d/static-facts.yaml

     [root@agent ~]# cat  /etc/puppetlabs/facter/facts.d/static-facts.yaml
     ---
     location: amsterdam


     [root@agent ~]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for agent.example.com
     Info: Applying configuration version '1479246847'
     Notice: Location is: amsterdam
     Notice: /Stage[main]/Main/Notify[Location is: amsterdam]/message: defined 'message' as 'Location is: amsterdam'
     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content:
     --- /etc/ntp.conf    2016-11-15 13:51:41.518139951 -0800
     +++ /tmp/puppet-file20161115-7667-1t9r2tm    2016-11-15 13:54:09.972941140 -0800
     @@ -23,10 +23,10 @@
      # prefer - select preferrable server
      # minpoll - set minimal update frequency
      # maxpoll - set maximal update frequency
     -server 0.us.pool.ntp.org
     -server 1.us.pool.ntp.org
     -server 2.us.pool.ntp.org
     -server 3.us.pool.ntp.org
     +server 0.nl.pool.ntp.org
     +server 1.nl.pool.ntp.org
     +server 2.nl.pool.ntp.org
     +server 3.nl.pool.ntp.org


      # Driftfile.

     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content: content changed '{md5}7647ce38234dc60f92b74fadcfe1a49f' to '{md5}606dacc879656797d591a909ecc5121a'
     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp/step-tickers]/content:
     --- /etc/ntp/step-tickers    2016-11-15 13:51:41.537138446 -0800
     +++ /tmp/puppet-file20161115-7667-19a40gh    2016-11-15 13:54:09.987942799 -0800
     @@ -1,6 +1,6 @@
      # List of NTP servers used by the ntpdate service.

     -0.us.pool.ntp.org
     -1.us.pool.ntp.org
     -2.us.pool.ntp.org
     -3.us.pool.ntp.org
     +0.nl.pool.ntp.org
     +1.nl.pool.ntp.org
     +2.nl.pool.ntp.org
     +3.nl.pool.ntp.org

     Notice: /Stage[main]/Ntp::Config/File[/etc/ntp/step-tickers]/content: content changed '{md5}7fe1770afdb4f2d3f50a25dad7fd92e9' to '{md5}99ae95e4ebb1c47d27ff1f507e4bda34'
     Info: Class[Ntp::Config]: Scheduling refresh of Class[Ntp::Service]
     Info: Class[Ntp::Service]: Scheduling refresh of Service[ntp]
     Notice: /Stage[main]/Ntp::Service/Service[ntp]: Triggered 'refresh' from 1 events
     Notice: Applied catalog in 0.83 seconds
```

So what have we learned here?

1.  We can create agent-side custom facts.  We've created a static fact called **"location"**
2.  Facter facts automatically become puppet top-scope variables
3.  Any top-scope variable can be used in the hiera.yaml to define the hierarchy
4.  We observed that more-specific class parameters override less-specific ones

### Security Note

Did you notice that you affected the configuration of the host by simply changing the value of a agent-side fact?
You should be asking yourself the question:  do we want this ability and power on the agent side? Or do we prefer to "host" this power within our Hiera data?
Clearly, if we use agent-side facts, we want to ensure the facts are only writable by those we trust.

### Getting back to Hiera

Remember we were talking about how to use the **hiera()** lookup function?

In the above example we were creating an agent-side yaml file to hold our
static facts.  What if we **do not** like the idea of the facts being stored
on the agent's filesystem?  We many not like that any user with root
could change those facts to something else, potentially bypassing change-control
proceedure, or even bypassing security?  Puppet does support something
called 'Trusted Facts' which stores a node's facts in its certificate, but
this is an advanced topic, and for the sake of illistrating how we can
use Hiera, we're going to take a different approach.

The approach here will be to define a hiera key called **location** and
we'll use it instead of the facter fact.

To do this, we will add the same key/value pair that we had previously
added to the node's facts.d/ directory to the hiera node-level yaml file
instead.

```
     [root@puppet node]# pwd
     /etc/puppetlabs/code/environments/production/hieradata/node
     [root@puppet node]# tree
     .
     ├── agent.example.com.yaml
     └── puppet.example.com.yaml

     0 directories, 2 files
     [root@puppet node]# vi agent.example.com.yaml
     [root@puppet node]# vi puppet.example.com.yaml
     [root@puppet node]# cat agent.example.com.yaml
     ---

     location: 'amsterdam'

     classes:
        - ntp
        - timezone

     timezone::timezone: 'US/Pacific'

     [root@puppet node]# cat puppet.example.com.yaml
     ---

     location: 'seattle'

     classes:
        - ntp
        - timezone

     timezone::timezone: 'US/Pacific'
```

We've created a new key called **"location"** in each node-level yaml file.  We can now use the **hiera()** lookup function to query for that key and retrieve the value.
Let's edit our site.pp to accomplish that...

```

     #
     # Global - All code outside a node definition gets applied to all nodes
     #

     $location = hiera('location')

     notify{ "Location is: ${::location}": }

```

Now try running puppet on either node...

```
     [root@agent ~]# puppet agent -t
     Info: Using configured environment 'production'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Error: Could not retrieve catalog from remote server: Error 500 on SERVER: Server Error: Evaluation Error: Cannot reassign variable '$location' at /etc/puppetlabs/code/environments/production/manifests/site.pp:31:11 on node agent.example.com
     Warning: Not using cache on failed catalog
     Error: Could not retrieve catalog; skipping run
```

We got an error!  What happened?

Remember, any variable created in the site.pp automatically becomes a top-scope
variable.  Also, all facter facts become top-scope puppet variables.  In this
case, we have both a facter fact **location** as well as a puppet variable
assignment to **$location** happening in the site.pp, so we're getting an
error because **$::location** is already defined when we try to assign to it
a value.  *Puppet only allows a variable to be set once*, and only once. If you've
done any scripting or programming, you may be surprised that in puppet once you
assign a value to a variable, you can not re-assign to it again.  In fact,
it doesn't make much sense to call it a "variable" because it can not vary!

So, to get around this, we have to either remove the previously-created custom fact,
or rename one of the two.

Let's simply remove the fact that we had previously created, and prove that the
newly-created hiera key/value pair is taking over the responsibility of setting
the top-scope variable $::location

On both the **puppet** node and the **agent** node remove the custom-facts.yaml

```
     [root@puppet ~]# rm -f /etc/puppetlabs/facter/facts.d/static-facts.yaml
     [root@agent ~]# rm -f /etc/puppetlabs/facter/facts.d/static-facts.yaml
```

Now if we re-run puppet on both of our training nodes, we will see success!

But what's the advantage of doing this?  I can't run **facter location** anymore, and I liked that.

Now, the location variable is controlled within our Hiera data, and not on
the local agent node.  In theory, this is more secure, because your puppet
master should be more secure than any old agent node out there.

### Important note about using Hiera Data within the hiera.yaml

It's really important for you to notice that:

1.  We've defined the top-scope $::location variable in our site.pp
2.  We are using that top-scope variable in our hiera.yaml

How does this work?

Becuase we query hiera('location') in the site.pp, a new top-scope variable
is created, and this top-scope variable becomes availble to subsequent levels
within the hierarchy.  Crazy eh?

What this implies is that we should do any assignments like this in our site.pp
**ONLY**. This guarantees top-scope variables.  We should also use ONLY a single
manifest file at the top level, where the site.pp is sitting, even though
puppet supports having other manifests at the same level.  This guarantees our
top-scope variables are set prior to any other code or modules needing to use
them.

For example, in the training exercises we've been working through, we've created
a common_hosts.pp and common_packages.pp at the top-level.  If we tried to use
the top-scope variable **$::location** inside either of those manifests, there is
no guarantee it has been defined yet.  The order in which puppet processes top-level
manifests is not defined, and the common_hosts.pp may be read prior to the site.pp,
and the $::location variable would be undefined at that point.

### Hiera vs Facter for static data

So, to summarize, we can use the hiera() lookup function and facter to do
the same thing, the difference being where the data is hosted--on the agent side,
or on the puppet master side.

---

Continue to **Lab #8** --> [Environments](08-Environments.md)

---

### Further Reading

1. Hiera Configuration: <https://docs.puppet.com/hiera/3.2/configuring.html>
2. Hiera Lookup Functions: <https://docs.puppet.com/hiera/3.2/puppet.html#hiera-lookup-functions>
3. Hiera Automatic-Parameter Lookup: <https://docs.puppet.com/hiera/3.2/puppet.html#automatic-parameter-lookup>
4. External Facts: <http://docs.puppet.com/facter/3.4/custom_facts.html#external-facts>
5. What is Facter? <http://codingbee.net/tutorials/puppet/puppet-what-is-facter/>

---

<-- [Back to Contents](/README.md)

---

<-- [Back](07-Config-Hiera.md#lab-7)

---

### **Lab #8** - Environments

---

### Overview

Time to complete:  90 minutes

### Puppet Environments

We've already looked a little bit at puppet **environments** in
[Lab #5](05-Puppet-Config-and-Code.md), however we really haven't
talked about why you'd want to use them in too much detail.
Understanding environments and how puppet uses them will become very
important when we start to talk about R10K and Git in the coming labs.

So what exactly is a **Puppet Environment**?

PuppetLabs says this about environments:

>    A Puppet environment is an isolated set of Puppet manifests, modules, and data.
>
>    When a Puppet agent checks into a master and requests a catalog, it requests
>    that catalog from a specific environment.
>
>    Environments allow you to easily run different versions of Puppet code, so you
>    can test changes to that code without affecting all of your systems.

A Puppet Environment is just a container for puppet code.  You can have
multiple code trees for different sets of nodes.  Say you want all nodes of
**"Type A"** to use **"Code Tree A"** and all nodes of **"Type B"** to use
**"Code Tree B"**, you can do that with Puppet environments.

For example, imagine you are supporting two different customers with the
same PE infrastructure, and want to keep the code separate.

Or, imagine you want to create a temporary test environment to test some
code prior to promoting it up to the production environment.  This is a very
common use case, as we'll see...

A very useful way to envoke puppet from the command line is to
use the **double-dash-environment option**.  For example:

```
   puppet agent -t --environment=testing --noop
```

Even if the `puppet.conf` has `environment=production` in it, you can override
that by specifying an alternate environment from the command line.  This is
very useful if you have some test boxes that you want to test some new code
on.  In the above example we ran puppet against a **'testing'** environment, and
also specified **'--noop'** to ensure no changes are made to the host.  This way
we can see if the catalog compiles successfully, and if puppet wanted to make
the changes desired.

We're also going to introduce a new piece of software called **"R10K"**
(pronounced "Ar-Ten-Kay").  R10K will be coverd in a later lab along with
configuring a Git repository for our code.  For now, just know what R10K
does:  it simply pulls code from any number of Git repos, and drops it in
the correct location on your puppet master(s).  One useful feature of R10K
is that it maps every branch in the Git repo to a puppet environment.  So,
if you have a **'production'** branch in your Git repo, you will end up with a
**'production'** environment on your puppet master.  It's that simple.

Again, we will cover Git + R10K in a later lab.  I only introduce them breifly
here so that we can have the following discussion about **"Puppet Environments"**.
Going forward, I will simply write **environment** when talking about Puppet
Environments, as it gets old typing out **"Puppet Environemnts"** over and over.

### How to Use Puppet Environments

Again, we learned a little bit about environments and the `environment/`
directory structure in [Lab #5](05-Puppet-Config-and-Code.md), but we didn't
talk about when and why we'd use one.

There are at least a couple different ways to use environments.
The following two use cases are what I've seen out in the wild:

What I would call **"The Basic Use Case"** ...

  - All nodes are apart of the **production** environment (every `puppet.conf` would have `environment = production`)
  - New test environments are created by R10K dynamically, each environemnt corresponding to a Git branch (feature / temp branch)
  - These test environments are used to test code changes on a test node (could be a non-critical prod node, or a dedicated test node)
  - The tested code (that passed) would then be merged in to the production branch, and end up in the production environment via an R10K pull
  - At this point the new/changed code would be live in the production environment, and applied to all puppet-managed nodes
  - Delete the test (feature) branch in Git
  - Next time R10K runs, it would remove that test environemnt from the code tree on the master

So in the above use case, in addition to the "master" **production** environment, we may have
other puppet environments that are created dynamically on-the-fly by R10K.  In Git, they would be
short-lived feature branches that we introduce just for testing a new feature, or making a code
change. We'd test our changes on a non-production system, and once all bugs have been worked out,
we'd merge our feature branch in to the production branch.

What I would call **"The Multi-Customer Use Case"** ...

If your company has one team that supports multiple customers, you might very
well have one environemnt per customer.  The workflow would be identical to
the above **"Basic Use Case"** except you would have multiple production environments,
one for each customer.  The group that maintains the puppet infrastructure,
doesn't necessarily have to own the code in each of these environments either.

  1. internal_customer_A
  2. internal_customer_B
  3. internal_customer_C
  4. etc.

The internal customers might be different groups, or different divisions within
the same company.  All of these environments would still be "production"
environments.  They might be named something like:

  1. customer_A_prod
  2. customer_B_prod
  3. customer_C_prod
  4. etc.

Having separate environemnts like this, and using R10K, gives the ability to
have different groups maintain the code for each environemnt.  Depending on
the Git server you use, and the access controls available, you could even
restrict one environemnt to certain individuals (perhaps ones that signed
an NDA) within a group, and restrict a different environemnt to different
individuals within the same team.  Once we cover Git + R10K in more depth,
this will all start to make sense.

The above 2 use cases for environemnts are what I've seen used at the
companies I've worked at.  In fact, both use cases have been used simultaneously.

### Know the vocabulary and how different folks use it

There is the potential for some confusion when deciding how to build a new
puppet infrastructure for your company.  Every company is going to have
some vocabulary that is used around the shop regularly, and folks are
going to have an understanding of certain words that might only make
sense at that company.  One such word is **"environment"**.

One company might use names like "Lab Environment" or "Prod Environment",
etc., while another company might refer to their "Private Cloud Environment",
or "Hybrid Cloud Environemnt", etc.  It's very common for a Dev Shop to
have "environments" like:

  1. Development
  2. Integration
  3. Staging
  4. Production

...but are they really environments?  Maybe.  Do they correspond to different
sets of servers?  Or are they simply different branches in a Git repo?  You
have to remember, what are we using Puppet to manage?  Sets of servers. Do
these "Development Environments" match up to different sets of servers with
different config for each?  Probably not entirely.  You might see some
integration servers that QA/Test folks run their test on.  You might see
some staging servers where UAT is done.  But the important thing to keep in
mind is what servers/nodes you're managing, and how the configuration will
be different for each set.

Example:  A SaaS company that is releasing a Web/Java app every month
may have their Integration, Staging, and Production servers.  All of
these servers are identical with the exception of the users that are
allowed to login to them.  Do you need to create entirely separate
environments just to deal with authentication?  Probably not.

Do you need to be able to upgrade a puppet module and test it first
in the integration and/or staging environments first, before promoting
to production?

You really have to attack this issue from the perspective of what data
you want to manage, and what your workflow will be.  It's not a simple
answer.  It's probably the most difficult question to answer when you're
in the position of designing your Puppet infrastructure from scratch.

What I'd suggest is keep it simple, and then modify your puppet code
as needed to acomodate a new use case.  Do your best to predict how
you need Puppet to support your existing infrastructure, but keep in
mind there's more than one way to do things. (E.g. use a custom Hiera
data key to assign an application tier, division, workgroup, etc.,
and make decisions in your profile code based on that, rather than
abusing the puppet environments feature.)

From my own experience, I prefer the "Basic Use Case" I outlined above.
I've used it at multiple companies, and it works well.  I would advise
against using "static environments" that matches your development code
flow (Dev, Int, Stg, Prod).  As you'll see later on, having long-lived
branches in Git can cause you some **"Merge Agnst"**, if for example
you want to maintain permanent differences between each branch.

### Let's do something with Puppet Environments

Okay, so we've talked a lot about environments, but we haven't actually
done anything to show how they work.  So let's get our hands dirty
and try playing around with these environment things...

Remember that we manually created the **"development"** environment
in a previous Lab?  Well, let's try to run puppt against it on our
**agent** node:

```
[root@agent ~]# puppet agent -t --environment=development --noop
Warning: Local environment: "development" doesn't match server specified node environment "production", switching agent to "production".
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for agent.example.com
Info: Applying configuration version '1477337483'
Notice: /Stage[main]/Main/Notify[Location is: amsterdam]/message: current_value absent, should be Location is: amsterdam (noop)
Notice: Class[Main]: Would have triggered 'refresh' from 1 events
Notice: Stage[main]: Would have triggered 'refresh' from 1 events
Notice: Finished catalog run in 10.24 seconds
```

We got a warning!

```
Warning: Local environment: "development" doesn't match server specified node environment "production", switching agent to "production".
```

So how are we suposed to implement the code/dev/test/promote workflow we
presented previously?

There's a couple ways we can approach this:

1. Add our **agent.example.com** node to the **"Agent-specified environment"** node group via the PE Console
2. Disable the PE Console as the node classifier entirely

For the purposes of this example, we will go with option 1.


### Add node to Agent-specified environment node group

Let's do this:

1. Get logged into the PE console as the admin user.
2. Click on **Nodes** in the left-sidebar
2. Click on **Classification** sub-category
3. Drill into the **Production environment**
3. Click on **"Agent-specified environment"**
4. In the bottom section where you see **"Certname"** click and select **agent.example.com**
5. On the right, click **Pin Node**
6. Then in the bottom right corner, click **Commit**

That's all there is to it!  Your node **agent.example.com** will now be able to either specify its environment in its `puppet.conf` or from the command line, and the Puppet Master will gladly accept it.

![Agent-specified Environment](images/Agent-specified-environment.png)

Go ahead and try running puppet against the development environment again...

```
     [root@agent ~]# puppet agent -t --environment=development
     Info: Using configured environment 'development'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for agent.example.com
     Info: Applying configuration version '1479250438'
     Notice: Applied catalog in 0.57 seconds
```

It worked just fine!

But, remember that all of the code changes we made previously to manage the
hosts file, ntp and timezone config, is only in the production environment,
so when we run against the development environment, none of those things will
be enforced.  Give it a try by making a change to the /etc/hosts file, then
running puppet against the development environment, and note that nothing
happened to revert your change.  This is exactly what we'd expect.

### Our Development Environment

What if we want to introduce a new module in to our development environment,
but not production?

Take a look at what modules are already installed:

```
[root@puppet ~]# puppet module list --environment=development

/etc/puppetlabs/code/environments/development/modules
└── puppetlabs-stdlib (v4.9.1)
/etc/puppetlabs/code/modules
└── puppetlabs-stdlib (v4.12.0)
/opt/puppetlabs/puppet/modules
├── puppetlabs-pe_accounts (v2.0.2-6-gd2f698c)
├── puppetlabs-pe_concat (v1.1.2-7-g77ec55b)
├── puppetlabs-pe_console_prune (v0.1.1-9-gfc256c0)
├── puppetlabs-pe_hocon (v2016.2.0)
├── puppetlabs-pe_infrastructure (v2016.4.0)
├── puppetlabs-pe_inifile (v2016.2.1-rc0)
├── puppetlabs-pe_java_ks (v1.2.4-37-g2d86015)
├── puppetlabs-pe_nginx (v2016.4.0)
├── puppetlabs-pe_postgresql (v2016.2.0)
├── puppetlabs-pe_puppet_authorization (v2016.2.0-rc1)
├── puppetlabs-pe_r10k (v2016.2.0)
├── puppetlabs-pe_razor (v1.0.0)
├── puppetlabs-pe_repo (v2016.4.0)
├── puppetlabs-pe_staging (v2015.3.0)
└── puppetlabs-puppet_enterprise (v2016.4.0)
```

Notice that we already have the **puppetlabs-stdlib** installed (v4.9.1) in
the development environment.  Let's install another module in the development
environment only.

The **message of the day** module is very simple, so let's test it out.

It can be found on the **Puppet Forge** here:  https://forge.puppet.com/puppetlabs/motd/readme

Install it on your puppet master for the development environment only, as follows:

```
[root@puppet ~]# puppet module install --environment development puppetlabs/motd
Notice: Preparing to install into /etc/puppetlabs/code/environments/development/modules ...
Notice: Downloading from https://forgeapi.puppetlabs.com ...
Notice: Installing -- do not interrupt ...
/etc/puppetlabs/code/environments/development/modules
└─┬ puppetlabs-motd (v1.4.0)
  └─┬ puppetlabs-registry (v1.1.3)
    └── puppetlabs-stdlib (v4.9.1)
```

Okay, the puppetlabs-motd module has been installed in our development environment.

Take a look at the /etc/motd file on your agent node just to see what it contains out of the box:

```
[root@agent ~]# ls -al /etc/motd
-rw-r--r--. 1 root root 0 Jun  7  2013 /etc/motd
[root@agent ~]# cat /etc/motd
```

Nothing!  It's just an empty file.  Okay then, let's use our new module to create our motd.

To tell puppet to apply that module to our agent node, we need to classify it.  Let's copy over
the various files we've already created in the production environment to get us started, and 
we'll modify things from there...

```
[root@puppet manifests]# cd /etc/puppetlabs/code/environments/
[root@puppet environments]# cp -r production/hieradata development/
[root@puppet environments]# cp -r production/manifests/ development/
[root@puppet environments]# chown -R pe-puppet:pe-puppet development
[root@puppet environments]# chmod a+rX development

```

Okay, now we have all of the Hiera data and puppet code that we created for the production environment sitting in the development environment as well.

Now run puppet on the agent node and see what happens:

```
[root@agent ~]# puppet agent -t --environment development
Info: Using configured environment 'development'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Error: Could not retrieve catalog from remote server: Error 500 on SERVER: Server Error: Evaluation Error: Error while evaluating a Function Call, Could not find class ::ntp for agent.example.com at /etc/puppetlabs/code/environments/development/manifests/site.pp:42:1 on node agent.example.com
Warning: Not using cache on failed catalog
Error: Could not retrieve catalog; skipping run

```

Why did we get an error?

Remember that when we installed the **ntp** and **timezone** modules, we only installed them for the production environment.
Do you remember why that is?  Because puppet will install modules, by default, in the **first element of the modulepath.**

```
[root@puppet environments]# puppet config print modulepath
/etc/puppetlabs/puppet/environments/production/modules
```

Take a peek again at where the ntp and timezone modules are installed:

```
     [root@puppet environments]# puppet module list --environment=production
     /etc/puppetlabs/code/environments/production/modules
     ├── puppetlabs-ntp (v6.0.0)
     ├── puppetlabs-stdlib (v4.13.1)
     └── saz-timezone (v3.3.0)
     /etc/puppetlabs/code/modules
     └── puppetlabs-stdlib (v4.12.0)
     [snip]
```
Confirm that you do not see the ntp and timezone modules in the development:

```
     [root@puppet environments]# puppet module list --environment=development
     /etc/puppetlabs/code/environments/development/modules
     ├── puppetlabs-motd (v1.4.0)
     ├── puppetlabs-registry (v1.1.3)
     └── puppetlabs-stdlib (v4.9.1)
     /etc/puppetlabs/code/modules
     └── puppetlabs-stdlib (v4.12.0)
     [snip]
```

Okay, at this point we have a couple choices to fix our puppet run:

1. Un-classify the ntp and timezone modules for the agent node
2. Install those modules in the development environment

Let's just install the modules:

```
     [root@puppet environments]# puppet module install --environment development puppetlabs/ntp
     Notice: Preparing to install into /etc/puppetlabs/code/environments/development/modules ...
     Notice: Downloading from https://forgeapi.puppetlabs.com ...
     Notice: Installing -- do not interrupt ...
     /etc/puppetlabs/code/environments/development/modules
     └─┬ puppetlabs-ntp (v6.0.0)
       └── puppetlabs-stdlib (v4.9.1 -> v4.13.1)

     [root@puppet environments]# puppet module install --environment development saz-timezone
     Notice: Preparing to install into /etc/puppetlabs/code/environments/development/modules ...
     Notice: Downloading from https://forgeapi.puppetlabs.com ...
     Notice: Installing -- do not interrupt ...
     /etc/puppetlabs/code/environments/development/modules
     └─┬ saz-timezone (v3.3.0)
       └── puppetlabs-stdlib (v4.13.1)

     [root@puppet environments]# puppet module list --environment=development
     /etc/puppetlabs/code/environments/development/modules
     ├── puppetlabs-motd (v1.4.0)
     ├── puppetlabs-ntp (v6.0.0)
     ├── puppetlabs-registry (v1.1.3)
     ├── puppetlabs-stdlib (v4.13.1)
     └── saz-timezone (v3.3.0)
     /etc/puppetlabs/code/modules
     └── puppetlabs-stdlib (v4.12.0)
     [snip]
```

Okay, they're installed, so let's try our puppet run again:

```
[root@agent ~]# puppet agent -t --environment=development
Info: Using configured environment 'development'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Notice: /File[/opt/puppetlabs/puppet/cache/lib/facter/facter_dot_d.rb]/content:
[snip]
Info: Loading facts
Info: Caching catalog for agent.example.com
Info: Applying configuration version '1479251399'
Notice: Location is: amsterdam
Notice: /Stage[main]/Main/Notify[Location is: amsterdam]/message: defined 'message' as 'Location is: amsterdam'
Notice: Applied catalog in 0.75 seconds
```

Great, no errors!

Now, let's proceed to classify our node to use the **motd** module...

```
[root@puppet environments]# pwd
/etc/puppetlabs/code/environments
[root@puppet environments]# cd development/hieradata/node/
[root@puppet node]# vi agent.example.com.yaml
```

Edit the `agent.example.com.yaml` Hiera data to look like:

```
---

location: 'amsterdam'

classes:
   - ntp
   - timezone
   - motd

timezone::timezone: 'US/Pacific'
```

Now run puppet again...

```
     [root@agent ~]# puppet agent -t --environment=development
     Info: Using configured environment 'development'
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/registry_key]/ensure: created
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/registry_key/registry.rb]/ensure: defined content as '{md5}5c26c6cbb1669a01361e69fadbbb408d'
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/registry_value]/ensure: created
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/provider/registry_value/registry.rb]/ensure: defined content as '{md5}92b54f65f5be3c130681cbb04b216e09'
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/registry_key.rb]/ensure: defined content as '{md5}bcf74b3a991cafdae54514b3c3c4a38c'
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet/type/registry_value.rb]/ensure: defined content as '{md5}140295468b773a7ad709a532e496005c'
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet_x]/ensure: created
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet_x/puppetlabs]/ensure: created
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet_x/puppetlabs/registry]/ensure: created
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet_x/puppetlabs/registry.rb]/ensure: defined content as '{md5}e81ba7665aedbd5cb519d75a4a8dbbd2'
     Notice: /File[/opt/puppetlabs/puppet/cache/lib/puppet_x/puppetlabs/registry/provider_base.rb]/ensure: defined content as '{md5}7d511743b5cec6b375b684b88396838a'
     Info: Loading facts
     Info: Caching catalog for agent.example.com
     Info: Applying configuration version '1479251540'
     Notice: Location is: amsterdam
     Notice: /Stage[main]/Main/Notify[Location is: amsterdam]/message: defined 'message' as 'Location is: amsterdam'
     Notice: /Stage[main]/Motd/File[/etc/motd]/content:
     --- /etc/motd    2013-06-07 07:31:32.000000000 -0700
     +++ /tmp/puppet-file20161115-9822-o29juw    2016-11-15 15:12:22.973018511 -0800
     @@ -0,0 +1,3 @@
     +The operating system is CentOS
     +The free memory is 200.24 MiB
     +The domain is example.com

     Notice: /Stage[main]/Motd/File[/etc/motd]/content: content changed '{md5}d41d8cd98f00b204e9800998ecf8427e' to '{md5}a6ecca768e0149d03268b441b9774863'
     Notice: Applied catalog in 0.80 seconds
```

It did something!

Let's take a look at the contents of the motd file again...

```
[root@agent ~]# cat /etc/motd
The operating system is CentOS
The free memory is 200.24 MiB
The domain is example.com
```

Okay, now we see that the motd module wrote new content within the /etc/motd file.

What if we want to customize how our motd looks?

```
[root@puppet node]# cd /etc/puppetlabs/code/environments/development/modules/motd/templates
[root@puppet templates]# ls -al
total 12
drwxr-xr-x 2 pe-puppet pe-puppet   36 Nov 15 14:56 .
drwxr-xr-x 6 pe-puppet pe-puppet 4096 Jan 26  2016 ..
-rw-r--r-- 1 pe-puppet pe-puppet  146 Jan 26  2016 motd.erb
-rw-r--r-- 1 pe-puppet pe-puppet   24 Jan 26  2016 spec.erb

[root@puppet templates]# vi motd.erb
```

Let's edit the template to look like this:

```
###########################################################################

     OS Distro: <%= @operatingsystem %> <%= @operatingsystemrelease %>

     MEMORY:
          Total: <%= @memory['system']['total'] %>
      Available: <%= @memory['system']['available'] %>
      Swap used: <%= @memory['swap']['used'] %>

     <%- if @domain -%>
     My domain is <%= @domain %>
     <%- end -%>
     My location is: <%= @location %>

###########################################################################

```

This is the first time we've seen a template.  A template is simply a text file
that is pre-processed before being deployed to the agent node.  We'll talk more
about the ERB template syntax in a later lab.

Now, save your template, and run puppet again:

```
[root@agent ~]# puppet agent -t --environment=development
Info: Using configured environment 'development'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for agent.example.com
Info: Applying configuration version '1479253813'
Notice: Location is: amsterdam
Notice: /Stage[main]/Main/Notify[Location is: amsterdam]/message: defined 'message' as 'Location is: amsterdam'
Notice: /Stage[main]/Motd/File[/etc/motd]/content:
--- /etc/motd    2016-11-15 15:50:07.451576603 -0800
+++ /tmp/puppet-file20161115-11572-vzevz9    2016-11-15 15:50:16.274392880 -0800
@@ -0,0 +1,14 @@
+###########################################################################
+
+     OS Distro: CentOS 7.2.1511
+
+     MEMORY:
+          Total: 489.03 MiB
+      Available: 200.29 MiB
+      Swap used: 18.31 MiB
+
+     My domain is example.com
+     My location is: amsterdam
+
+###########################################################################
+

Notice: /Stage[main]/Motd/File[/etc/motd]/content:

Notice: /Stage[main]/Motd/File[/etc/motd]/content: content changed '{md5}d41d8cd98f00b204e9800998ecf8427e' to '{md5}ed131fae16df0e72f1a6144d053fa6cd'
Notice: Applied catalog in 0.78 seconds
```

Notice that your /etc/motd has been updated with the new content.

So how does this help us understand environments?

Notice that the **puppetlab-motd** module is only installed in the development
environment, so we're able to test it out without affecting the production
environment.

Now that we've tested our new module, and are confident that it works the way
we want, how would we promote it to production?

We'd need to:

1.  Make sure the module that we've tested against is also installed in production
2.  Classify nodes that we want to receive the motd

Since we've not yet introduced Git, we do this manually simply by installing the module
manually on the master (just as we have been doing), and then either copying or editing
the Hiera data in the production environemnt.  As you can see, this will get very
cumbersome very quickly, and why we want to be using a version control system such as Git.
We will setup a Git server and learn more about Git in the next lab, but for now, let's
go through the process of getting our new motd module and template activated within the
production environment:

```
[root@puppet puppet]# puppet module install --environment production puppetlabs/motd
Notice: Preparing to install into /etc/puppetlabs/puppet/environments/production/modules ...
Notice: Downloading from https://forgeapi.puppetlabs.com ...
Notice: Installing -- do not interrupt ...
/etc/puppetlabs/puppet/environments/production/modules
└─┬ puppetlabs-motd (v1.4.0)
  └─┬ puppetlabs-registry (v1.1.3)
    └── puppetlabs-stdlib (v4.13.1)
```

Next, let's copy over our template:

```
[root@puppet ~]# cd /etc/puppetlabs/code/environments
[root@puppet environments]# cp development/modules/motd/templates/motd.erb production/modules/motd/templates/motd.erb
cp: overwrite ‘production/modules/motd/templates/motd.erb’? y
```

Next, edit our Hiera data to classify all nodes...

```
[root@puppet environments]# cd production/hieradata
[root@puppet hieradata]# vi common.yaml
```

Let's classify all nodes to use this motd in our common.yaml

```
---

classes:
   - common_hosts
   - common_packages
   - motd

ntp::servers:
   - '0.pool.ntp.org'
   - '1.pool.ntp.org'
   - '2.pool.ntp.org'
   - '3.pool.ntp.org'

```

Run puppet on both the **puppet** master and the **agent** node, and you should see the /etc/motd get updated.

### Wrapping it up

We've seen how we can use two environments to test a new module, and then promote the code changes from one
to the other (development to production in this case).  We've also seen how cumbersome and prone to error it
can be with many manual steps of editing and copying files from one environment to the other.

In the following labs we will see how we can use a Git Hosting server called **GitLab** along with **R10K**
to automate the whole workflow, and make it way more fun!

---

Continue on to **Lab #9** --> [Install GitLab](09-Install-GitLab.md)

---

### More Reading

Gary Larizza did a nice write-up on this issue here:  <http://garylarizza.com/blog/2014/03/26/random-r10k-workflow-ideas/>

Gary Larizza Video Presentation:  <https://puppetlabs.com/webinars/git-workflow-best-practices-deploying-r10k>

Here's a PuppetLabs write-up, though a bit outdated (pre-R10K), there are a lot of interesting comments at the end:   <https://puppetlabs.com/blog/git-workflow-and-puppet-environments>

Here's the PuppetLabs write-up including R10K in the workflow:  <https://puppetlabs.com/blog/git-workflows-puppet-and-r10k>

---

<-- [Back to Contents](/README.md)

---

<-- Back to **Lab #8** - [More about Environments](08-Environments.md#lab-8)

---

### **Lab #9** - Install GitLab

---

### Overview ###

Note:  if you're running with less than 10GB of memory (like on an 8GB system), you may want to start only the **puppet** and **gitlab** virtual machines for this lab to save memory (don't start the **agent** VM, as we wont need it.)
When running all 3 VM's on an 8GB system, you may start to swap and your workstation may start to crawl and/or freeze up...Close any applications you don't need running to help conserve memory, or get a better workstations.
Geesh.

### Start up your GitLab VM

You should have already created your **gitlab** VM, but if not, go ahead and do it now...

```
     vagrant up gitlab     # bring up your VM
     vagrant ssh gitlab    # ssh in to get a shell
     sudo su -             # become root
```

### Install the Puppet Agent

Although not required for our training environment, let's install the Puppet
Agent on our GitLab VM so that our timezone is set, and NTP is configured to
run.  Review [Lab #4](04-Install-Puppet-Agent.md) for more details, but it's
basically just this:

```
     curl -k --tlsv1 https://puppet:8140/packages/current/install.bash | bash -s main:certname=gitlab.example.com
```

Sign the cert on the puppet master, and then run **puppet agent -t** on gitlab...

```
     Error: Could not retrieve catalog from remote server: Error 500 on SERVER:
     Server Error: Evaluation Error: Error while evaluating a Function Call, Could
     not find data item location in any Hiera data file and no default supplied at
     /etc/puppetlabs/code/environments/production/manifests/site.pp:31:13 on node
     gitlab.example.com
```

We need to create a node yaml file for **gitlab.example.com** on the puppet master...

```
[root@puppet ~]# cd /etc/puppetlabs/code/environments/production/hieradata/node/
[root@puppet node]# cp agent.example.com.yaml gitlab.example.com.yaml
[root@puppet node]# cat gitlab.example.com.yaml
---

location: amsterdam

classes:
   - ntp
   - timezone

timezone::timezone: 'US/Pacific'
```

That should be fine for now...Run puppet again and you should get a clean run...



### GitLab Installation Instructions

A summary of the minimal installation structions for GitLab follow...
(Full instructions are [here](https://about.gitlab.com/downloads/#centos7))

- Install and configure the necessary dependencies

```
     # Postfix should already be installed and running, but just in case...
     puppet resource package postfix ensure=present
     puppet resource service postfix ensure=running enable=true
     
     # The above puppet commands do the equivalent of the following, but abstract away from the OS flavor:
     yum install -y postfix
     systemctl enable postfix
     systemctl start postfix
```

- Open the host firewall to allow in-bound HTTP.  Although GitLab uses HTTP by default, it is possible to re-configure it to use HTTPS.  We won't go through that process in this training, but you may find the instructions [HERE](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/doc/settings/nginx.md)

```
     # There is a firewall module that could help us do this with Puppet itself, but for now, let's just be lazy and...
     # Let inbound HTTP through the firewall so we can hit the GitLab web interface
     firewall-cmd --permanent --add-service=http
     systemctl reload firewalld
```

- Add the GitLab package server and install the package

```
     curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | bash
     yum install -y gitlab-ce
```

- Configure and start GitLab

```
     gitlab-ctl reconfigure
```

- Browse to <http://127.0.0.1:24080/> and login
     - You'll be prompted to change the root password, so go ahead and do that.
     - Then login with:
       - Username: root
       - Password: `<the password you just set>`

- As the 'root' user, create a non-root-user account for yourself
     - Click the `wrench` icon in the top-right corner called the **Admin Area**
     - Click **Users** sub-tab in the **Overview** section
     - Click **New User** in the main pane and fill out the required info
       - Use a valid email, as the account creation process will send you a password reset link
       - In the **Access** section, check the **Admin** box to make this account and admin account
       - Also make sure `Can create a group` is checked
     - Click on the **Create User** button at the bottom of the form
     - Logout the root user
     - You should receive an email to the e-mail address you provided in the account creation form
       - The link to set your password will not work, but you may copy it and edit it to point to localhost like this:
       ```
       http://localhost:24080/users/password/edit?reset_password_token=Zo9b6dFY4Ld7YvvW7iCC
       ```
     - Next try logging in as yourself using the account you just created and set the password for
       - Note:  if unable to get the reset link to work, you can also set the password via the webGUI as the root user
       - Click on the **Wrench** icon again
       - Click **Users**
       - Click the **Edit** button next to your account
       - Scroll a bit down, and you'll find where you can set your password
       - Enter your choice of password twice, and click **Save Changes**

- Once your are logged in as yourself (as the user you just created) the continue...

- Click the GitLab icon top/center (the Orange Origami Fox Head thingy) to get to the Dashboard

- Create a group called `puppet`
     - Click on **New Group**
     - Enter the group name `puppet`
     - Select **Public**
     - click **Create Group**
     - Note: since you've created the group, you are the **Owner** and will have access to any projects (repos) within that group

- Create a project called `control` and set the Namespace to be the `puppet` group
     - Click **New Project** and change the Namespace to `puppet` instead of yourself
     - Select **Public** visibility here too
     - The remaining options can be left at their defaults
     - Click **Create Project**

-  Configure your GitLab account to allow you to clone/pull/push to the **puppet/control** repo
     - You should see a notice that says *"You won't be able to pull or push project code via SSH until you add an SSH key to your profile"*
     - Click the link [add an SSH key](http://127.0.0.1:24080/profile/keys)
     - If you already have your own personal public/private key pair for SSH, you may use it
     - If you don't already have a key, you can generate one with `ssh-keygen`
     - Add your **public** key to your GitLab account under **Profile Settings -> SSH Keys**
     - Add a config section to your ~/.ssh/config to tell ssh what **private** key to use, as well as what user and port

```
  Host localhost
    User git
    Port 24022
    IdentityFile /Users/Mark/.ssh/id_rsa.gitlab-mark
```

You should now be able to clone your repo. But first, make sure you've set your Name and Email in your Git client config so that your commits show who you are:

For example:

```
git config --global user.name "Mark Bentley"
git config --global user.email "bentlema@yahoo.com"
```

Next, try to clone the [puppet/control](http://127.0.0.1:24080/puppet/control) repo from GitLab:

```
[/Users/Mark/Puppet-Tutorial] $ mkdir -p gitlab/puppet
[/Users/Mark/Puppet-Tutorial] $ cd gitlab/puppet
[/Users/Mark/Puppet-Tutorial/gitlab/puppet] $ git clone ssh://localhost/puppet/control.git
Cloning into 'control'...
The authenticity of host '[localhost]:24022 ([127.0.0.1]:24022)' can't be established.
RSA key fingerprint is 25:cb:6c:9c:da:4e:6f:46:72:75:46:ac:18:19:31:ee.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[localhost]:24022' (RSA) to the list of known hosts.
warning: You appear to have cloned an empty repository.
Checking connectivity... done.

[/Users/Mark/Puppet-Tutorial/gitlab/puppet] $ cd control
[/Users/Mark/Puppet-Tutorial/gitlab/puppet/control] $ git remote -v
origin  ssh://localhost/puppet/control.git (fetch)
origin  ssh://localhost/puppet/control.git (push)

[/Users/Mark/Puppet-Tutorial/gitlab/puppet/control] $ vi README.md
```

Put some text in to your README.md such as 'Hello World' then add that file and commit it.

```
[/Users/Mark/Puppet-Tutorial/gitlab/puppet/control] *$ git add README.md

[/Users/Mark/Puppet-Tutorial/gitlab/puppet/control] *$ git commit -m 'First commit'
[master (root-commit) 276c800] First commit
 1 file changed, 2 insertions(+)
 create mode 100644 README.md

[/Users/Mark/Puppet-Tutorial/gitlab/puppet/control] (master)$ git push
Counting objects: 3, done.
Writing objects: 100% (3/3), 231 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To ssh://localhost/puppet/control.git
 * [new branch]      master -> master
```

Now go back to the GitLab webGUI and take a look at the file you just pushed.

You'll have to explore the GitLab WebGUI a bit, and figure out things on your own.
Some key things:

* Click on the orange oragami fox head icon to return to the main dashboard
* Click on [puppet/control](http://127.0.0.1:24080/puppet/control) to see your new empty Puppet "Control Repo"
* Click on the sub-tab [Files](http://127.0.0.1:24080/puppet/control/tree/master) to see the files in the repo
* Click on the sub-tab [Commits](http://127.0.0.1:24080/puppet/control/commits/master) to see individual commit history
* Click on an individual commit to see what changed

At this point you have a GitLab server running, and ready to be used.  You have a single
group called 'puppet' containing a single project (or repo) called 'control'.  In the next
lab we will work to move our puppet code in to GitLab so we don't have to make manual
edits on the puppet master itself.

---

Continue on to **Lab #10** --> [Move Puppet Code under Git Control](10-Move-Puppet-Code-to-GitLab.md#lab-10)

---

See Also:

    - Installation Instructions:      <https://about.gitlab.com/downloads/#centos7>
    - GitLab Omnibus Documentation:   <http://doc.gitlab.com/omnibus/>
    - Configure GitLab to use HTTPS:  <https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/doc/settings/nginx.md>

---

<-- [Back to Contents](/README.md)

---

<-- [Back](09-Install-GitLab.md#lab-9)

---

### **Lab #10** - Move Puppet Code Under GitLab Control

---

### Overview

We've just setup GitLab in [Lab #9](09-Install-GitLab.md#lab-9) and now we
want to be able to use it.  We've already written some puppet code, and it's
currently sitting in `/etc/puppetlabs/code/environments` on our puppet
master node.  So we have a few questions:

- How can we get this code in to GitLab?
- Once it's in GitLab, how do we pull it back out into the correct place?
- Can we do this without any puppet outage?

We're going to walk through a process to copy our code to the **puppet/control**
repo that we've already cloned to our workstation, and then push it up to
GitLab.  Following that we will configure R10K to pull down our code from
GitLab, and drop it (along with all of the modules we're using) in the
correct place.  All of this will be done without seeing a failed puppet run
(say if one of the agents runs in the background while we're doing this work.)

But first, let's talk a bit more about Git commands and concepts...

### Some Git commands and concepts

What is a **Push**? **Pull**? What does it mean to **"clone a repo"**?  What is Git exactly?
We're going to talk more about Git commands in [Lab #12](12-Git-Basics.md), but we
need to know a few Git commands for this lab, so let's talk about them now.

What is Git?  Simply put, Git is a distributed version control system.
Git can be thought about like it's a mini-filesystem with snapshot capabilities,
and all of the Git commands you utilize manipulate this **"mini-filesystems"**
and its **"stream of snapshots"**.

Git is the tool that will...

- track changes to your code (audit trail)
- allow multiple code-maintainers to make changes simultaneously, and resolve conflicts
- allow multiple complete copies (or **"clones"**) of a repo which can be sync'ed up

Let's go over some of the basic Git concepts and commands we will see in this lab...

### Git Concept: The Git Repository

A **"Git Repo"** is just just a self-contained bundle of files along with its commit history.
As mentioned previously, this **commit history** is like a stream of snapshots.
Typically the files in a Git Repo are source code--in our case: Puppet Manifests.
However, a Git repo can contain any text and/or binary files.  Usually binary files
would be excluded from the repo, as you're probably not interested in versioning
them (e.g. they might be object files left from a compile/software build).  In any
case, Git is happy to store versions of any type of file you might want in your repo.

You can find many public Git repositories out on <https://github.com/explore> but
remember that the nice WebGUI is not the repo itself, it's just a **nice view** into the
repo.  GitHub is a Git server (or **"Git Hosting Service"**) sitting in front of the
actual Git repo--actually thousands of Git repos.  There are other Git servers out
there too, where both public and private repos can be hosted, and access-controled.
We are using one called GitLab for this training.

Some other well known Git servers available are:

- [GitHub](http://github.com)
- [Atlassian BitBucket](https://bitbucket.org/)
- [Gitolite](http://gitolite.com/)

### Git Command:  git clone

To make a complete copy of a remote repo, you can use `git clone <URL>`

- It makes an exact clone of the remote Git repository.
- Git will also setup a **remote** for pushing to and pulling from
- Git will also configure this remote as a **tracking branch**

The **remote** is just a meaningful name given to the remote URL which you can refer
to with other git commands.  Git will setup a remote with the name **origin** by default.

Type `git remote -v` to see the configured remote of your control repo.

Git will also use **origin** as the default for other commands if you don't
specify a remote.

### Git Command: git status

The `git status` command is useful for telling you what branch you're on, as
well as the status of the staging area.  When you make changes to files that
Git is tracking, it will notice that, and show you that it noticed.

### Git Concept: The Staging Area

Git has something called a **"Staging Area"** (sometimes referred to as
**"The Index"**).  It's where we can "stage" our changes in preperation
to permanently commit them to the repo.  Once you commit a change, it's
there forever in the commit history.

### Git Command: git add

To add a file to the staging area, use the `git add` command.  We can use
this command to add files to the staging area prior to commiting them.

### Git Command: git commit

Now that we're run the `git add` on a file, and the `git status` shows that
it's staged (ready to be committed), we can go ahead and either:

1. commit the change
2. un-stage the change

A typical commit would look like this:  `git commit -m 'some comment'`

The **-m** option is short for **commit message** and is just a descriptive message
to go along with the commit in case we need to find it in the future, the message
should make it easier to identify later what changes were made

### Git Command: git push

Remember earlier we looked at `git clone` and mentioned that when we clone a
repo, Git will automatically setup the remote tracking branch.  Git is comparing
our repo's branch with what it knows about the remote repo's branch, and it will
notice when we have made a new commit, but the remote doesn't yet have that commit.

We need to `git push` to send our local commits to the remote repository.

### Git Command: git pull

If you have multiple people working in the same repo, they will also have a
clone on their local workstation, and will also be making changes and
adding/committing/pushing up to the remote.  If you're both working on the
same files, it's possible you could have an outdated copy if the other person
had edited a file and pushed their change to the remote after you cloned it.
So how to we keep our **local** clone up-to-date with the **remote**?

A good habit to get into is to also do a `git pull` prior to doing any work
in your local clone of a repo.  This ensures you're pulling down any changes
other folks have made, and will potentially avoid merge conflicts in the future.

When you do a `git pull` Git will pull down changes to the current branch,
and bring your branch up-to-date with the remote.  Git can also be configured
to fetch all changes in other branches as well.  Depending on the version of
Git your using, this behavior may differ, so just to be safe, it's also good
to `git pull` after switching to a different branch.

### Git Concept: Git Branches

Although we've seen **branches** a little bit (e.g. **production branch**) we've
not really talked about what a branch is.  Git allows us to spin off a copy of
our repo, makes changes within that copy (called a branch), and then either
merge our changes up to the parent branch, or discard our changes.  This idea
becomes very useful when making changes to puppet code without affecting the
production environment, but still allowing us to test our code.

### Git Command: git checkout

The easiest way to create a new branch is to use `git checkout -b <branch-name>`

It's a shortcut for creating a branch, and then checking out that branch.

The longer version would be a `git branch <branch-name>`
followed by `git checkout <branch-name>` but why type all of that?

### Okay, shifting gears...

With these Git concepts and commands in mind, let's work on getting our existing
Puppet code into GitLab (our Git hosting server).

GitLab provides the authentication/authorization framework around our puppet/control repo.
We've previously setup our own account, and cloned the puppet/control repo to our
workstation, but we will also need to allow the root user on our puppet master access
to clone the control repo as well, as this repo is where we will store some of our
puppet code and hiera data.

### Let's Setup root SSH access to GitLab

Since Puppet runs as root, and needs to pull code out of GitLab, we need to configure
the root account on our Puppet Master with read-only access to the control repo.

Once this is done, we
will take advantage of a **puppet.conf** config item called **postrun_command** which
will run a command or script after every Puppet agent run.  Since the Puppet agent
runs every 30 minutes, so would our postrun_command.  We will set our postrun_command
to run R10K.  It goes like this:

- Puppet executes the **postrun_command** as root every 30 minutes (by default)
- We use **postrun_command** to run `r10k` to build puppet environments from Git branches
- R10K uses the git command to clone and pull repos down to the puppet master
- The git command will use ssh keys to authenticate with the GitLab server

So, if we want to use R10K to pull our code down, we need to give root the ability
to use git to pull code from our gitlab host.

Let's setup the **root** user on the puppet master to be able to
**clone** the **puppet/control repo** which we setup in the previous lab. If we're
able to manually clone the repo as the root user, than so should R10K be able to.

First, as root on your **puppet** VM, generate a new passphraseless keypair as follows:

```
[root@puppet ~]# ssh-keygen -t rsa -b 2048 -N '' -C 'root@puppet - Used by R10K to pull from GitLab'
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
e1:e3:ce:c4:a3:e7:f8:17:dc:f8:7e:fa:f2:c1:3d:c3 root@puppet - Used by R10K to pull from GitLab
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|                 |
|        .        |
|       . .       |
|        S. o     |
|       o .+ .... |
|        =  o  oE.|
|       *... o ..o|
|      o+=. .o*o  |
+-----------------+
```

r10k will need to be able to pull from GitLab without having to enter a password, so leave the **passphrase** empty.
(That's what the `-N ''` option does, so you shouldn't even be prompted to provide a passphrase.)

Next, within the GitLab WebGUI

- Go to your **[puppet/control](http://127.0.0.1:24080/puppet/control)** project (repo)
- Find and click the **'Settings Gear Icon'** (should be in top/right)
- Click on **[Deploy Keys](http://127.0.0.1:24080/puppet/control/deploy_keys)**
- Enter a meaningful title like **'Used by R10K to pull from GitLab'**
- Copy the **public key** you just generated from `/root/.ssh/id_rsa.pub` on your puppet master
- Paste the key in to the **Key** dialog box
- Save it by clicking **'Add Key'**

At this point, the root user on the **puppet* VM should be able to clone the repo.

### Test that the root user can clone the puppet/control repo

```
[root@puppet .ssh]# cd /tmp
[root@puppet tmp]# git clone ssh://git@gitlab/puppet/control.git
Cloning into 'control'...
The authenticity of host 'gitlab (192.168.198.11)' can't be established.
ECDSA key fingerprint is 39:e5:9b:0d:8b:bd:74:0a:12:e8:c6:37:cb:cf:17:c3.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'gitlab,192.168.198.11' (ECDSA) to the list of known hosts.
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
```

Works!

### Create new branch called 'production'

In the GitLab webGUI, create a new branch called 'production'

- Navigate back to your **[puppet/control](http://127.0.0.1:24080/puppet/control)**
- Click the **+** icon (Plus sign) and select **New Branch**
- Enter **production** created from **master**

We now have 2 branches in our puppet/control repo

- master
- production

We will eventually delete the **master** branch, and use **production** as its replacement.
We dont want **'master'** as we do not have a master puppet environment.  We do have a
**production** puppet environment, so that's why we want a production **branch**

### Test that R10K can pull down your code

R10K knows to look for its config file in `/etc/puppetlabs/r10k/`

```
[root@puppet ~]# cd /etc/puppetlabs/r10k
[root@puppet r10k]# vi r10k.yaml
```

Create an `/etc/puppetlabs/r10k/r10k.yaml` containing the following:

```yaml
---
cachedir: '/var/cache/r10k'

sources:
  puppet-tutorial:
    remote:  'git@gitlab:puppet/control'
    basedir: '/tmp/r10k-test'
```

**WARNING**: Make sure that before you save this file that the **basedir**
is set to a temporary location, and not the default.  If you leave the **default**
as in the example `r10k.yaml.example`, you will blow away all of your existing
code.  Very very bad bad!  We don't want to do that yet.

```
[root@puppet ~]# cd /tmp
[root@puppet tmp]# r10k deploy environment -vp
INFO     -> Deploying environment /tmp/r10k-test/master
INFO     -> Environment master is now at 3e6627e116e24bbdf7e4d24b24d67d4aa586634e
INFO     -> Deploying environment /tmp/r10k-test/production
INFO     -> Environment production is now at 3e6627e116e24bbdf7e4d24b24d67d4aa586634e
```

Cool.  So we successfully tested that r10k runs using our test `r10k.yaml` file will work

```
[root@puppet tmp]# tree r10k-test
r10k-test
├── master
│   └── README.md
└── production
    └── README.md

```

Notice that R10K created an environment for each branch found in the Git repo.  We see
the original **master** branch (the initial branch that a new Git repo always comes with)
and the **production** branch, which we created in the GitLab WebGUI.  Both get their
own environment directory.

### Let's Move Our Existing Puppet Code In To GitLab

How do we get our existing puppet code in to GitLab?

We've given the root user the ability to clone/pull code, but it can not
push code (we're using a Deploy Key, which gives read-only access)

Should we temporarily give root access to push to the repo?  That's one option,
but since we've already cloned the repo to our workstation, and tested pushing
back and know it works, let's simply copy the files we need from the puppet
master to our local clone of the repo.

Let's take advantage of the `/share` mount, which is shared between our VM and our host.
If we copy the needed files there (from within the VM) we will be able to copy them
into place from the host side, and then commit to our Git repository, and push up to GitLab.

1. On your **puppet** VM,  identify the files we need (within `/etc/puppetlabs/code/environments`)
2. Copy those files over to `/share` (still within the VM)
3. Then in another terminal window, on the host side, copy from your `puppet-tutorial-pe/share` directory...
4. ...to the location of your clone of the **puppet/control** repo

So, in my case, this 2-stage copy will look like this:

**Stage 1**:  On my puppet master VM, Copy each environment directory...
```
   From: /etc/puppetlabs/code/environments
   To:   /share
```
**Stage 2**: On my host workstation, Copy each environment directory...
```
   From: /Users/bentlema/Documents/Git/GitHub/bentlema/puppet-tutorial-pe/share
   To:   /Users/bentlema/gitlab/puppet/control
```

Note that when we get to the **Stage 2** copy, we eliminate the actual
environment directory name, and instead ensure that the proper **Git Branch**
is **checked out** when we copy.  We will be copying the **production/** directory
over to the **production branch** within our Git repo, and likewise for the
**development/** directory and branch.

So within the **puppet** VM I'm going to start by copying the **production**
environment over to `/share`

```
[root@puppet ~]# cd /etc/puppetlabs/code/environments
[root@puppet environments]# ls -al
total 4
drwxr-xr-x 4 pe-puppet pe-puppet   41 Oct 21 11:52 .
drwxr-xr-x 7 root      root      4096 Oct 21 14:50 ..
drwxr-xr-x 5 root      root        47 Oct 24 13:06 development
drwxr-xr-x 5 root      root        47 Oct 21 14:49 production
[root@puppet environments]# cp -r production /share
```

Now on the host side (outside of the VM) copy those files using **rsync** to your repo like this:

```
# Check where we are at (our current working dir)
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (master)$ pwd
/Users/bentlema/gitlab/puppet/control

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (master)$ ls -al
total 8
drwxr-xr-x   4 bentlema  staff  136 Nov 16 12:27 .
drwxr-xr-x   3 bentlema  staff  102 Nov 16 12:23 ..
drwxr-xr-x  13 bentlema  staff  442 Nov 16 13:34 .git
-rw-r--r--   1 bentlema  staff   12 Nov 16 12:27 README.md

# Take a peek at the dir we will be copying from
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (master)$ ls -al /Users/bentlema/Documents/Git/GitHub/bentlema/puppet-tutorial-pe/share
total 32
drwxr-xr-x   6 bentlema  staff   204 Nov 16 13:34 .
drwxr-xr-x  15 bentlema  staff   510 Nov 16 07:56 ..
-rw-r--r--@  1 bentlema  staff  8196 Nov 11 14:07 .DS_Store
-rw-r--r--   1 bentlema  staff   137 Nov 11 10:50 README.md
drwxr-xr-x   6 bentlema  staff   204 Nov 16 13:34 production
drwxr-xr-x   8 bentlema  staff   272 Nov 11 13:01 software

# Okay, we see the production directory we just copied over within our VM
# Let's checkout the production branch, and copy the production code over to it

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (master)$ git checkout production
error: pathspec 'production' did not match any file(s) known to git.

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (master)$ git pull
From ssh://localhost/puppet/control
 * [new branch]      production -> origin/production
Already up-to-date.

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (master)$ git checkout production
Branch production set up to track remote branch production from origin.
Switched to a new branch 'production'

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)$ rsync -acv /Users/bentlema/Documents/Git/GitHub/bentlema/puppet-tutorial-pe/share/production/* .
building file list ... done
environment.conf
hieradata/
hieradata/common.yaml
hieradata/location/
hieradata/location/amsterdam.yaml
hieradata/location/seattle.yaml
hieradata/location/woodinville.yaml
hieradata/node/
hieradata/node/agent.example.com.yaml
hieradata/node/gitlab.example.com.yaml
hieradata/node/puppet.example.com.yaml
hieradata/role/
manifests/
manifests/common_hosts.pp
manifests/common_packages.pp
manifests/site.pp
[snip]
sent 1179660 bytes  received 15020 bytes  2389360.00 bytes/sec
total size is 1122664  speedup is 0.94


mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)*$ ls -al
total 16
drwxr-xr-x   8 bentlema  staff  272 Nov 16 13:42 .
drwxr-xr-x   3 bentlema  staff  102 Nov 16 12:23 ..
drwxr-xr-x  15 bentlema  staff  510 Nov 16 13:43 .git
-rw-r--r--   1 bentlema  staff   12 Nov 16 12:27 README.md
-rw-r--r--   1 bentlema  staff  879 Nov 16 13:34 environment.conf
drwxr-xr-x   6 bentlema  staff  204 Nov 16 13:34 hieradata
drwxr-xr-x   5 bentlema  staff  170 Nov 16 13:34 manifests
drwxr-xr-x   7 bentlema  staff  238 Nov 16 13:34 modules

```

Okay, we just copied over the entire production environment (Hiera data, manifests and modules) to our clone of the control repo.

Next we need to commit it to our Git repo...

```
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)*$ git add environment.conf
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)*$ git add hieradata
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)*$ git add manifests
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)*$ git add modules

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)*$ git commit -m 'initial add of production env'
[production 1ead2c1] initial add of production env
 654 files changed, 31627 insertions(+)
 create mode 100644 environment.conf
 create mode 100644 hieradata/common.yaml
 create mode 100644 hieradata/location/amsterdam.yaml
 create mode 100644 hieradata/location/seattle.yaml
 create mode 100644 hieradata/location/woodinville.yaml
 create mode 100644 hieradata/node/agent.example.com.yaml
 create mode 100644 hieradata/node/gitlab.example.com.yaml
 create mode 100644 hieradata/node/puppet.example.com.yaml
 create mode 100644 manifests/common_hosts.pp
 create mode 100644 manifests/common_packages.pp
 create mode 100644 manifests/site.pp
[snip]

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)$ git push
Counting objects: 741, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (704/704), done.
Writing objects: 100% (741/741), 346.73 KiB | 0 bytes/s, done.
Total 741 (delta 186), reused 0 (delta 0)
remote:
remote: Create merge request for production:
remote:   http://gitlab.example.com/puppet/control/merge_requests/new?merge_request%5Bsource_branch%5D=production
remote:
To ssh://localhost/puppet/control.git
   3e6627e..1ead2c1  production -> production

```

Now all of our production environment is hosted on our GitLab server.

We've also used some Git commands for the first time

- git checkout
- git pull
- git add
- git commit
- git push

A summary of what we've just done would go like this:

1.  We copied over our entire production codebase to `/share` within our VM
2.  Changed directory into our clone of the **puppet/control** Git repository on the host side
3.  Rsync'ed our entire production codebase **from** the `share/` directory **to** our **puppet/control** repo
4.  Pulled down the latest code from GitLab (with **git pull**) since we created our **production** branch via GitLab's WebGUI
5.  Checked out (switched to) the production branch (with **git checkout production**)
6.  Selected our `hieradata/`, `manifests/`, and `modules/` directories to be staged for a commit to our control repo
7.  Commited the staged changes to our control repo (with **git commit**)
8.  Pushed our local changes to the remote repository hosted within GitLab (with **git push**)

We will look at all of these commands in more depth in the next lab.

Now, Let's do the same thing for the development environment, so it doesn't get left behind...

**But**, let's make one small change:  **Do NOT copy over the `modules/` directory.  We will show how we can use R10K to pull the modules down for us.



```
[root@puppet environments]# mkdir /share/development
[root@puppet environments]# cd /share/development
[root@puppet development]# rsync -acv /etc/puppetlabs/code/environments/development/hieradata .
[root@puppet development]# rsync -acv /etc/puppetlabs/code/environments/development/manifests .
```

We've just copied over our Hiera data and manifests from the **development** environment directory on the puppet master to our temporary `/share` area.

Next, back on the host (outside the VM) we will rsync the files over to our **puppet/control** repo

```
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)$ git branch -a
  master
* production
  remotes/origin/master
  remotes/origin/production
```

Notice that we do not yet have a **development** branch in our **puppet/control**
repo.  Rather than creating a new branch via the GitLab WebGUI, let's create it
using the **-b** option to **git checkout**

```
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)$ git checkout -b development
Switched to a new branch 'development'
```

Okay, now copy the `hieradata/` and `manifests/` directories over...

```
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)$ rsync -acv /Users/bentlema/Documents/Git/GitHub/bentlema/puppet-tutorial-pe/share/development/* .
building file list ... done
hieradata/
hieradata/common.yaml
hieradata/location/
hieradata/node/
hieradata/node/agent.example.com.yaml
hieradata/role/
manifests/

sent 950 bytes  received 136 bytes  2172.00 bytes/sec
total size is 2650  speedup is 2.44

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)*$ ls -al hieradata
total 8
drwxr-xr-x  6 bentlema  staff  204 Nov 15 14:57 .
drwxr-xr-x  8 bentlema  staff  272 Nov 16 13:42 ..
-rw-r--r--  1 bentlema  staff  153 Nov 15 14:57 common.yaml
drwxr-xr-x  5 bentlema  staff  170 Nov 15 14:57 location
drwxr-xr-x  5 bentlema  staff  170 Nov 15 15:12 node
drwxr-xr-x  2 bentlema  staff   68 Nov 15 14:57 role

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)*$ ls -al manifests
total 24
drwxr-xr-x  5 bentlema  staff   170 Nov 15 14:57 .
drwxr-xr-x  8 bentlema  staff   272 Nov 16 13:42 ..
-rw-r--r--  1 bentlema  staff   446 Nov 15 14:57 common_hosts.pp
-rw-r--r--  1 bentlema  staff   189 Nov 15 14:57 common_packages.pp
-rw-r--r--  1 bentlema  staff  1326 Nov 15 14:57 site.pp
```

Next, let's add those files to our commit.

```
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)*$ git add hieradata
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)*$ git add manifests
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)*$ git status
On branch development
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   hieradata/common.yaml
    modified:   hieradata/node/agent.example.com.yaml
```

Remember, only files that are different from the **production** branch will be added.  Since we branched off of the **production** branch, we already have all of the files that were in the production branch, and now we are just committing the differences between **development** and **production**

```
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)*$ git commit -a -m 'initial commit'
[development 6852c44] initial commit
 2 files changed, 1 insertion(+), 1 deletion(-)
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)$ git push
fatal: The current branch development has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin development

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)$     git push --set-upstream origin development
Counting objects: 6, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 615 bytes | 0 bytes/s, done.
Total 6 (delta 2), reused 0 (delta 0)
remote:
remote: Create merge request for development:
remote:   http://gitlab.example.com/puppet/control/merge_requests/new?merge_request%5Bsource_branch%5D=development
remote:
To ssh://localhost/puppet/control.git
 * [new branch]      development -> development
Branch development set up to track remote branch development from origin.
```

Boom!  Our development code, less the modules, is now hosted in GitLab.

We now have most of our development environment setup.  We have not copied over the modules, as we will use R10K to do that next...

Let's do another R10K test run from /tmp to see what comes down ...

```
[root@puppet tmp]# cd /tmp
[root@puppet tmp]# r10k deploy environment -vp
INFO     -> Deploying environment /tmp/r10k-test/development
INFO     -> Environment development is now at 6852c44ba61c9d2a4cd10338fcd490544a859a70
INFO     -> Deploying environment /tmp/r10k-test/master
INFO     -> Environment master is now at 3e6627e116e24bbdf7e4d24b24d67d4aa586634e
INFO     -> Deploying environment /tmp/r10k-test/production
INFO     -> Environment production is now at 1ead2c1cf2e6c94484134912a66b9cde1b20db70
```

See how R10K pulled down our code and dropped it in /tmp/r10k-test as per our r10k.yaml ?

We need to do one more thing before we can swing it over to the final location of `/etc/puppetlabs/code/environments`

We need to get the **modules/** directory populated the same way as on the master currently

Look at what's in the production environment modules directory right now:

```
[root@puppet tmp]# cd /etc/puppetlabs/code/environments/production/
[root@puppet production]#  tree -L 1 modules
modules
├── motd
├── ntp
├── registry
├── stdlib
└── timezone

5 directories, 0 files
```

What versions of those modules are we running in the development environment?

```
[root@puppet production]# puppet module list --environment=development
/etc/puppetlabs/code/environments/development/modules
├── puppetlabs-motd (v1.4.0)
├── puppetlabs-ntp (v6.0.0)
├── puppetlabs-registry (v1.1.3)
├── puppetlabs-stdlib (v4.13.1)
└── saz-timezone (v3.3.0)
/etc/puppetlabs/code/modules
└── puppetlabs-stdlib (v4.12.0)
```

There's 5 modules in there.  Before we tell R10K to pull code in to `/etc/puppetlabs/code/environments` we need to make sure the modules are pulled down, otherwise our puppet runs will break.

There's another feature of R10K that allows us to specify other Git repositories to pull modules from.  To control this, we create a config file called a **Puppetfile**.

Back on your workstation in your **puppet/control** repo, create a **Puppetfile** at the top level

Put these lines in your Puppetfile (in the **development** branch)
(Change the versions to match what you have on your VM)

```
moduledir 'modules'
mod 'puppetlabs/motd',     '1.4.0'
mod 'puppetlabs/ntp',      '6.0.0'
mod 'puppetlabs/registry', '1.1.3'
mod 'puppetlabs/stdlib',   '4.13.1'
mod 'saz/timezone',        '3.3.0'
```

Save, add, commit, and push your new Puppetfile ...

```
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)$ vi Puppetfile
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)*$ git add Puppetfile
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)*$ git commit -a -m 'initial commit'
[development bcde4e5] initial commit
 1 file changed, 6 insertions(+)
 create mode 100644 Puppetfile
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)$ git push
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 388 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote:
remote: Create merge request for development:
remote:   http://gitlab.example.com/puppet/control/merge_requests/new?merge_request%5Bsource_branch%5D=development
remote:
To ssh://localhost/puppet/control.git
   6852c44..bcde4e5  development -> development
```

And re-run r10k from your puppet master...

```
[root@puppet production]# cd /tmp
[root@puppet tmp]# r10k deploy environment -vp
INFO     -> Deploying environment /tmp/r10k-test/development
INFO     -> Environment development is now at cb26335eb5c41c808979a5e13f51b41715ceb4f7
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/motd
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/ntp
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/registry
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/stdlib
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/timezone
INFO     -> Deploying environment /tmp/r10k-test/master
INFO     -> Environment master is now at 3e6627e116e24bbdf7e4d24b24d67d4aa586634e
INFO     -> Deploying environment /tmp/r10k-test/production
INFO     -> Environment production is now at 1ead2c1cf2e6c94484134912a66b9cde1b20db70

```

Notice how R10K pulled down all of the modules we specified in the **development** branch'es **Puppetfile**?  Pretty cool, eh?

Next let's copy our **development** `Puppetfile` in to the **production** branch, so that our **production** environment gets the modules as well.

```
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (development)$ git checkout production
Switched to branch 'production'
Your branch is up-to-date with 'origin/production'.
```

We switch to the **production** branch, getting ready to create/edit the Puppetfile in that branch...

```
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)$ git diff --stat development
 Puppetfile                            | 6 ------
 hieradata/common.yaml                 | 1 +
 hieradata/node/agent.example.com.yaml | 1 -
 3 files changed, 1 insertion(+), 7 deletions(-)
```

Notice that the development branch has 3 differeing files, one of which is the Puppetfile.
Remember, we've created a Puppetfile in the **development** branch, but not the **production** branch.
Let's simply checkout the development branch'es Puppetfile in to the **production** branch (which is our current branch) and edit it...

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Training/control] (production)$ git checkout development Puppetfile
```

Add it, commit and push it...

```
mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)*$ git add Puppetfile

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)*$ git commit -a -m 'initial commit'
[production 2230b9e] initial commit
 1 file changed, 6 insertions(+)
 create mode 100644 Puppetfile

mbp-mark:[/Users/bentlema/gitlab/puppet/control] (production)$ git push
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 384 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote:
remote: Create merge request for production:
remote:   http://gitlab.example.com/puppet/control/merge_requests/new?merge_request%5Bsource_branch%5D=production
remote:
To ssh://localhost/puppet/control.git
   1ead2c1..2230b9e  production -> production
```

Now let's run R10K again, and see if we get everything...

```
[root@puppet tmp]# r10k deploy environment -vp
INFO     -> Deploying environment /tmp/r10k-test/development
INFO     -> Environment development is now at cb26335eb5c41c808979a5e13f51b41715ceb4f7
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/motd
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/ntp
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/registry
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/stdlib
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/timezone
INFO     -> Deploying environment /tmp/r10k-test/master
INFO     -> Environment master is now at 3e6627e116e24bbdf7e4d24b24d67d4aa586634e
INFO     -> Deploying environment /tmp/r10k-test/production
INFO     -> Environment production is now at 2230b9ef9a1ef461ac9c336663891485649fd36c
INFO     -> Deploying Puppetfile content /tmp/r10k-test/production/modules/motd
INFO     -> Deploying Puppetfile content /tmp/r10k-test/production/modules/ntp
INFO     -> Deploying Puppetfile content /tmp/r10k-test/production/modules/registry
INFO     -> Deploying Puppetfile content /tmp/r10k-test/production/modules/stdlib
INFO     -> Deploying Puppetfile content /tmp/r10k-test/production/modules/timezone
```

Looks good, all except for that **master** environment in there.  Since Git creates the **master** branch by default, and we dont need or want a **master** Puppet Environment, we should delete that branch from our Git repository.  If we dont, we'll end up with a **master** environment on our Puppet Master.  It wont hurt anything, but will be annoying.

Delete the **local master branch** with: `git branch -d master`

Delete the **remote master branch** with: `git push origin --delete master`

Try it now...

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Training/control] (production)$ git branch -a
  development
  master
* production
  remotes/origin/development
  remotes/origin/master
  remotes/origin/production
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Training/control] (production)$ git branch -d master
Deleted branch master (was 056f697).
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Training/control] (production)$ git push origin --delete master
remote: GitLab: You are not allowed to delete protected branches from this project.
To ssh://localhost/puppet/control.git
 ! [remote rejected] master (pre-receive hook declined)
error: failed to push some refs to 'ssh://localhost/puppet/control.git'
```

Okay, darn.  Normally that would work, but our **master** branch is **protected**.  Let's go into the GitLab WebGUI and figure out how to un-protect it.

- Go to the **Settings** gear icon, and select **Protected Branches**
- Add **production** as a protected branch
- Click **Unprotect** for the **master** branch
- Now let's re-try our **git push**

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Training/control] (production)$ git push origin --delete master
remote: error: By default, deleting the current branch is denied, because the next
remote: error: 'git clone' won't result in any file checked out, causing confusion.
remote: error:
remote: error: You can set 'receive.denyDeleteCurrent' configuration variable to
remote: error: 'warn' or 'ignore' in the remote repository to allow deleting the
remote: error: current branch, with or without a warning message.
remote: error:
remote: error: To squelch this message, you can set it to 'refuse'.
remote: error: refusing to delete the current branch: refs/heads/master
To ssh://localhost/puppet/control.git
 ! [remote rejected] master (deletion of the current branch prohibited)
error: failed to push some refs to 'ssh://localhost/puppet/control.git'
```

The **master** branch is still configured as the **default** branch within
GitLab, but we've deleted it. To fix this, go into the Project Settings, and
change the default branch to **production**

- Settings --> Edit Project --> Default Branch --> Select 'production'

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Training/control] (production)$ git push origin --delete master
To ssh://localhost/puppet/control.git
 - [deleted]         master
```

Okay, re-run R10K again...

```
[root@puppet tmp]# r10k deploy environment -vp
INFO     -> Deploying environment /tmp/r10k-test/development
INFO     -> Environment development is now at cb26335eb5c41c808979a5e13f51b41715ceb4f7
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/motd
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/ntp
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/registry
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/stdlib
INFO     -> Deploying Puppetfile content /tmp/r10k-test/development/modules/timezone
INFO     -> Deploying environment /tmp/r10k-test/production
INFO     -> Environment production is now at 2230b9ef9a1ef461ac9c336663891485649fd36c
INFO     -> Deploying Puppetfile content /tmp/r10k-test/production/modules/motd
INFO     -> Deploying Puppetfile content /tmp/r10k-test/production/modules/ntp
INFO     -> Deploying Puppetfile content /tmp/r10k-test/production/modules/registry
INFO     -> Deploying Puppetfile content /tmp/r10k-test/production/modules/stdlib
INFO     -> Deploying Puppetfile content /tmp/r10k-test/production/modules/timezone
INFO     -> Removing unmanaged path /tmp/r10k-test/master
```

Notice that the un-used **master** directory environment was removed?  That's what we want.

### Put R10K in control

Finally!

Update the **r10k.yaml** so that **basedir** points to the real PE environments dir like this:

```
cd /etc/puppetlabs/r10k
vi r10k.yaml
```

Update the **basedir** as follows...

```yaml
---
cachedir: '/var/cache/r10k'

sources:
  puppet-training:
    remote:  'git@gitlab:puppet/control'
    basedir: '/etc/puppetlabs/code/environments'
```

Now, when we run r10k, it will completely wipe anything in the basedir, and then pull everything in exactly like we were doing into our test dir... Let's go ahead and do that...

```
[root@puppet r10k]# r10k deploy environment -vp --verbose debug
[2016-11-16 14:22:25 - DEBUG] Fetching 'git@gitlab:puppet/control' to determine current branches.
[2016-11-16 14:22:25 - INFO] Deploying environment /etc/puppetlabs/code/environments/development
[2016-11-16 14:22:25 - DEBUG] Replacing /etc/puppetlabs/code/environments/development and checking out development
[2016-11-16 14:22:25 - INFO] Environment development is now at cb26335eb5c41c808979a5e13f51b41715ceb4f7
[2016-11-16 14:22:25 - INFO] Deploying Puppetfile content /etc/puppetlabs/code/environments/development/modules/motd
[2016-11-16 14:22:25 - INFO] Deploying Puppetfile content /etc/puppetlabs/code/environments/development/modules/ntp
[2016-11-16 14:22:25 - INFO] Deploying Puppetfile content /etc/puppetlabs/code/environments/development/modules/registry
[2016-11-16 14:22:25 - INFO] Deploying Puppetfile content /etc/puppetlabs/code/environments/development/modules/stdlib
[2016-11-16 14:22:25 - INFO] Deploying Puppetfile content /etc/puppetlabs/code/environments/development/modules/timezone
[2016-11-16 14:22:25 - DEBUG] Purging unmanaged Puppetfile content for environment 'development'...
[2016-11-16 14:22:25 - INFO] Deploying environment /etc/puppetlabs/code/environments/production
[2016-11-16 14:22:25 - DEBUG] Replacing /etc/puppetlabs/code/environments/production and checking out production
[2016-11-16 14:22:26 - INFO] Environment production is now at 2230b9ef9a1ef461ac9c336663891485649fd36c
[2016-11-16 14:22:26 - INFO] Deploying Puppetfile content /etc/puppetlabs/code/environments/production/modules/motd
[2016-11-16 14:22:26 - INFO] Deploying Puppetfile content /etc/puppetlabs/code/environments/production/modules/ntp
[2016-11-16 14:22:26 - INFO] Deploying Puppetfile content /etc/puppetlabs/code/environments/production/modules/registry
[2016-11-16 14:22:26 - INFO] Deploying Puppetfile content /etc/puppetlabs/code/environments/production/modules/stdlib
[2016-11-16 14:22:26 - INFO] Deploying Puppetfile content /etc/puppetlabs/code/environments/production/modules/timezone
[2016-11-16 14:22:26 - DEBUG] Purging unmanaged Puppetfile content for environment 'production'...
[2016-11-16 14:22:26 - DEBUG] Purging unmanaged environments for deployment...
```

Now do a test puppet run on both the puppet master and the gitlab VM, and you should still get a clean run...

If you also have the **agent** VM up and running, you should get a clean run on it as well.

### Cleanup

Remember that we used the `share/` directory as a temporary staging directory.  Let's clean that up so Git doesn't bug us about untracked files there...

```
mbp-mark:[/Users/bentlema/Documents/Git/GitHub/bentlema/puppet-tutorial-pe/share] (master)*$ pwd
/Users/bentlema/Documents/Git/GitHub/bentlema/puppet-tutorial-pe/share
mbp-mark:[/Users/bentlema/Documents/Git/GitHub/bentlema/puppet-tutorial-pe/share] (master)*$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    development/
    production/

no changes added to commit (use "git add" and/or "git commit -a")


mbp-mark:[/Users/bentlema/Documents/Git/GitHub/bentlema/puppet-tutorial-pe/share] (master)*$ rm -rf development production
mbp-mark:[/Users/bentlema/Documents/Git/GitHub/bentlema/puppet-tutorial-pe/share] (master)$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

### R10K Notes

So we've configured R10K, but it's not going to magically run itself.

We previously talked about configuring the **postrun_command** in the **puppet.conf** to run our 
`r10k deploy environment` command every 30 minutes, and we could certainly do that, but something
more interesting would be to setup GitLab to trigger R10K automatically every time someone 
pushes to the control repo.

Let's look at how we can setup a post-receive hook...

### Git post-receive hook

Let's setup a **post-receive hook** which will ssh from GitLab to the puppet master and run r10k for us.
This way, every time we do a **git push** to GitLab, it will automatically run R10K on the master for us.

There's more than one way to do this, but a few that come to mind are:

1. setup the *git* account as an MCollective client, and use the r10k module to enable the 'mco r10k sync' command
2. setup ssh keys to allow the git user to run commands on the puppet master password-less
3. configure a webhook within GitLab to trigger an R10K run

In a production environment, you'd likely want to setup a [webhook](http://127.0.0.1:24080/help/web_hooks/web_hooks), but...

To keep things simple, let's just setup SSH keys.  Make sure you **trust** the GitLab server, as we will
be giving it the ability to ssh in as root on our puppet master! (Take care to understand the security
implications here.)

On the GitLab VM...

Become the **git** user this time (as we don't need to be root)

```
sudo su - git
```

```
[git@gitlab ~]# cd /var/opt/gitlab/git-data/repositories/puppet/control.git
[git@gitlab control.git]# mkdir custom_hooks
[git@gitlab control.git]# cd custom_hooks/
```

Make a bash script called **post-receive** ...

```
vi post-receive
```

...and copy-and-paste in this content:

```
#!/bin/bash

# List of Puppet Masters to update
pe_masters="
puppet"

PATH="/opt/gitlab/bin:/opt/gitlab/embedded/bin:/opt/gitlab/embedded/libexec/git-core:/opt/puppetlabs/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin"
export PATH

echo
echo "Running post-receive hook..."
for pm in $pe_masters ; do
  ssh -l root $pm "echo \"[$pm] Updating...\" ; /usr/local/bin/r10k deploy environment -p ; echo \"[$pm] Done.\""
done
echo

```

Make sure to give read/execute perms on the post-receive script

```
[git@gitlab custom_hooks]# chmod a+rx post-receive
```

All that does is iterate through a list of puppet masters, and ssh as root to each one and run r10k.
(We only have one puppet master in our training environment, but most production environments would have 2 or more for load-balancing.)

Make sure you are the git user (not root), and create an ssh key pair, and then copy the public key to the root user's ~/.ssh/authorized_keys on the puppet master...

```
-sh-4.2$ cd     # get back to git's home dir

-sh-4.2$ pwd
/var/opt/gitlab

-sh-4.2$ ls -al .ssh
total 8
drwx------  2 git  git    55 Mar  9 13:35 .
drwxr-xr-x 13 root root 4096 Mar  9 10:59 ..
-rw-------  1 git  git  1087 Mar  9 14:58 authorized_keys
-rw-r--r--  1 git  git     0 Mar  9 14:58 authorized_keys.lock

-sh-4.2$ ssh-keygen -t rsa -b 2048 -N ''
Generating public/private rsa key pair.
Enter file in which to save the key (/var/opt/gitlab/.ssh/id_rsa):
Your identification has been saved in /var/opt/gitlab/.ssh/id_rsa.
Your public key has been saved in /var/opt/gitlab/.ssh/id_rsa.pub.
The key fingerprint is:
6a:05:5c:80:75:80:c9:c9:df:44:98:a7:c6:be:ab:5b git@gitlab
The key's randomart image is:
+--[ RSA 2048]----+
|   o *+*+        |
|    B.ooo        |
|     oo=         |
|      =..        |
|     o  S        |
|      .o         |
|      E.         |
|     o.          |
|    oo..         |
+-----------------+

-sh-4.2$ ls -al .ssh
total 16
drwx------  2 git  git    85 Mar 16 12:55 .
drwxr-xr-x 13 root root 4096 Mar  9 10:59 ..
-rw-------  1 git  git  1087 Mar  9 14:58 authorized_keys
-rw-r--r--  1 git  git     0 Mar  9 14:58 authorized_keys.lock
-rw-------  1 git  git  1679 Mar 16 12:55 id_rsa
-rw-r--r--  1 git  git   392 Mar 16 12:55 id_rsa.pub

-sh-4.2$ cat .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDQqKmxWCjcBllO+BnZLVRd+rhzXlm/6S5ccspvbeBEH/zST5DhKNGLwtJn0yz8u1cWyYztkyjZIPwuJzBbap3vU/Lx6juVaoAUK8AnDIeCY+nZFN6oZaSfpBEJunno1FPlVVja1sCoYSMqmsnCY/kcLawq3ui9zdx25NFWc7hG9jOqUcmIdJgGFcy5/GsCgJtKvS/UkJ22xaxKWKJMHT0/KHb+0mw/RClhqWsJD9PFI0+Psnh/D2XFuG7eoZooenSFV3bVQoWe5AgwNIX5/B0/0xlUWcPjTyWfa7MhffHTCmTzUauEytkqScfH3ArtBNL6vRd8uCPi7pTrRFwo9jWl git@gitlab
```

Add that public key to root's ~root/.ssh/authorized_keys file on the master using the ssh-copy-id command:

```
-sh-4.2$ ssh-copy-id -i .ssh/id_rsa.pub root@puppet
The authenticity of host 'puppet (192.168.198.10)' can't be established.
ECDSA key fingerprint is 1e:67:49:63:1f:80:8b:a4:19:16:1e:f8:b1:28:82:8d.
Are you sure you want to continue connecting (yes/no)? yes
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@puppet's password: vagrant

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@puppet'"
and check to make sure that only the key(s) you wanted were added.
```

Now test sshing from the git@gitlab account to root@puppet ...

```
-sh-4.2$ whoami
git

-sh-4.2$ ssh -l root puppet
The authenticity of host 'puppet (192.168.198.10)' can't be established.
ECDSA key fingerprint is 39:e5:9b:0d:8b:bd:74:0a:12:e8:c6:37:cb:cf:17:c3.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'puppet,192.168.198.10' (ECDSA) to the list of known hosts.
Last login: Wed Mar 16 12:27:24 2016 from 192.168.198.11
[root@puppet ~]#
[root@puppet ~]# exit
logout
Connection to puppet closed.
```

Now we should be able to ssh without being prompted at all...

```
-sh-4.2$ ssh -l root puppet 'hostname;id'
puppet
uid=0(root) gid=0(root) groups=0(root)
```

Now our post-receive hook will ssh to the master and run r10k automatically...

The next time you make a change in your control repo, commit, and push, you should see something like this:

```
[/Users/Mark/Git/Puppet-Training/control/data/node] (production)$ git push
Counting objects: 1, done.
Writing objects: 100% (1/1), 188 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
remote:
remote: Running post-receive hook...
remote: [puppet] Updating...
remote: [puppet] Done.
remote:
To ssh://localhost/puppet/control.git
   f193b1d..625fe7e  production -> production
```

Those lines beginning with **"remote:"** are the actual output from your post-receive hook.  

There are several [other Git hooks](https://docs.gitlab.com/ce/administration/custom_hooks.html) you could configure as well, including pre-commit, post-commit, update, etc.

### Summary

So what have we done?

1. Created SSH key for the root user
  - Puppet runs as root, so this key is used by puppet/R10K to pull from GitLab
2. Installed SSH key as **Deploy Key** in GitLab for puppet/control
  - This allows the root user to clone/pull that repo
3. Created a new branch called **production** in our control repo
4. Moved our existing Puppet code and Hiera data in to GitLab
5. Configured R10K to pull our Puppet code and Hiera data down to the Puppet Master
  - This includes configuring the **Puppetfile** to download the modules we use
6. Configured a **post-receive** hook on the GitLab server
  - This will trigger an R10K run on the puppet master when we do a **git push**

### REMINDER - No more editing Puppet code directly on the Puppet Master

From this point going forward we will **NOT** edit any code files directly on the Puppet Master.
Instead, we will edit our code and Hiera data on our workstation in our **puppet/control** repo.

In the next section we will look at Git in more detail, but the basic process going forward will be:

1.  Edit file(s) in your local clone of the puppet/control repo
2.  Add/Commit new or changed file(s)
3.  Push to the remote puppet/control repo hosted in/on GitLab

That's it!

---

Continue on to **Lab #11** --> [Roles & Profiles](11-Roles-and-Profiles.md#lab-11)

---

<-- [Back to Contents](/README.md)

---

<-- [Back](10-Move-Puppet-Code-to-GitLab.md#lab-10)

---

### **Lab #11** - Roles and Profiles

---

### Overview

So where are we at now?

1.  We've moved our Puppet code and Hiera data into Git
2.  We've configured R10K to pull our Puppet code and Hiera data over to the Puppet Master
3.  We've configured a post-receive hook to trigger an R10K run automatically when we `git push`
3.  We've also configured a Puppetfile within our control repo to pull in some external Puppet Modules

Where do we want to go?

1.  We want to learn more about **using Git**
2.  We want to learn more about the **Roles & Profiles** pattern
3.  We want to do some more Puppet coding to get practice
4.  We want to learn how to **test our puppet code** prior to pushing to production (develop a Workflow)

We will see some Git usage as we move through this lab, and will cover the basics in the following lab.
Let's start by looking at the **"Roles & Profiles Pattern"**...

### Code Organization

We've already seen that there are multiple ways to classify a node:

1. node definition in the `site.pp`
2. using an ENC such as the PE Console
3. using Hiera to assign classes to a node

We have 3 different ways to choose from, and we can even mix and match if we wanted to.

We can create a big complex mess if we want to!

The same goes for writing Puppet code, and how to organize it.

We can...

1. Store individual manifests at the top level along with the site.pp (not recommended, especially in 3.8+)
2. We could create complete modules with site-specific code (has a learning curve)
3. We could create a sub-directory structure beneith the manifests directory, and sprinkle code there
4. We could create a couple special-purpose modules called **role** and **profile** to hold our code in a specific way

We're going to look at the 4th option:  Roles and Profiles

### Roles & Profiles

There's already been *a lot* written on the Roles & Profiles Pattern...

See:  [Further Reading](../YY-Further-Reading.md)

Roles and Profiles are just puppet manifests containing a set of class definitions
and some code.  There's nothing special about them from Puppet's point of
view.  Puppet doesn't have any knowledge about something called a "Role" or
something called a "Profile".  We, as the **Puppet Code Maintainer**, have simply
chosen to use those names, and it is ***us*** that enforces their use in a specific
way..  To Puppet, a role class or a profile class is just another class.  You could
just as easily replace "Role" with "Foo" and "Profile" with "Bar", and you could
acomplish the same goal of keeping your code organized in a certain way.

The idea of a Role:

  - Each node is assigned exactly **one** role
  - That single role class would be assigned to the node using any node classification method
    - We will use Hiera to assign the role class to a node
  - The role class
    - simply contains a list of **include profile::<class>** statements
    - traditionally does **NOT** contain any conditional logic

The idea of a Profile:

  - contains code snipits that contain resource types to do real work
  - ideally the profile class would lookup any needed data in Hiera via auto-parameter lookup
  - one or more profiles would be bundled together to form a role
  - individual profile classes could be used across multiple roles if written to support that

For example, if you have an Application called 'FooApp' it might have a role called:

  - role::foo_app

And that role would be assigned to a particular node in order to make it a 'FooApp'.  The role::foo_app class might look like this:

```puppet
class role::foo_app {
  include profile::foo_app::users
  include profile::foo_app::nfs_share
  include profile::webserver::apache
  include profile::database::mysql
}
```

These other profile classes we are including are parts needed to make a
FooApp server, so when we assign **role::foo_app** to a node, it will get the
included profile classes for an Apache web server, a MySQL DB, and an NFS
share (probably mounts up a shared filesystem used by Apache and/or MySQL)

Notice that there are some application-specific classes (users, and nfs_share)
that are located within a `foo_app/` directory, while there are also some
general-purpose classes for an Apache web server and MySQL server, that many
other roles might share.  Hiera data would be used to configure these for
the specific application, so that the same profile code could be used by
multiple roles/applications.

As you can see, we need to think a bit about how we organize our profile code,
as we want to reduce code duplication if possible (maximize code reuse), but
at the same time we want to maintain simplicity and readability of code.

That is key, so let's restate it:

We want to maintain code **simplicity** and **readability** while
**reducing code duplication** and **maximizing code reuse** where possible.

### Where to put our Roles & Profiles?

We could treat our **role** and **profile** code directories like modules,
and put them in their own Git repos, and then use R10K to pull them in
just like it does for any other module.  The code within them would be
within the modulepath, so could be found no problem.

You'd end up with lines in your **Puppetfile** like this:

```
mod 'role',    :git => 'git@gitlab:puppet-role',    :ref => '857310c'
mod 'profile', :git => 'git@gitlab:puppet-profile', :ref => '026975a'
```
...or maybe specifying your feature branch instead of a hash ref:

```
mod 'role',    :git => 'git@gitlab:puppet-role',    :branch => 'my_feature'
mod 'profile', :git => 'git@gitlab:puppet-profile', :branch => 'my_feature'
```

We could point to a specific hash ref, tagged version, or even a specific branch
within our in-house **role** and **profile** modules.

Remember, the **Puppetfile** in your control repo controls what modules are pulled in
to your environment.  The **Puppetfile** would control both third-party modules
(from the Puppetlabs Forge, or from 3rd-party developer's Git repositires) as well
as your own **in-house-developed "site" modules**.  You'd likely be pinning those
modules to a specific version or Git hash reference to guarantee production code is
static. (You wouldn't want some external module update to automatically trickle
in to your production environment until you've tested it.)

With all of this said, there is a problem with doing this for your in-house
modules, especially if you're using **long-lived branches** in your workflow
(such as a **production**, **staging**, and **development** branch), and
especially if you make frequent changes.  You'd end up with differing
Puppetfile's in each branch pointing at the appropriate code.  The production
branch Puppetfile would point to the production branch of each module, while
the staging branch Puppetfile would point to the staging branch of each module,
etc.  The annoying part becomes evident the first time you want to merge from
development --> staging --> production and you realize the Puppetfiles are
different, and you want to keep them different.  Whether you use hash ref,
tagged version, or branch in your Puppetfile, you run into the same issue.

The way to get around the issue is to simply **NOT** use long-lived branches
in your workflow, and just branch off of production with feature branches,
then merge the feature branch in to production after testing, and delete afterwards.
In theory, you would have pulled the most recent version of the Puppetfile
when you created your feature branch, and hopefully you didn't take too long
to develop your feature.  When testing your code changes, you would have updated
the Puppetfile in your feature branch with the commit hash you're testing
against, and once you're ready to merge in to production, you could merge
everything, including the Puppetfile.

There's still the potential for a problem.  What if you took several days to
develop your new feature, and another person was also developing their own
feature, and they updated the Puppetfile in their feature branch?  You'll
eventually have to deal with getting the Puppetfile right.  It's just annoying,
isn't it?  Ultimately the problem comes down to a couple things:

1.  The Puppetfile lives in your control repo
2.  The **role** and **profile** modules are modified frequently by many users
3.  Every time the **role** or **profile** modules are updated, we need to update the Puppetfile

Every time the role and/or profile module is updated, the Puppetfile also has
to be updated, and R10K run to update the puppet environments.  It's likely if
you're working on a medium or large team, you have many people making changes
to the role and profiles modules on a daily basis (they are your glue code).
In order to test code in another puppet environment (your feature branch) the
Puppetfile would also have to be updated (in your feature branch), and then
when you go to merge your changes into production, you have to intentionally
omit your modified Puppetfile (if specifying branch names).  If specifying
a hash ref, you may be able to get away with merging in your Puppetfile, and
hope you dont conflict with another user (highly likely with a medium/large
team, and many people making changes/additions to the role and profile modules
at the same time.)

There is a simple solution to avoid all of this, and more and more folks
are doing it the following way...

### Keep it simple...

We really have no good reason to keep our **role** and **profile** modules
in their own Git repos, so let's take a different approach, and add another
path to the module search path.

We will just **keep our in-house-developed site modules in the control repo.**
(This would include the **role** and **profile** modules.) This way every change
to our own modules will be a change in the control repo, and our R10K
post-receive hook will be triggered too, so extra bonus.  And, the **BIG**
win here is that our in-house-developed modules would **NOT** need to be in
the Puppetfile.  R10K would still be used to pull over our control repo code, but
we wouldn't have to deal with the Puppetfile anymore (for in-house modules).

Our strategy to implement the Roles & Profiles Pattern:

1.  Create new `site/` directory within our control repo which will house
    role and profile modules
2.  Create an `environment.conf` that inserts **"site"** at the head of the
    module search path
3.  Create a **"base"** profile, and move the `common_hosts.pp` and `common_packages.pp`
    classes out of `manifests/` dir

Why do we create a new `site/` directory? (By the way, you can call it
whatever you like.)

Because we've configured R10K to populate the `modules/` directory on the
puppet master, if we create a `modules/` directory in our control repo,
anything in it will get pulled down to the master, but then when R10K pulls
down the modules as configured in the Puppetfile, it will blow away anything
in that directory, and plop the modules down in to it.  To avoid that, we will
create another directory for modules called **"site"** because these will be
our own site's modules (vs 3rd-party modules).   Another option for such a
directory might be **"local"** for "my local code" but I'm not going to lose
any sleep over naming and continue on...

So, change dir to your **puppet/control** repo, and...
(Note: the path to your control repo will be different than shown here.)

```
$ cd ~/puppet/control    # location of where you cloned your puppet control repo

(production)$ mkdir -p site/role/manifests

(production)$ mkdir -p site/profile/manifests

(production)$ cp manifests/common_*.pp site/profile/manifests

(production)*$ find site
site
site/profile
site/profile/manifests
site/profile/manifests/common_hosts.pp
site/profile/manifests/common_packages.pp
site/role
site/role/manifests
```

What have we done?

* We've created our `site/` directory containing a `role/` and `profile/` directory.
* We've created the `manifests/` directory within each as well (puppet looks for puppet code within a module here)
* We've copied our two common manifests into the profile module manifests directory (this will be there new home)

Next, let's edit those two manifests in their new location to have the correct
class names:

```puppet
class profile::common_hosts {
```

...and...

```puppet
class profile::common_packages {
```

**IMPORTANT**: The class name must match the underlying directory structure.

So, within your `site/profile/manifests/` directory, edit each of the manifest files
to have the correct class name, then  add/commit/push, and then run r10k on
the master...

```
(production)*$ cd site/profile/manifests/

(production)*$ vi common_hosts.pp

(production)*$ vi common_packages.pp

(production)*$ git add *.pp

(production)*$ git status
On branch production
Your branch is up-to-date with 'origin/production'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   common_hosts.pp
    new file:   common_packages.pp

(production)*$ git commit -m 'update class name'
[production d91192d] update class name
 2 files changed, 24 insertions(+)
 create mode 100644 site/profile/manifests/common_hosts.pp
 create mode 100644 site/profile/manifests/common_packages.pp

(production)$ git push
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (7/7), 838 bytes | 0 bytes/s, done.
Total 7 (delta 1), reused 0 (delta 0)
remote:
remote: Running post-receive hook...
remote: [puppet] Updating...
remote: [puppet] Done.
remote:
To ssh://localhost/puppet/control.git
   1b8679a..936baf3  production -> production

```

Notice that your post-receive hook ran, so it would have run R10K automatically for you.
You shouldn't have to run R10K manually on the puppet master any longer, but if for some
reason you didn't complete the post-receive hook config in the previous lab, you can
still run it manually on the master like always:

```
[root@puppet ~]# r10k deploy environment -vp
INFO     -> Deploying environment /etc/puppetlabs/puppet/environments/development
INFO     -> Deploying module /etc/puppetlabs/puppet/environments/development/modules/motd
INFO     -> Deploying module /etc/puppetlabs/puppet/environments/development/modules/ntp
INFO     -> Deploying module /etc/puppetlabs/puppet/environments/development/modules/registry
INFO     -> Deploying module /etc/puppetlabs/puppet/environments/development/modules/stdlib
INFO     -> Deploying module /etc/puppetlabs/puppet/environments/development/modules/timezone
INFO     -> Deploying environment /etc/puppetlabs/puppet/environments/production
INFO     -> Deploying module /etc/puppetlabs/puppet/environments/production/modules/motd
INFO     -> Deploying module /etc/puppetlabs/puppet/environments/production/modules/ntp
INFO     -> Deploying module /etc/puppetlabs/puppet/environments/production/modules/registry
INFO     -> Deploying module /etc/puppetlabs/puppet/environments/production/modules/stdlib
INFO     -> Deploying module /etc/puppetlabs/puppet/environments/production/modules/timezone
```

But wait, how do we let Puppet know to look in this new **"site"** directory for
puppet modules/classes?  We have to create an **environment.conf** and modify
the modulepath to include this additional directory.

So, back to your control repo...and let's add it...

Get back to the top level of your control repo:

```
(production)$ cd ../../..
```

Then create the new file `environment.conf` (if it doesn't already exist) and add a line to set the **modulepath**

```
(production)$ echo 'modulepath = site:modules:$basemodulepath' >> environment.conf

(production)*$ git add environment.conf

(production)*$ git commit -m 'create environment.conf to modify modulepath'
[production c0aeb1a] create environment.conf to modify modulepath
 1 file changed, 1 insertion(+)
 create mode 100644 environment.conf

(production)$ git push
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 310 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote:
remote: Running post-receive hook...
remote: [puppet] Updating...
remote: [puppet] Done.
remote:
To ssh://localhost/puppet/control.git
   936baf3..9d64f56  production -> production

```

Now the real test is **can we update our common.yaml** in Hiera data to use the new **profile::common_hosts** and **profile::common_packages** ?  Let's try and see if it works...

Edit your `common.yaml` to look like this:
```
---

classes:
   - profile::common_hosts
   - profile::common_packages
   - motd

ntp::servers:
  - '0.pool.ntp.org'
  - '1.pool.ntp.org'
  - '2.pool.ntp.org'
  - '3.pool.ntp.org'
```

Commit/push that, and run r10k on the master...

```
(production)$ cd hieradata

(production)$ vi common.yaml

(production)*$ git commit -a -m 'point to new location for common_hosts and common_packages'
[production 9af5aa4] point to new location for common_hosts and common_packages
 1 file changed, 2 insertions(+), 2 deletions(-)

(production)$ git push
Counting objects: 4, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 474 bytes | 0 bytes/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote:
remote: Running post-receive hook...
remote: [puppet] Updating...
remote: [puppet] Done.
remote:
To ssh://localhost/puppet/control.git
   9d64f56..4a5909a  production -> production

```

Next, let's remove the old common_hosts.pp and common_packages.pp, as we're no longer using them...

```
(production)$ cd ../manifests/

(production)$ ls -al
total 24
drwxr-xr-x   5 bentlema  staff   170 Oct 25 12:11 .
drwxr-xr-x  10 bentlema  staff   340 Oct 28 06:48 ..
-rw-r--r--   1 bentlema  staff   447 Oct 25 12:11 common_hosts.pp
-rw-r--r--   1 bentlema  staff   189 Oct 25 12:11 common_packages.pp
-rw-r--r--   1 bentlema  staff  1687 Oct 25 12:11 site.pp

(production)$ rm -f common_hosts.pp

(production)*$ rm -f common_packages.pp

(production)*$ git commit -a -m 'no longer used in this location'
[production 02c52fe] no longer used in this location
 2 files changed, 24 deletions(-)
 delete mode 100644 manifests/common_hosts.pp
 delete mode 100644 manifests/common_packages.pp

(production)$ git push
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 296 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote:
remote: Running post-receive hook...
remote: [puppet] Updating...
remote: [puppet] Done.
remote:
To ssh://localhost/puppet/control.git
   4a5909a..28151ab  production -> production

```

Okay, great, here's what our files/dirs look like now on the master:

```
[root@puppet environments]# cd /etc/puppetlabs/code/environments/production/
[root@puppet production]# tree hieradata manifests site
hieradata
├── common.yaml
├── location
│   ├── amsterdam.yaml
│   ├── seattle.yaml
│   └── woodinville.yaml
└── node
    ├── agent.example.com.yaml
    ├── gitlab.example.com.yaml
    └── puppet.example.com.yaml
manifests
└── site.pp
site
└── profile
    └── manifests
        ├── common_hosts.pp
        └── common_packages.pp

4 directories, 10 files
```

Note:  the **role/** directory hasn't shown up on the master yet, as git
wont commit it to the control repo until there's at least 1 file within.

Notice that we've just setup our repo to use the Roles & Profiles pattern,
but we've not really used it yet.  Also notice that we've re-located a couple manifests
within our profile module, but we are assigning them in our common.yaml.
The take-away here should be that we can include profiles directly in our
classes hierarchy in Hiera, or we can assign a role, but a role would be
assigned at the node-level or role-level, not in the common.yaml. (We build
a role class for a specific application.)

### TODO

Anyway, now that we're setup with the proper directory structure, and the
environment.conf to set the modulepath, let's do a real example to illistrate
how to use Roles and Profiles...

We should also mention how to use inheritance, as the one place it's still
considered okay to use is in the role class, where we inherit a more general
or common class, and then include application-specific classes to augment it.

### TODO

We also have to decide if we want to assign the role via a custom facter
fact? or do we want to use a hiera key/value pair?

---

Continue on to **Lab #12** --> [Git Basics](12-Git-Basics.md#lab-12)

---

<-- [Back to Contents](/README.md)

---

<-- Back to **Lab #11** - [Roles & Profiles](11-Roles-and-Profiles.md#lab-11)

---

### **Lab #12** - Git Basics

---

### Overview

First of all, Git is a huge topic.  It will be impossible to cover everything in this
quick overview, but we will cover all of what you'll need to work with Git to manage
change to your Puppet code.

What is Git?

Simply put, Git is a distributed version control system.  The book *[Pro Git](https://git-scm.com/book/en/v2)*
describes Git as a mini-filesystem with snapshot capabilities, and all of the Git commands you
utilize manipulate that mini-filesystems and its [stream of snapshots](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics#Snapshots,-Not-Differences).

Git is the tool that will...
- track changes to your code (audit trail)
- allow multiple code-maintainers to make changes simultaneously, and resolve conflicts
- allow multiple complete clones of a repo for disaster recovery

I highly recommend reading *[Pro Git](https://git-scm.com/book/en/v2)* when you get a chance.
It's certainly not required to work through these labs, but if you're the type of person
that can read a technical book and absorb the content for use later, it's a must read.  If
you're like me, and tend to learn more through examples, and then consult the book when you
want to learn more about a specific feature of Git, then use it as a reference.

### The Repository

A "Git Repo" is just a self-contained bundle of files along with its commit history.
As mentioned previously, this *commit history* is like a stream of snapshots.
Typically the files in a Git Repo are source code--in our case: Puppet Manifests.
However, a Git repo can contain any text and/or binary files.  Usually binary files
would be excluded from the repo, as you're probably not interested in versioning
them (e.g. they might be object files left from a compile/software build).  In any
case, Git is happy to store versions of any type of file you might want in your repo.

You can find many public Git repositories out on <https://github.com/explore> but
remember that the nice WebGUI is not the repo itself, it's just a nice view into the
repo.  GitHub is a Git server (or "Git Hosting Service) sitting in front of the
actual Git repo--actually thousands of Git repos.  There are other Git servers out
there too, where both public and private repos can be hosted, and access controled.
We are using one called GitLab for this training.

Some well known Git servers available are:
- [GitHub](http://github.com)
- [Atlassian BitBucket](https://bitbucket.org/)
- [GitLab](https://gitlab.com/explore)
- [Gitolite](http://gitolite.com/)
- [Gerrit Code Review](https://www.gerritcodereview.com)

Let's talk about the basics of Git...

### Git Clone

To make a complete copy of a remote repo, you can use *git clone*

- It makes an exact clone of the remote Git repository.
- Git will also setup a *remote* for pushing to and pulling from
- Git will also configure this remote as a tracking branch

The *remote* is just a meaningful name given to the remote URL which you can refer
to with other git commands.  Git will setup a remote with the name *origin* by default.

Type *git remote -v* to see the configured remote of your control repo.

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ git remote -v
origin  ssh://localhost/puppet/control.git (fetch)
origin  ssh://localhost/puppet/control.git (push)
```

In our case, the remote with name *origin* refers to a repo accessible via
ssh on localhost.  Remember that we had previously setup our ~/.ssh/config
to know the correct ssh port and private key to use for localhost.

Git will also use *origin* as the default for other commands if you don't
specify a remote.

---

### Git Status

The *git status* command is useful for telling you what branch you're on, as
well as the status of the staging area.  When you make changes to files that
Git is tracking, it will notice that, and show you that it noticed.

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ git status
On branch production
Your branch is up-to-date with 'origin/production'.

nothing to commit, working directory clean
```

Notice that our *git status* output tells us that we are on the *production*
branch, and that it also believes we are up-to-date with the remote tracking
branch, referred to as *origin/production*.

As an example, let's edit our *environment.conf* and add a comment at the top
like this:

```
[/Users/Mark/Git/Puppet-Training/control] (production)*$ cat environment.conf
# site hosts local modules, while modules hosts R10K-managed modules
modulepath = site:modules:$basemodulepath
```

We've just made a change in *the working tree*

Now run *git status* again...

```
[/Users/Mark/Git/Puppet-Training/control] (production)*$ git status
On branch production
Your branch is up-to-date with 'origin/production'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

  modified:   environment.conf

no changes added to commit (use "git add" and/or "git commit -a")
```

Now it tells you it noticed that the *environment.conf* was modified in the
working tree.   However, it's still not added to the *staging area*

---

### The Staging Area (Index)

Git has something called a *Staging Area*.  It's where we can "stage" our changes
in preperation to permanently commit them to the repo.  Once you commit a
change, it's there forever in the commit history.

---

### Git Diff

To see what changes we've made in the working tree that are not yet staged,
use *git diff* or *git diff <filename>* for a specific file.   In our example,
if you do *git diff environment.conf* we'll see the comment we added with a
*plus* at the beginning of the line indicating that it was added.  If you're
on a ANSI color terminal, it will also be in *GREEN*.  Any lines removed would
be in *RED*.  Unchanged lines displayed for context, would just be in your
default terminal font color.

```
[/Users/Mark/Git/Puppet-Training/control] (production)*$ git diff environment.conf
diff --git a/environment.conf b/environment.conf
index ec1ce4e..0de3371 100644
--- a/environment.conf
+++ b/environment.conf
@@ -1 +1,2 @@
+# site hosts local modules, while modules hosts R10K-managed modules
 modulepath = site:modules:$basemodulepath
```

---

### Git Add

To add a file to the staging area, use the *git add* command.  In our example,
we can simply run *git add environment.conf*

```
[/Users/Mark/Git/Puppet-Training/control] (production)*$ git status
On branch production
Your branch is up-to-date with 'origin/production'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

  modified:   environment.conf

```

Also, notice now that if we run *git diff* we dont see any changes.  This is
because our changes in the working tree have been staged, so the working tree
matches what's in the staging area.

To see difference between the remote branch (origin/production) and the staging
area, you can run *git diff* like this:

```
[/Users/Mark/Git/Puppet-Training/control] (production)*$ git diff origin/production environment.conf
diff --git a/environment.conf b/environment.conf
index ec1ce4e..0de3371 100644
--- a/environment.conf
+++ b/environment.conf
@@ -1 +1,2 @@
+# site hosts local modules, while modules hosts R10K-managed modules
 modulepath = site:modules:$basemodulepath
```

The *git diff* command can show you a diff of pretty much anything you want,
it's just a matter of knowing what you want to compare.

There are lots of things you could compare:

1. local working-tree
2. local staging area (also called the *index* in the git man pages)
3. local repo / same branch
4. local repo / different branc
5. remote repo / same branch
6. remote repo / different branch

We will go over all of these in a later lab...

---

### Git Commit

Now that we're run the *git add* on our file, and the *git status* shows that
it's staged (ready to be committed), we can go ahead and either:

1. commit the change
2. un-stage the change

Let's go ahead an commit the change. We will talk more about un-staging in a later lab...

```
[/Users/Mark/Git/Puppet-Training/control] (production)*$ git commit -m 'added comment'
[production 8c229f1] added comment
 1 file changed, 1 insertion(+)
```

The *-m* option is short for *commit message* and is just a descriptive message
to go along with the commit in case we need to find it in the future, the message
should make it easier to identify later what changes were made

---

### Git Push

If we do a *git status* again, we'll see something new:

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ git status
On branch production
Your branch is ahead of 'origin/production' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working directory clean
```

Notice that Git is telling us *Your branch is ahead of 'origin/production' by 1 commit.*

Isn't that nice of it?

Remember earlier we looked at *git clone* and mentioned that when we clone a
repo, Git will automatically setup the remote tracking branch.  Git is comparing
our repo's production branch with what it knows about the remote repo's production
branch, and it's noticing that we just made 1 new commit, but the remote doesn't
yet have that commit.  We need to *git push* it

Note:  Git only knows about the remote since the last time we did a *git fetch*
or *git pull*.  It's *NOT* actively going out a looking at the real remote repo
every time you do a *git status*.  It's just remembering from it's local copy
of the remote.

Let's push our change to the remote (in this case, our GitLab server)...

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ git push
Counting objects: 6, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 335 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote:
remote: Running post-receive hook...
remote: [puppet] Updating...
remote: [puppet] Done.
remote:
To ssh://localhost/puppet/control.git
   64f3df7..8c229f1  production -> production
```

...and do another *git status* ...

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ git status
On branch production
Your branch is up-to-date with 'origin/production'.

nothing to commit, working directory clean
```

---

### Git Pull

If you have multiple people working in the same repo, they will also have a
clone on their local workstation, and will also be making changes and
adding/committing/pushing up to the remote.  If you're both working on the
same files, it's possible you could have an outdated copy if the other person
had edited a file and pushed their change to the remote after you cloned it.
So how to we keep our *local* clone up-to-date with the *remote*?

A good habit to get into is to also do a *git pull* prior to doing any work
in your local clone of a repo.  This ensures you're pulling down any changes
other folks have made, and will potentially avoid merge conflicts in the future.

When you do a **git pull** Git will pull down changes to the current branch,
and bring your branch up-to-date with the remote.  It will do this by mergeing
in those changes automatically for you.  It is possible if you've changed
the same file as someone else, you could get a merge conflict, and have to
resolve that conflict, and then manually add and commit the changes.

Git can also be configured to fetch all changes in other branches as well.
Depending on the version of Git your using, this behavior may differ, so just
to be safe, it's also good to *git pull* after switching to a different branch.

---

### Git Branches

Although we've seen *branches* a little bit (e.g. *production branch*) we've
not really talked about what a branch is.  Git allows us to spin off a copy of
our repo, makes changes within that copy (called a branch), and then either
merge our changes up to the parent branch, or discard our changes.  This idea
becomes very useful when making changes to puppet code without affecting the
production environment, but still allowing us to test our code.

The easiest way to create a new branch is to use *git checkout -b <branch-name>*

It's a shortcut for creating a branch, and then checking out that branch.
The longer version would be a *git branch <branch-name>*
followed by *git checkout <branch-name>* but why type all of that?

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ git status
On branch production
Your branch is up-to-date with 'origin/production'.

nothing to commit, working directory clean

[/Users/Mark/Git/Puppet-Training/control] (production)$ git branch foo

[/Users/Mark/Git/Puppet-Training/control] (production)$ git status
On branch production
Your branch is up-to-date with 'origin/production'.

nothing to commit, working directory clean

[/Users/Mark/Git/Puppet-Training/control] (production)$ git branch
  development
  foo
* production

[/Users/Mark/Git/Puppet-Training/control] (production)$ git checkout foo
Switched to branch 'foo'

[/Users/Mark/Git/Puppet-Training/control] (foo)$ git branch
  development
* foo
  production
```

We've just created a new branch called **"foo"**, then checked it out to work on.

Notice that **checkout** is just another way of saying **switch to this branch**

Also, notice we've seen the new command **git branch** which shows us the branches
in the repo, and puts an asterisk next to the currently-checked-out branch.

---

### Git Merge

When we make changes in another branch, it's possible that we will eventually
want to merge those changes back in to the branch we split off of.

We branched off of *production* and want to make a change, test it, and then
merge it back in to *production*

Let's add a silly comment to our environment.conf again...

```
[/Users/Mark/Git/Puppet-Training/control] (foo)$ vi environment.conf

[/Users/Mark/Git/Puppet-Training/control] (foo)*$ cat environment.conf
# site hosts local modules, while modules hosts R10K-managed modules
modulepath = site:modules:$basemodulepath
# this is another wonderful comment at the end of the file
```

Let's add/commit that change...

```
/Users/Mark/Git/Puppet-Training/control] (foo)*$ git status
On branch foo
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

  modified:   environment.conf

no changes added to commit (use "git add" and/or "git commit -a")

[/Users/Mark/Git/Puppet-Training/control] (foo)*$ git commit -a -m 'a silly comment'
[foo eb20962] a silly comment
 1 file changed, 1 insertion(+)
```

Now let's look at the differences between our **production** branch and our **foo** branch

```
[/Users/Mark/Git/Puppet-Training/control] (foo)$ git diff production environment.conf
diff --git a/environment.conf b/environment.conf
index 0de3371..6c2bae3 100644
--- a/environment.conf
+++ b/environment.conf
@@ -1,2 +1,3 @@
 # site hosts local modules, while modules hosts R10K-managed modules
 modulepath = site:modules:$basemodulepath
+# this is another wonderful comment at the end of the file
```

You could also explicitely specify the two branches like this:

```
git diff production..foo environment.conf
```

...but if you don't explicitely specify the second branch, git will compare
the currently-checked-out branch with the one specified.

Now, to do a merge, we have to **checkout the branch we want to merge *in* to**
first, so **git checkout production**

```
[/Users/Mark/Git/Puppet-Training/control] (foo)$ git checkout production
Switched to branch 'production'
Your branch is up-to-date with 'origin/production'.

[/Users/Mark/Git/Puppet-Training/control] (production)$ git merge foo
Updating 8c229f1..eb20962
Fast-forward
 environment.conf | 1 +
 1 file changed, 1 insertion(+)

[/Users/Mark/Git/Puppet-Training/control] (production)$ cat environment.conf
# site hosts local modules, while modules hosts R10K-managed modules
modulepath = site:modules:$basemodulepath
# this is another wonderful comment at the end of the file

[/Users/Mark/Git/Puppet-Training/control] (production)$ git status
On branch production
Your branch is ahead of 'origin/production' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working directory clean
```

A few things happened there:

1. We checked out production
2. We merged foo in to production
3. Because the merge didn't encounter any conflicts, it was automatically committed
4. Notice our production branch is 1 commit behind origin/production

Now that we've merged foo back in to production, we likely want to update the
remote repo as well with a **git push**, but first, let's use **git diff** to compare
our local repo with the remote repo:

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ git diff origin/production environment.conf
diff --git a/environment.conf b/environment.conf
index 0de3371..6c2bae3 100644
--- a/environment.conf
+++ b/environment.conf
@@ -1,2 +1,3 @@
 # site hosts local modules, while modules hosts R10K-managed modules
 modulepath = site:modules:$basemodulepath
+# this is another wonderful comment at the end of the file
```

Okay, let's go ahead an push our production branch to the remote...

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ git push
Counting objects: 6, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 339 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote:
remote: Running post-receive hook...
remote: [puppet] Updating...
remote: [puppet] Done.
remote:
To ssh://localhost/puppet/control.git
   8c229f1..eb20962  production -> production
```

---

### More about branches

Let's do a `git branch -a` to see all of our local and remote branches...

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ git branch -a
  development
  foo
* production
  remotes/origin/development
  remotes/origin/production
```

Notice that even after we did a push, the *foo* branch is local only.  If we
want a remote tracking branch for *foo*, we could push it as well...

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ git checkout foo
Switched to branch 'foo'

[/Users/Mark/Git/Puppet-Training/control] (foo)$ git push
fatal: The current branch foo has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin foo

```

Isn't that nice of Git to tell us how to do things we likely want to do?

```
[/Users/Mark/Git/Puppet-Training/control] (foo)$ git push --set-upstream origin foo
Total 0 (delta 0), reused 0 (delta 0)
To ssh://localhost/puppet/control.git
 * [new branch]      foo -> foo
Branch foo set up to track remote branch foo from origin.

[/Users/Mark/Git/Puppet-Training/control] (foo)$ git branch -a
  development
* foo
  production
  remotes/origin/development
  remotes/origin/foo
  remotes/origin/production

```

Cool, now we see a **remotes/origin/foo** in the list, which means our branch
exists on the remote called **origin** and our local branch is tracking it


---

### Another example...

We configured Hiera in an earlier lab, but one thing we left un-done was the
location of the **hiera.yaml**.  It's fine sitting where it's at, but it's
outside of Git control.  Wouldn't we like our **hiera.yaml** to be safely located
in our Git control repo along with the actual Hiera Data?  Let's move it...

The **hiera.yaml** is a pretty small YAML text file, so let's just copy-and-paste
to pull it off of our master, and get it in our Git repo..

On your master...

```
[root@puppet ~]# cat /etc/puppetlabs/puppet/hiera.yaml
---
:backends:
  - yaml

:hierarchy:
  - "node/%{::trusted.certname}"
  - "role/%{::role}"
  - "location/%{::location}"
  - common

:yaml:
  :datadir: "/etc/puppetlabs/code/environments/%{environment}/hieradata"

```

Now on your workstation where you are hosting your own clone of the control repo,
let's create the **hiera.yaml** at the **top-level** of your repo.  (It should
be at the same level as your **environment.conf**)

```
[/Users/Mark/Git/Puppet-Training/control] (production)$ echo '
---
:backends:
  - yaml

:hierarchy:
  - "node/%{::trusted.certname}"
  - "role/%{::role}"
  - "location/%{::location}"
  - common

:yaml:
  :datadir: "/etc/puppetlabs/code/environments/%{environment}/hieradata"
' >> hiera.yaml

```

Now cat your new hiera.yaml and make sure it looks correct (comparing it to
the one you just copied)...

```
[/Users/Mark/Git/Puppet-Training/control] (production)*$ cat hiera.yaml
---
:backends:
  - yaml

:hierarchy:
  - "node/%{::trusted.certname}"
  - "role/%{::role}"
  - "location/%{::location}"
  - common

:yaml:
  :datadir: "/etc/puppetlabs/code/environments/%{environment}/hieradata"

```

Okay, now let's add, commit, and push it...

```
[/Users/Mark/Git/Puppet-Training/control] (production)*$ git add hiera.yaml

[/Users/Mark/Git/Puppet-Training/control] (production)*$ git status
On branch production
Your branch is up-to-date with 'origin/production'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

  new file:   hiera.yaml

[/Users/Mark/Git/Puppet-Training/control] (production)*$ git commit -m 'put under git control'
[production 522c89a] put under git control
 1 file changed, 15 insertions(+)
 create mode 100644 hiera.yaml

[/Users/Mark/Git/Puppet-Training/control] (production)$ git push
Counting objects: 5, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 444 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote:
remote: Running post-receive hook...
remote: [puppet] Updating...
remote: [puppet] Done.
remote:
To ssh://localhost/puppet/control.git
   708d37f..522c89a  production -> production

```

Next, we should move the original **hiera.yaml** off to the side, and then make a symlink to the new location.


```
[root@puppet ~]# cd /etc/puppetlabs/puppet

[root@puppet puppet]# ls -al hiera*
-rw-r--r-- 1 root root 198 Mar  2 15:43 hiera.yaml
-rw-r--r-- 1 root root 314 Mar  2 15:27 hiera.yaml.orig

[root@puppet puppet]# mv hiera.yaml hiera.yaml-2017-01-21

[root@puppet puppet]# ln -s environments/production/hiera.yaml

[root@puppet puppet]# ls -al hiera*
lrwxrwxrwx 1 root root  34 Mar 16 11:30 hiera.yaml -> environments/production/hiera.yaml
-rw-r--r-- 1 root root 198 Mar  2 15:43 hiera.yaml-2016-03-16
-rw-r--r-- 1 root root 314 Mar  2 15:27 hiera.yaml.orig

```

Note:  even though we've moved our **hiera.yaml** under Git control, gaining the ability
to track changes, don't forget that a restart of the puppet master is required in
order to re-read the file and "activate" the changes.


---

### Example 2:  Remember that we have a develpment branch?

We've largely been working with the **production** branch in our control repo, but
remember that we've created a **development** branch as well?

Back in Lab #11, we setup our code base to use the **Roles & Profiles Pattern**
but we only did this for the production branch.  Let's also get the development
branch setup properly.


```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (production)$ git branch -a
  development
  foo
* production
  remotes/origin/development
  remotes/origin/foo
  remotes/origin/production

mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (production)$ git checkout development
Branch development set up to track remote branch development from origin.
Switched to a new branch 'development'

mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)$ git diff --stat production
 environment.conf                                         | 20 ++++++++++++++++++--
 hiera.yaml                                               | 13 -------------
 hieradata/common.yaml                                    |  5 ++---
 hieradata/node/agent.example.com.yaml                    |  1 +
 {site/profile/manifests => manifests}/common_hosts.pp    |  2 +-
 {site/profile/manifests => manifests}/common_packages.pp |  2 +-
 6 files changed, 23 insertions(+), 20 deletions(-)
```

Notice that there are a bunch of differences between **production** and **development** branches.

Let's try to merge the **production** changes in to the current branch, which is the **development** branch.

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)$ git merge --no-commit production
Auto-merging site/profile/manifests/common_packages.pp
Auto-merging site/profile/manifests/common_hosts.pp
Auto-merging hieradata/common.yaml
CONFLICT (content): Merge conflict in hieradata/common.yaml
Automatic merge failed; fix conflicts and then commit the result.
```

Hmmm, we got a merge conflict with our common.yaml.  Let's look at that.

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)*$ git status
On branch development
Your branch is up-to-date with 'origin/development'.
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Changes to be committed:

    modified:   environment.conf
    new file:   hiera.yaml
    renamed:    manifests/common_hosts.pp -> site/profile/manifests/common_hosts.pp
    renamed:    manifests/common_packages.pp -> site/profile/manifests/common_packages.pp

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:   hieradata/common.yaml

```

Looks like our `common.yaml` has conflicts.  Since we know that the version
we have in the **production** branch is correct, let's just checkout that version.  If we
really wanted to, we could also just edit the file with conflicts, and make the desired
changes, git add them, commit, and push, and be done with it.

Follow along and see how we can resolve the conflict(s)...

Let's look at our `common.yaml` ...

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)*$ git diff hieradata/common.yaml
diff --cc hieradata/common.yaml
index 05bee3b,bc84197..0000000
--- a/hieradata/common.yaml
+++ b/hieradata/common.yaml
@@@ -1,8 -1,9 +1,14 @@@
  ---

  classes:
++<<<<<<< HEAD
 +   - common_hosts
 +   - common_packages
++=======
+    - profile::common_hosts
+    - profile::common_packages
+    - motd
++>>>>>>> production

  ntp::servers:
     - '0.pool.ntp.org'
```

Okay, we see that the conflict is in the common_hosts and common_packages classes.
We moved them, but hadn't updated the development branch.  Since we want what is
in production, let's simply checkout the production version, git add, commit, and push.


```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)*$ git checkout --theirs hieradata/common.yaml

mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)*$ git status
On branch development
Your branch is up-to-date with 'origin/development'.
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Changes to be committed:

    modified:   environment.conf
    new file:   hiera.yaml
    renamed:    manifests/common_hosts.pp -> site/profile/manifests/common_hosts.pp
    renamed:    manifests/common_packages.pp -> site/profile/manifests/common_packages.pp

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:   hieradata/common.yaml
```

Okay, now add it and do a `git status` again...

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)*$ git add hieradata/common.yaml

mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)*$ git status
On branch development
Your branch is up-to-date with 'origin/development'.
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

    modified:   environment.conf
    new file:   hiera.yaml
    modified:   hieradata/common.yaml
    renamed:    manifests/common_hosts.pp -> site/profile/manifests/common_hosts.pp
    renamed:    manifests/common_packages.pp -> site/profile/manifests/common_packages.pp
```

You fixed the conflict, and are ready to commit...

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)*$ git commit -m 'merge production in to development'
[development 9a8ba50] merge production in to development

mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)$ git push
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 346 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote:
remote: To create a merge request for development, visit:
remote:   http://gitlab.example.com/puppet/control/merge_requests/new?merge_request%5Bsource_branch%5D=development
remote:
remote:
remote: Running post-receive hook...
remote: [puppet] Updating...
remote: [puppet] Done.
remote:
To ssh://localhost/puppet/control.git
   5053c6f..eee0234  development -> development
```

Let's compare the production and development environments on the master and see what we see...

```
[root@puppet ~]# cd /etc/puppetlabs/code/environments

[root@puppet environments]# (cd production && tree hieradata manifests site)
hieradata
├── common.yaml
├── location
│   ├── amsterdam.yaml
│   ├── seattle.yaml
│   └── woodinville.yaml
└── node
    ├── agent.example.com.yaml
    ├── gitlab.example.com.yaml
    └── puppet.example.com.yaml
manifests
└── site.pp
site
└── profile
    └── manifests
        ├── common_hosts.pp
        └── common_packages.pp

4 directories, 10 files

[root@puppet environments]# (cd development && tree hieradata manifests site)
hieradata
├── common.yaml
├── location
│   ├── amsterdam.yaml
│   ├── seattle.yaml
│   └── woodinville.yaml
└── node
    ├── agent.example.com.yaml
    ├── gitlab.example.com.yaml
    └── puppet.example.com.yaml
manifests
└── site.pp
site
└── profile
    └── manifests
        ├── common_hosts.pp
        └── common_packages.pp

4 directories, 10 files
```

Now let's see if we have any other differences between production and development:

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)$ git diff --stat production
 hieradata/node/agent.example.com.yaml | 1 +
 1 file changed, 1 insertion(+)

mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)$ git diff production
diff --git a/hieradata/node/agent.example.com.yaml b/hieradata/node/agent.example.com.yaml
index 63453d9..7d07440 100644
--- a/hieradata/node/agent.example.com.yaml
+++ b/hieradata/node/agent.example.com.yaml
@@ -5,5 +5,6 @@ location: 'amsterdam'
 classes:
    - ntp
    - timezone
+   - motd

 timezone::timezone: 'US/Pacific'
```

We do have one difference.  We have the **motd** class in our `agent.example.com.yaml` in the development branch,
but not the production branch.  Since we already have the **motd** module declared in the `common.yaml` we dont
need it in the `agent.example.com.yaml`.

Rather than simply editing the `common.yaml`, let's do another merge, this time in the other direction.
Let's merge **production** <-- **development**.  Since I already know there will not be any conflicts,
I'm going to do the merge **without** the `--no-commit` option.  Doing the merge this way will immediately
commit the changes.

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (development)$ git checkout production
Switched to branch 'production'
Your branch is up-to-date with 'origin/production'.

mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (production)$ git merge development
Updating 95d7b55..eee0234
Fast-forward
 hieradata/node/agent.example.com.yaml | 1 +
 1 file changed, 1 insertion(+)

mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (production)$ git push
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: Running post-receive hook...
remote: [puppet] Updating...
remote: [puppet] Done.
remote:
To git@localhost:puppet/control
   02c52fe..9a8ba50  production -> production
```

Notice that we always merge in to the current branch (the one we have checked out).  So if we
want to merge the differences in the **development** branch in to the **production** branch,
we ensure we have the **production** branch **checked out**, and then execute the merge
specifying the **"source"** branch.

Now, if you do another `git diff` you'll see that there are no more differences:

```
mbp-mark:[/Users/bentlema/Documents/Git/Puppet-Tutorial/control] (production)$ git diff --stat development
```

---

Continue on to **Lab 13** --> [Git Workflow](/tutorial/vbox/13-Git-Workflow.md)

---

<-- [Back to Contents](/README.md)

---

<-- Back to **Lab #12** - [Git Basics](12-Git-Basics.md#lab-12)

---

### **Lab #13** - Git Workflow

### Under Construction

---

### Overview

We will work on a few things in this lab:

- Introduce a workflow for deploying puppet code to your puppet infrastructure
- More on Git branching and merging

### What the heck is a Workflow?

Well, it's just the order of tasks we perform to accomplish some work.  In our
case, the work is making changes to our puppet code to affect our infrastructure,
while not breaking the production environment.

The high level description of a puppet workflow goes like this:

1.  Clone control repo, or if previously cloned, bring it up-to-date with remote
2.  Change dir to the repository to give Git a context to work
3.  Create new *feature branch* to commit your changes to
4.  Switch to your new feature branch
5.  Add or edit files to affect puppet
6.  Commit and Push your changes to the remote
7.  The push will trigger R10K run to build your environments, including a new environment named after your feature branch
8.  Login to a canary system, or system dedicated to testing puppet, and do a test puppet run on that host only to test your code
9.  Repeat from step 5 above until you're satisfied with your code
10. Switch to production branch, and merge in your feature branch


### Local and Remote Repositories

It is important to understand that your local clone of a repo is a full
complete copy of the repo, and identical to the remote repo as of the time
it was cloned.  However, the second your clone is created, it will diverge
from the remote as other developers push their changes up to the remote.

You do not automatically receive changes that are committed/pushed to the "upstream"
Git repo.  So commands like *git status* that tell you if you're ahead
or behind the remote tracking repo will probably only be useful if you're
truely up-to-date with the remote.

### Keeping Your Local Repo Up-to-date

To bring your local repository up-to-date with the remote, there are a few commands to know about:

- git fetch
- git pull

The *git fetch* command will fetch all objects and refs from a remote repository.

The *git pull* command will implicitely fetch, but will also merge in any new commits.

### STILL UNDER DEVELOPMENT ...


---

### Further Reading

http://stackoverflow.com/questions/3258243/check-if-pull-needed-in-git
http://stackoverflow.com/questions/2688251/what-is-the-difference-between-git-fetch-origin-and-git-remote-update-origin
http://stackoverflow.com/questions/1856499/differences-between-git-remote-update-and-fetch
http://stackoverflow.com/questions/17712468/what-is-the-difference-between-git-remote-update-git-fetch-and-git-pull

### Under Construction

Should cover the topic of creating *topic branches* for testing, or the use of a *long-lived personal dev branch*

Should cover how to DELETE a branch (Clean up after done working with a topic (aka "feature") branch


---

Continue on to Lab #14

---

<-- [Back to Contents](/README.md)

---


---

### Lab ??
### Disable the PE Console Node Classifier

---

### Overview ###

Time to complete:  60 minutes

```
TODO TODO TODO TODO TODO TODO TODO TODO TODO TODO

We need to talk about the site.pp, and the **node** declaration, as well as the PE Console and classifying nodes.

TODO TODO TODO TODO TODO TODO TODO TODO TODO TODO
```

### What classes are assigned by the PE Console? ###

On any puppetized host you can find what classes are assigned to it (how it is "classified") by looking at the contents of the classes.txt file.  The classes.txt is generated upon every puppet agent run.

Here are the classes we see assigned to our agent node:

```
[root@agent ~]# find /var -name classes.txt
/var/opt/lib/pe-puppet/classes.txt
[root@agent ~]# cat /var/opt/lib/pe-puppet/classes.txt | sort | uniq
common_hosts
common_packages
default
ntp
ntp::config
ntp::install
ntp::params
ntp::service
puppet_enterprise
puppet_enterprise::mcollective::server
puppet_enterprise::mcollective::server::certs
puppet_enterprise::mcollective::server::facter
puppet_enterprise::mcollective::server::logs
puppet_enterprise::mcollective::server::plugins
puppet_enterprise::mcollective::service
puppet_enterprise::params
puppet_enterprise::profile::agent
puppet_enterprise::profile::mcollective::agent
puppet_enterprise::symlinks
settings
timezone
timezone::params
```

Here are teh classes we see assigned to our puppet master node:

```
[root@puppet ~]# cat /var/opt/lib/pe-puppet/classes.txt | sort | uniq
common_hosts
common_packages
default
ntp
ntp::config
ntp::install
ntp::params
ntp::service
pe_concat::setup
pe_console_prune
pe_repo
pe_repo::platform::el_7_x86_64
pe_staging
puppet_enterprise
puppet_enterprise::amq
puppet_enterprise::amq::certs
puppet_enterprise::amq::config
puppet_enterprise::amq::service
puppet_enterprise::console
puppet_enterprise::console::config
puppet_enterprise::console::console_auth_config
puppet_enterprise::console::database
puppet_enterprise::console::service
puppet_enterprise::console_services
puppet_enterprise::license
puppet_enterprise::master
puppet_enterprise::master::puppetserver
puppet_enterprise::mcollective::server
puppet_enterprise::mcollective::server::certs
puppet_enterprise::mcollective::server::facter
puppet_enterprise::mcollective::server::logs
puppet_enterprise::mcollective::server::plugins
puppet_enterprise::mcollective::service
puppet_enterprise::packages
puppet_enterprise::params
puppet_enterprise::profile::agent
puppet_enterprise::profile::amq::broker
puppet_enterprise::profile::certificate_authority
puppet_enterprise::profile::console
puppet_enterprise::profile::console::apache_proxy
puppet_enterprise::profile::console::certs
puppet_enterprise::profile::console::console_services_config
puppet_enterprise::profile::master
puppet_enterprise::profile::master::auth_conf
puppet_enterprise::profile::master::classifier
puppet_enterprise::profile::master::console
puppet_enterprise::profile::master::mcollective
puppet_enterprise::profile::master::puppetdb
puppet_enterprise::profile::mcollective::agent
puppet_enterprise::profile::mcollective::console
puppet_enterprise::profile::mcollective::peadmin
puppet_enterprise::profile::puppetdb
puppet_enterprise::puppetdb
puppet_enterprise::puppetdb::database_ini
puppet_enterprise::puppetdb::jetty_ini
puppet_enterprise::puppetdb::service
puppet_enterprise::symlinks
settings
timezone
timezone::params
```

Wow!  Where did all those come from?



### More about node classification... ###


In Puppet Enterprise, the so-called 'PE Console' actualy comes configured
out-of-the-box as an "External Node Classifier" (or ENC).  Unfortunately,
in a "Large Environment Install" (or "LEI") the Console becomes a bottleneck
to scaling out.  When your install base exceeds 1000 or so agents, and every
agent needs to wait for the PE Console to tell the puppet master what environment
the agent belongs to, and what classes to apply to it, things go slow.

For this reason, Puppet Labs disables the PE Console ENC.  The settings 
that controls this is in the puppet.conf, as is:

```
[master]
node_terminus = classifier
```

You might think you can just edit the puppet.conf and change this setting
to `node_terminus = plain` and call it good.  Go ahead and try that, and
run puppet again, and see what happens:

```
[root@puppet puppet]# puppet agent -t
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for puppet
Info: Applying configuration version '1454024977'
Notice: /Stage[main]/Puppet_enterprise::Profile::Master::Classifier/Pe_ini_setting[node_terminus]/value: value changed 'plain' to 'classifier'
Info: Class[Puppet_enterprise::Profile::Master::Classifier]: Scheduling refresh of Service[pe-puppetserver]
```

What happens is that puppet is configured to configure itself, and so sees
that it's config changed, and changes it back to what it thinks it should be.
So how do we configure puppet to configure the puppet master the way we
want?

Because you didn't restart the pe-puppetserver service, when you run the agent,
it talks back to the master (which happens to be running on the same host, but
hasn't yet re-read it's edited config file) and the master re-applies what it
believes it the config should be.  Puppet is just doing what it's made to do!

Edit the puppet.conf again, and change node_terminus to plain, and restart the puppet master with:

```
[root@puppet puppet]# systemctl restart pe-puppetserver
```

If you try to run the puppet agent now, you'll find that the master has lost its mind, and 
doesn't know it's a master.  Don't do that.

Then add the following to the site.pp, and run `puppet agent -t`

```
class { 'puppet_enterprise':
  certificate_authority_host   => 'puppet.example.com',
  puppet_master_host           => 'puppet.example.com',
  console_host                 => 'puppet.example.com',
  puppetdb_host                => 'puppet.example.com',
  database_host                => 'puppet.example.com',
  mcollective_middleware_hosts => [ 'puppet.example.com' ]
}

include puppet_enterprise::profile::agent
include puppet_enterprise::profile::mcollective::agent

node puppet {
  class { 'puppet_enterprise::profile::master':
      classifier_host => false,
  } ->
  pe_ini_setting { 'disable_console_classifier':
    ensure  => present,
    path    => '/etc/puppetlabs/puppet/puppet.conf',
    section => 'master',
    setting => 'node_terminus',
    value   => 'plain',
    notify  => Service['pe-puppetserver']
  }
}
```






```







NOTE:

Need to re-do everything following, as I re-did the above code







```







See Zee's "LEI Wrapper" module to accomplish this, and
simplify PE configuration.

<https://github.com/pizzaops/pizzaops-lei_wrapper>

Without this, you would not be able to set the environment in the agent's
puppet.conf, as the PE Console ENC would override that.

Our tiny training environment is certainly not an LEI, but for the sake
of learning, let's go ahead and walk through the process of disabling
the PE Console ENC.

---

### Disable the PE Console ENC ###

This really needs to be split out into a separate lab...

To disable the use of the PE Console as an ENC, we have to simply:

* Edit puppet.conf and set **node_terminus = plain**
* Ensure that the next puppet run doesn't change it back
* Ensure that without the PE Console ENC, are master and agents are still properly classified

We could simply use the pizzaops-lei_wrapper module, but in this lab, we will
walk through the steps manually so that we understand all that is happening...


an copy-and-paste the above code in to our
site.pp as we will require this in a later lab.  As we are adding this
to our **node puppet** make sure we replace the existing definition, or
only copy and paste the contents of the code within the existing definition.

This is only half of what we need to do, though.  We also need to
change the value of node_terminus to **plain** in our puppet.conf.  Because
this setting in the puppet.conf on the master is actually managed by 
puppet itself, we have to jump through a few hoops.

via the PE Console:

* Login as 'admin'
* Click **Classification** tab
* Click **PE Master**
* Click **Classes** sub-tab
* Scroll down to where you see **Class: puppet_enterprise::profile::master**
* Under **Parameter** find **classifier_host** in the list and set to **false**
* Click **Add Parameter** to save the value
* Click **Commit 1 change** at the bottom of the screen

Now, run **puppet agnet -t** and you should see the setting be changed, and the puppetmaster restarted:

```
Notice: /Stage[main]/Main/Node[puppet]/Pe_ini_setting[disable_console_classifier]/value: value changed 'classifier' to 'plain'
Info: /Stage[main]/Main/Node[puppet]/Pe_ini_setting[disable_console_classifier]: Scheduling refresh of Service[pe-puppetserver]
Notice: /Stage[main]/Puppet_enterprise::Master::Puppetserver/Service[pe-puppetserver]: Triggered 'refresh' from 1 events
```

### But! ###

What else have we just done?  We've just disabled the PE Console as an ENC,
which is what classified the puppet master.  The puppet master no longer knows
that it's a puppet master.  So, let's fix that by adding the following to
our site.pp within the **node puppet** definition.

```
include puppet_enterprise::profile::master
```



---

Continue to **Lab #5** --> [Practice doing some puppet code, and puppet runs](05-Puppet-Code-Practice.md)

---

Further Reading:

For more info on how the site.pp **main manifest** is configured and used,
see the PuppetLabs docs at:

<https://docs.puppetlabs.com/puppet/3.8/reference/dirs_manifest.html>

---

<-- [Back to Contents](/README.md)

---

### Terms

### Puppet Master
   - Certificate Authority (CA)
   - Master of Masters (MoM) - manages the other Puppet Infra nodes configuration (Compile masters, PuppetDB, PE Console, ActiveMQ hub/spokes)
   - Compile Master - another Puppet master that Agents point directly at

### Puppet Agent
   - Every node out there gets an agent.  Even the masters run the puppet agent, and that's how they too get configured.


### Puppet Terminology:

    Type
    Provider
    Resources
    Class
    Manifest
    Template
    Fact
    Hiera
    Roles & Profiles (are just classes organized in a meaningful way)
    Environment

### Key config items configured in the puppet.conf

    server - the puppet master server to point at (typically one of the compile masters)
    environment - often correspond to 'Application Tiers' such as "Dev, Stage, Prod"
    certname - usually the same as the hosts FQDN, but can me anything you want.

### Key key key thing about Roles and Profiles:

    Role is a Module
    Profile is a Module
    You should be able to classify a node with a SINGLE role and THAT’S IT. This makes
    node  classification simple and static:
        - the node gets its role
        - the role includes profiles
        - profiles call out to Hiera for data
        - that data is passed to component modules


### Git Terminology:

    Git Repository / "Repo" - a directory that Git knows about changes in / tracks changes in
    Git Server - a server setup to host Git repositories
    Branch - Each repo can have any number of forks off of the master branch, and they are called 'branches'
    Commit - a snapshot of differences relative to the previous commits
    Local vs Remote Repositories
    Push - push commits to a remote repository
    Pull - fetch and merge remote changes into your local repository
    Merge - merge differences in one branch to another

### How doe we use Puppet and Git together?

    R10K
    Puppetfile

### How does a node (AKA host) know what puppet master to talk to?

### How is the certname configured?

Talk about the /etc/puppetlabs/puppet/puppet.conf
   [main]
   [master]
   [agent]


### What happens during a puppet run?

    - Send Facts to puppetmaster (they get instantiated as top-scope puppet variables)
    - Start building the client config catalog starting with the site.pp manifest
    - Within the site.pp there can be:
         Other static top-scope variables set (Note: any variable defined within the site.pp is automatically "Top Scope")
         Conditional statements and resource declarations that affect every node
         Hiera function calls
         Class declarations with class parameters that affect every node
         Include statements that affect every node
         Node definitions that can contain puppet code that only affects a specific node or sub-set of notes
            (Note: there is a special node 'default' definition that applies to any node that doesn't have its own node definition.)



### Troubleshooting:

    - Verify the certname in puppet.conf matches the name of the node yaml file
    - If hostname is wrong and you fix it, you must also update the 'certname' in the puppet.conf, and re-gen and re-sign the client cert
    - Are you using the correct Puppetfile?
    - Verify your files (The ones you think you've added or changed) got pushed to the puppetmasters, and to the correct environment
    - For PE3.8 you may want to run the update-all-masters.sh script, or make sure to do your 'git push' on puppet-site-temp last
    - Use MCollective to ensure the classes are getting applied to your host
    - Trace through from node.yaml --> role.yaml --> classes (sometimes both the node and role yaml files will have classes)


### Great Series on Roles, Profiles, and Puppet Workflow:

Building a Functional Puppet Workflow Part 1: Module Structure
http://garylarizza.com/blog/2014/02/17/puppet-workflow-part-1/

Building a Functional Puppet Workflow Part 2: Roles and Profiles
http://garylarizza.com/blog/2014/02/17/puppet-workflow-part-2/

Building a Functional Puppet Workflow Part 3: Dynamic Environments With R10k
http://garylarizza.com/blog/2014/02/18/puppet-workflow-part-3/

Building a Functional Puppet Workflow Part 3b: More R10k Madness
http://garylarizza.com/blog/2014/03/07/puppet-workflow-part-3b/

On R10k and 'Environments'
http://garylarizza.com/blog/2014/03/26/random-r10k-workflow-ideas/

R10k + Directory Environments
http://garylarizza.com/blog/2014/08/31/r10k-plus-directory-environments/

On Dependencies and Order
http://garylarizza.com/blog/2014/10/19/on-dependencies-and-order/

Workflows Evolved: Even Besterer Practices
http://garylarizza.com/blog/2015/11/16/workflows-evolved-even-besterer-practices/


Foreman

Even though PE includes the PE Console for node classification, it would be cool to configure The Foreman as an ENC just to show how an ENC can be tied in.

http://theforeman.org/manuals/1.5/index.html#3.5.5FactsandtheENC

---

<-- [Back to Contents](/README.md)

---


These values come from the [main] section of the puppet.conf on the master:

[root@puppet]# puppet config print basemodulepath
/etc/puppetlabs/puppet/modules:/opt/puppet/share/puppet/modules

[root@puppet]# puppet config print environmentpath
/etc/puppetlabs/puppet/environments


[root@puppet]# puppet config print modulepath
/etc/puppetlabs/puppet/environments/production/modules:/etc/puppetlabs/puppet/modules:/opt/puppet/share/puppet/modules

[root@puppet production]# pwd
/etc/puppetlabs/puppet/environments/production

[root@puppet]# vi environment.conf # and add the modulepath line as follows...

[root@puppet production]# cat environment.conf
modulepath = site:modules:$basemodulepath

#
# Now let's look at the modulepath again...
#

[root@puppet]# puppet config print modulepath
/etc/puppetlabs/puppet/environments/production/site:/etc/puppetlabs/puppet/environments/production/modules:/etc/puppetlabs/puppet/modules:/opt/puppet/share/puppet/modules

#
# See how we now have an extra path element which will look in the production
# environment directory, and site sub-directory?
#

This will be useful later on because it allows us to look in multiple places for modules.

We could then configure things like this:

/etc/puppetlabs/puppet/environments/production/site    - holds modules NOT managed by R10K
/etc/puppetlabs/puppet/environments/production/modules - holds modules pulled down by R10K
/etc/puppetlabs/puppet/modules                         - holds modules common to all envs
/opt/puppet/share/puppet/modules                       - modules that ship with PE

This will allow us to keep role and profile modules in the control repo, without R10K
overwriting them


# A whole mess of links for further reading...

## General Puppet Learning Links
- [Puppet Lunch](http://puppetlunch.com/contents/)
- [Designing Puppet – Roles and Profiles](http://www.craigdunn.org/2012/05/239/)
- [Puppet Cookbook](http://www.puppetcookbook.com/)
- [Adrien Thebo Blog](http://somethingsinistral.net/blog/)
- [R.I. Pienaar Blog - Author of Hiera and MCollective](https://www.devco.net/)
- [The PuppetLabs Learning VM](https://puppet.com/download-learning-vm)
- [Example42 Blog](http://www.example42.com/blog/)

## RNELSON0 Blog on Puppet, Git, and related topics
- [Puppet for vSphere Admins - Table of Contents](http://rnelson0.com/puppet-for-vsphere-admins/)
- [Puppet and Git, 101: Git Basics](http://rnelson0.com/2014/05/05/puppet-and-git-101-git-basics/)
- [Puppet and Git, 102: Feature Branches](http://rnelson0.com/2014/05/12/puppet-and-git-102-feature-branches/)
- [Puppet and Git, 201: r10k Setup – Installation](http://rnelson0.com/2014/05/19/puppet-and-git-201-r10k-setup-installation/)
- [Puppet and Git, 202: r10k Setup – Conversion, Deployment](http://rnelson0.com/2014/05/26/puppet-and-git-202-r10k-setup-conversion-deployment/)
- [Puppet and Git, 203: r10k Workflow for New Module](http://rnelson0.com/2014/06/02/puppet-and-git-203-r10k-workflow-new-module/)
- [Puppet and Git, 204: r10k Workflow for Existing Module](http://rnelson0.com/2014/06/09/puppet-and-git-204-r10k-workflow-for-existing-module/)
- [Puppet and Git, 205: Git Hooks – Pre-Commit](https://rnelson0.com/2014/06/16/puppet-and-git-205-git-hooks/)
- [Puppet and Git, 206: Git Hooks – Post-Receive](https://rnelson0.com/2014/06/23/puppet-and-git-206-git-hooks-post-receive/)
- [Intro to Roles and Profiles with Puppet and Hiera](http://rnelson0.com/2014/07/14/intro-to-roles-and-profiles-with-puppet-and-hiera/)
- [Hiera, R10K, and the end of manifests as we know them](http://rnelson0.com/2014/07/21/hiera-r10k-and-the-end-of-manifests-as-we-know-them/)
- [Improved r10k deployment patterns](http://rnelson0.com/2015/04/15/improved-r10k-deployment-patterns/)
- [Troubleshooting Hiera from the CLI](https://rnelson0.com/2015/11/17/troubleshooting-hiera-from-the-cli/)
- [Configuring an R10k webhook on your Puppet Master](https://rnelson0.com/2015/05/03/configuring-an-r10k-webhook-on-your-puppet-master/)

## Gary Larizza Blog
- [Shit Gary Says Blog - Table of Contents](http://garylarizza.com/blog/categories/puppet/)
- [Fun With Puppet Providers - Part 1 of Whatever](http://garylarizza.com/blog/2013/11/25/fun-with-providers/)
- [Who Abstracted My Ruby?](http://garylarizza.com/blog/2013/11/26/fun-with-providers-part-2/)
- [Seriously, What Is This Provider Doing?](http://garylarizza.com/blog/2013/12/15/seriously-what-is-this-provider-doing/)
- [Building a Functional Puppet Workflow Part 1: Module Structure](http://garylarizza.com/blog/2014/02/17/puppet-workflow-part-1/)
- [Building a Functional Puppet Workflow Part 2: Roles and Profiles](http://garylarizza.com/blog/2014/02/17/puppet-workflow-part-2/)
- [Building a Functional Puppet Workflow Part 3: Dynamic Environments With R10k](http://garylarizza.com/blog/2014/02/18/puppet-workflow-part-3/)
- [Building a Functional Puppet Workflow Part 3b: More R10k Madness](http://garylarizza.com/blog/2014/03/07/puppet-workflow-part-3b/)
- [On R10k and 'Environments'](http://garylarizza.com/blog/2014/03/26/random-r10k-workflow-ideas/)
- [R10k + Directory Environments](http://garylarizza.com/blog/2014/08/31/r10k-plus-directory-environments/)
- [On Dependencies and Order](http://garylarizza.com/blog/2014/10/19/on-dependencies-and-order/)
- [Puppetconf 2014 Talk - the Refactor Dance](http://garylarizza.com/blog/2014/10/23/puppetconf-2014-talk/)
- [Puppet Workflows 4: Using Hiera in Anger](http://garylarizza.com/blog/2014/10/24/puppet-workflows-4-using-hiera-in-anger/)
- [Workflows Evolved: Even Besterer Practices](http://garylarizza.com/blog/2015/11/16/workflows-evolved-even-besterer-practices/)

## PuppetLabs Videos and Presentations
- [Killer R10K Workflow - Phil Zimmerman, Time Warner Cable](https://puppetlabs.com/presentations/killer-r10k-workflow-phil-zimmerman-time-warner-cable)
- [Git Workflow Best Practices & Deploying with r10k](https://puppetlabs.com/webinars/git-workflow-best-practices-deploying-r10k)
- [PuppetConf 2015 Videos and Presentations](https://puppetlabs.com/puppetconf-2015-videos-and-presentations) (Broken? Fix me.)
- [PuppetConf 2014 Videos and Presentations](https://puppetlabs.com/puppetconf-2014-videos-and-presentations) (Broken? Fix me.)
- [PuppetConf 2013 Videos and Presentations](https://puppetlabs.com/resources/puppetconf-2013) (Broken? Fix me.)
- [PuppetConf 2012 Videos on YouTube](https://www.youtube.com/playlist?list=PLV86BgbREluVFB73Wwqp_tCbw5Z9TMLX1)
- [Other PuppetLabs Playlists on YouTube](https://www.youtube.com/user/PuppetLabsInc/playlists)
- [Git Workflow and Puppet Environments](https://puppetlabs.com/blog/git-workflow-and-puppet-environments)
- [Git Workflows with Puppet and r10k](https://puppetlabs.com/blog/git-workflows-puppet-and-r10k)
- [Refactor Your Monolithic Code Repo To Deploy with r10k](https://puppetlabs.com/blog/refactor-your-monolithic-code-repo-to-deploy-with-r10k)
- [Puppet Enterprise 3.8 Upgrade Series ](https://puppetlabs.com/puppet/whats-new/puppet-enterprise-3.8-upgrade-series)

## General Git-Related Links

- [Try Git @ Code School](http://try.github.io/)
- [Git HowTo](https://githowto.com/) - Guided Tour of Git Usage
- [Git Real @ Code School](http://gitreal.codeschool.com/) - Interactive Git Tutorial
- [Git Home](https://git-scm.com/) - The Official Git Home Page
- [Git Downloads](https://git-scm.com/downloads) - Get the Git client
- [Git Tutorial](https://git-scm.com/docs/gittutorial) - Official Git Tutorial
- [Git Documentation](https://git-scm.com/docs) - The Official Git Documentation
- [Pro Git Book](https://git-scm.com/book/) - Read the *Pro Git* Book (Download or Read Online *FREE*)
- [Git Video Tutorials](https://git-scm.com/videos) - Some video tutorials
- [More Links](https://git-scm.com/doc/ext) - More links to Git Books, Tutorials, and Videos
- [Git Reference by GitHub Team](http://gitref.org/)
- [Git Immersion](http://gitimmersion.com/)
- [Ry’s Git Tutorial](http://rypress.com/tutorials/git/)
- [Intro to Git Video](https://youtu.be/ZDR433b0HJY) - Introduction to Git with Scott Chacon of GitHub
- [Visual Git Guide](http://marklodato.github.io/visual-git-guide/index-en.html)
- [Lars Vogel Git Tutorial](http://www.vogella.com/tutorials/Git/article.html)
- [Git Tower - Learn Git Tutorial](https://www.git-tower.com/learn/)
- [Git in the Trenches Book](http://cbx33.github.io/gitt/intro.html) - Read online or download PDF
- [GitHub's Cheatsheets](https://services.github.com/resources/)
- [GitHub's Training Kit](https://github.com/github/training-kit)
- [GitHub's Help](https://help.github.com/) - Has a ton of Git info, not just about GitHub
- [Git Magic Book](http://www-cs-students.stanford.edu/~blynn/gitmagic/)
- [Git Beginner's Guide for Dummies](http://backlogtool.com/git-guide/en/)
- [Atlassian Advanced Git Tutorials](https://www.atlassian.com/git/tutorials/advanced-overview)
- [The Git Guys Blog](http://www.gitguys.com/)
- [OpenSource.com - Part 1 - What is Git?](https://opensource.com/resources/what-is-git)
- [OpenSource.com - Part 2 - Getting Started with Git](https://opensource.com/life/16/7/stumbling-git)
- [OpenSource.com - Part 3 - Creating your first Git repo](https://opensource.com/life/16/7/creating-your-first-git-repository)
- [OpenSource.com - Part 4 - How to restore older file version in Git](https://opensource.com/life/16/7/how-restore-older-file-versions-git)
- [OpenSource.com - Part 5 - Graphical tools for Git](https://opensource.com/life/16/8/graphical-tools-git)
- [OpenSource.com - Part 6 - How to build your own Git server](https://opensource.com/life/16/8/how-construct-your-own-git-server-part-6)
- [OpenSource.com - Part 7 - How to manage binary blobs in Git](https://opensource.com/life/16/8/how-manage-binary-blobs-git-part-7)
- [OpenSource.com - Using Git in the classroom](https://opensource.com/education/16/1/git-education-classroom)
- [Git Large File Storage](https://git-lfs.github.com/)


# Vagrant-Related
- [Keep VirtualBox Guest Additions Up-to-Date](https://github.com/dotless-de/vagrant-vbguest)


---

<-- [Back to Contents](/README.md)

---

Copyright © 2016 by Mark Bentley






































