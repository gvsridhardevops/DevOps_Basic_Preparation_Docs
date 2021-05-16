Start Here
----------

This is a **Puppet Enterprise** and **Git** tutorial.
We will also use **Vagrant** and/or **Docker** to provision our training environment.

![Puppet](images/Puppet-Logo.jpg)
![Git](images/Git-Logo.png)

## Training Overview

What will we do in this tutorial?

* Deploy your own training environment using either Docker or Vagrant + VirtualBox
* Install Puppet Enterprise (Monolithic Install), GitLab, and one additional training VM/Container
* Learn the basics of Puppet Enterprise (Config, CLI, Environments, Code, etc.)
* Learn how to use Hiera for...
    - Node classification
    - Auto-lookup of class parameters
    - Hierarchical data lookup
* Learn how to install and Setup GitLab, and how to use it to host your Puppet code
* Learn how to setup R10K to automate code deployment
* Learn basic Puppet Coding, Git usage, and Vagrant & Docker usage


## How to use this repo?

* Download and install the Git client on your workstation

     - Link:  https://git-scm.com/downloads

* Clone this repo

```
     git clone https://github.com/bentlema/puppet-tutorial-pe
```

* Change directory to...

```
     cd puppet-tutorial-pe
```

* Begin following the Labs (links below)

## Navigating This Tutorial

This entire tutorial is of course a Git repository!  All of the tutorial
files are written in [Markdown](https://en.wikipedia.org/wiki/Markdown).
Make sure you're comfortable navigating this code repo before you start.
At any time, if you wish to return to this main README.md file, you can
click the bentlema/[puppet-tutorial-pe](/README.md) link at the top of the page.


## Minimum System Requirements

* You will need the ability to install software on your workstation (Admin / Super-User privileges).  We will use the following software:
    - Git
    - VirtualBox
    - Vagrant or Docker

* The software we will use is available for Windows, Linux, and Mac OS X, so you should be able to use any platform.

* Disk space:
    - 20GB of free disk space to acomodate software and VM/Container images

* Memory:
    - minimum 8GB of RAM if using Docker Containers
    - minimum 12GB of RAM if using VirtualBox VMs

* Note: You will use either Vagrant to spin up VM's or Docker to spin up Containers
    - We will not use both (at the same time), so choose one or the other
    - Although Vagrant is capable of spinning up Docker containers, we will
      not use this capability

## Software Versions

As there can be compatibility issues between different versions of the software we will use (e.g. Vagrant and Virtualbox), I would recommend sticking to the versions that have been tested to work together nicely.

| Docker | Vagrant | VirtualBox | Platform                  |
| ------ | ------- | ---------- | ------------------------- |
| 1.13.0 | 1.8.4   | 5.0.28     | Mac OS X 10.11 El Capitan |
|        |         |            | Mac OS X 10.10 Yosemite   |
|        |         |            | Mac OS X 10.9 Mavericks   |
|        |         |            | Linux                     |
|        |         |            | Windows                   |

All of my initial testing has been done using Mac OS X 10.11 El Capitan, Vagrant 1.8.4, and VirtualBox 4.0.28.
Feel free to test other version/platform combinations, and contribute your results here.

---

## Labs

There are enough differences between using Docker and Vagrant+VirtualBox,
I've split this tutorial into two distinct tracks.  I will cover all of the
same core materials, but will use different examples in each track, so it
may be worth it to go through both tracks if you have the time and energy.



---

### TRACK: Vagrant + VirtualBox
![Vagrant Logo](images/Vagrant-Logo.png) ![VirtualBox Logo](images/VirtualBox-Logo.png)

 * **Lab 01** - [Vagrant to deploy 3 training VMs](/tutorial/vbox/01-Provision-Training-VMs.md#lab-1)
 * **Lab 02** - [Prepare to Install Puppet Enterprise on VMs](/tutorial/vbox/02-Prep-to-Install-Puppet-Master.md#lab-2)
 * **Lab 03** - [Install Puppet Master](/tutorial/vbox/03-Install-Puppet-Master.md)
 * **Lab 04** - [Install Puppet Agent](/tutorial/vbox/04-Install-Puppet-Agent.md)
 * **Lab 05** - [Get familiar with puppet config files, and puppet code, and CLI](/tutorial/vbox/05-Puppet-Config-and-Code.md)
 * **Lab 06** - [Practice doing some puppet code, and puppet runs](/tutorial/vbox/06-Puppet-Code-Practice.md)
 * **Lab 07** - [Configure Hiera](/tutorial/vbox/07-Config-Hiera.md)
 * **Lab 08** - [More about Environments](/tutorial/vbox/08-Environments.md)
 * **Lab 09** - [Install GitLab on the gitlab VM](/tutorial/vbox/09-Install-GitLab.md)
 * **Lab 10** - [Move Puppet Code under Git Control](/tutorial/vbox/10-Move-Puppet-Code-to-GitLab.md)
 * **Lab 11** - [Modules, Roles & Profiles, and the environment.conf](/tutorial/vbox/11-Roles-and-Profiles.md)
 * **Lab 12** - [Git Basics](/tutorial/vbox/12-Git-Basics.md)
 * **Lab 13** - [Git Workflow](/tutorial/vbox/13-Git-Workflow.md)
 * **Lab 14** - [Practice doing some puppet code, and puppet runs](/tutorial/vbox/14-practice.md)


---

### TRACK: Docker Containers
![Docker Logo](images/Docker-Logo.png)

 * **Lab 01** - [Docker to deploy 3 training Containers](/tutorial/docker/01-Provision-Training-Containers.md#lab-1)
 * **Lab 02** - [Prepare to Install Puppet Enterprise on Containers](/tutorial/docker/02-Prep-to-Install-Puppet-Master.md#lab-2)
 * **Lab 03** - [Install Puppet Master](/tutorial/docker/03-Install-Puppet-Master.md)
 * **Lab 04** - [Install Puppet Agent](/tutorial/docker/04-Install-Puppet-Agent.md)
 * Now you may continue following the remaining Labs in the Vagrant track, starting with Lab #5.  Good luck.

---

## Further Reading

 - [Lots of links to external resources](/tutorial/YY-Further-Reading.md)

---

=================================================================================================================================================

<-- [Back](/README.md#labs)

---

### **Lab #1** - Install software, and use Docker to deploy 3 training Containers

---

### Overview ###

Time to complete:  30 minutes

The following software should already be installed:

* **Git** - the version control system

In this lab you will install the following software:

* **Docker** - the container deployment tool

...and then create 3 containers named as follows:

1. **puppet**   (Puppet Master, PE Console, etc.)
3. **agent**    (A Puppet agent)
2. **gitlab**   (the Git Hosting Software)

### Download Software ###

* Download the needed software for this training...

From the top-level of the cloned repo, you'll find a script which will
download the necessary software...

```
     [puppet-tutorial-pe]$ cd share
     [puppet-tutorial-pe/share]$ cd software
     [puppet-tutorial-pe/share/software]$ ./download-all.sh
```

### Installing Docker ###

After running the **download-all.sh** Find the appropriate installer for
Mac or Windows in the **share/software/docker** folder.

```
     [puppet-tutorial-pe/share/software]$ cd docker
     [puppet-tutorial-pe/share/software/docker]$ ls -l

     total 426328
     -rw-r--r--  1 bentlema  staff  115214630 Aug 20 16:08 Docker.dmg
     -rw-r--r--  1 bentlema  staff  103059456 Aug 20 16:09 InstallDocker.msi
     -rwxr-xr-x  1 bentlema  staff        305 Aug 19 12:07 download-docker.sh

```

Open up a new Finder window (on Mac) or an Explorer window (on Windows) and navigate to the **share/software/docker** directory, and...

* On Mac OS X, double-click the **Docker.dmg**, and then double-click the installer
* On Windows double-click the **InstallDocker.msi** file to launch the installer
* If you're running Linux, Docker runs natively, so you just need to install the **docker-engine** package.
  See:  <https://docs.docker.com/engine/installation/linux/>

### Set Maximum Memory Usage

Docker utilizes a VM to run docker images in, and it needs to be told the maximum memory and virtual CPUs to use.
Open up the Docker Settings / Preferences, and under the Advanced Tab, change the number of vCPUs and Memory
to 4 CPUs and 8.0GB Memory.  If you dont do this, your Docker containers will begin to swap and appear to run
very slowly.

![Docker Preferences](images/Docker-Prefs.png)


### Some Docker Basics ###

Here are some of the Docker commands you will be using.

```
     docker help     # see help page
     docker ps       # see running containers
     docker ps -a    # See all containers
     docker images   # show local docker images
     docker rmi      # remove an image
     docker run      # create and run a container
     docker exec     # run a command within an already running container
     docker stop     # stop a container
     docker rm       # remove a stopped container
     docker network  # create/destroy/list Docker networks
```

We will learn about these as we go along...

### Creating A Private Docker Network ###

Create a Docker Network.  It will be used by the 3 containers we create to communicate with eachother.

```
     docker network create --subnet=192.168.198.0/24 example.com
```

Note:  We name our network using what looks like a domain name because Docker
will use the network name in the **Fully-Qualified Domain Name** (FQDN) of the
container.  Docker's internal DNS server will take the short hostname and
append the network name when doing reverse-lookups (PTR records) so we do
this little hack to make sure the forward and reverse lookups match.  With
this said, I've seen inconsistant behavior with Docker's internal DNS, and
sometimes reverse lookups just dont work at all.  No matter, we push on!


### Setup environment variable with your BASEDIR ###

I don't know where you've cloned this puppet-tutorial-pe repo to on your
workstation.  So, let's make our lives simpler, and set an environment variable
to contain the absolute path to your working tree (top level of the repo)

Make sure your current working directory is the top level of the puppet-tutorial-pe
repo, and then just type this:

If using Windows:

   * Need to put instructions here for Windows users (but I dont own a Windows system to test on)
   * For now, if using Windows, you'll just have to type out the full absolute path where you see **${BASEDIR}** referenced below.

If using a bash shell (Linux or Mac OS X):

```
     export BASEDIR=$(pwd)
```

Then validate that BASEDIR is set to what you want:

```
     echo $BASEDIR
```

And you should see something like this:

```
     $ pwd
     /Users/bentlema/Puppet-Training/puppet-tutorial-pe

     $ export BASEDIR=$(pwd)

     $ echo $BASEDIR
     /Users/bentlema/Puppet-Training/puppet-tutorial-pe

```

Now, as you'll see in the following sections, we will use **${BASEDIR}** in the docker
command when specifying the absolute path to our volumes.  If you dont specify the
absolute path, the volume mapping will not work as expected.  So, rather than having
to type out the entire path each time, we save it in BASEDIR.


### Create and run your Puppet Master Container ###

To start up the container for the Puppet Master:

```
   docker run -d                         \
      --memory 4G                        \
      --net example.com                  \
      --ip 192.168.198.10                \
      -p 22022:22                        \
      -p 22443:443                       \
      -p 22080:8080                      \
      -p 22081:8081                      \
      -p 22140:8140                      \
      -p 22000:3000                      \
      --name puppet                      \
      --hostname puppet.example.com      \
      --dns-search=example.com           \
      --network-alias=puppet             \
      --network-alias=puppet.example.com \
      --volume ${BASEDIR}/share:/share   \
      bentlema/centos6-puppet-nocm       \
      /sbin/init
```

You will see someoutput like this:

```
Unable to find image 'bentlema/centos6-puppet-nocm:latest' locally
latest: Pulling from bentlema/centos6-puppet-nocm
b6d7b2ebc0a7: Pull complete
f8d0a70b3a37: Pull complete
3a45149aa462: Pull complete
ed8f8a2946b2: Pull complete
14dd1ef04b2a: Pull complete
7c6783c108f4: Pull complete
7d3711267225: Pull complete
c4e780c655e7: Pull complete
Digest: sha256:321205a84c4340f0d24b56cb8def669ac5e769704d8d26921ba9af039c290833
Status: Downloaded newer image for bentlema/centos6-puppet-nocm:latest
1e728f78568bf155963a222f4aee9524c82d32a524449908158d3b924e687d44
```

You've just started up your **puppet** container, and left it running in the background.

To see your running containers do:

```
     docker ps
```

### Login to the puppet container ###

```
     docker exec -it puppet /bin/bash
```

*OR*

```
     ssh -l root localhost -p 22022
```

The default password is:  *foobar23*


### Create and run your agent container ###

```
   docker run -d                        \
      --memory 512M                     \
      --net example.com                 \
      --ip 192.168.198.11               \
      -p 23022:22                       \
      --name agent                      \
      --hostname agent.example.com      \
      --dns-search=example.com          \
      --network-alias=agent             \
      --network-alias=agent.example.com \
      --volume ${BASEDIR}/share:/share  \
      bentlema/centos6-puppet-nocm      \
      /sbin/init
```

You've just started up your **agent** container, and left it running in the background.

To see your running containers do:

```
     docker ps
```

### Login to the agent container ###

```
     docker exec -it agent /bin/bash
```

*OR*

```
     ssh -l root localhost -p 23022
```

The default password is:  *foobar23*

### Create and run GitLab container ###

Notice that the container image we're using is from *gitlab* itself.  That's
the nice thing about Docker.  There are many pre-built container images
that we can just use, and instantly have a nice piece of software up and
running without any fuss.


```
   docker run --detach                   \
      --net example.com                  \
      --ip 192.168.198.12                \
      --publish 24022:22                 \
      --publish 24080:80                 \
      --publish 24443:443                \
      --name gitlab                      \
      --hostname gitlab.example.com      \
      --dns-search example.com           \
      --network-alias gitlab             \
      --network-alias gitlab.example.com \
      --env GITLAB_DATABASE_POOL="3;"    \
      --env GITLAB_OMNIBUS_CONFIG="gitlab_rails['db_pool'] = 3" \
      --volume "${BASEDIR}/gitlab/config:/etc/gitlab"   \
      --volume "${BASEDIR}/gitlab/logs:/var/log/gitlab" \
      --volume "${BASEDIR}/gitlab/data:/var/opt/gitlab" \
      gitlab/gitlab-ce:latest

```

Give GitLab a minute or two to configure itself, and then try connecting to the web GUI:

- GitLab Server: <http://127.0.0.1:24080/>

You will be prompted to set a new password, so go ahead and do that, and then login:

- Username: ***root***
- Password: ***\<the password you just set\>***

That's all we're going to do with GitLab for now.  We'll come back to it in a later lab...

---

At this point, all 3 of your Docker containers should be up and running.  Woot!

---

### Test network connectivity and name resolution between your containers ###

Before we continue, let's make sure we can communicate over the internal
Docker network we've created to/from each container.

Connect to your puppet and agent containers in 2 different terminal windows:

- `docker exec -it puppet /bin/bash`
- `docker exec -it agent /bin/bash`

From each, test that you can ping the other by name:

From the **puppet** container, ping the **agent** container using the short name:

```
     ping agent
```

You should see something like this:

```
     [root@puppet /]# ping agent
     PING agent (192.168.198.11) 56(84) bytes of data.
     64 bytes from agent.example.com (192.168.198.11): icmp_seq=1 ttl=64 time=0.140 ms
     64 bytes from agent.example.com (192.168.198.11): icmp_seq=2 ttl=64 time=0.118 ms
     64 bytes from agent.example.com (192.168.198.11): icmp_seq=3 ttl=64 time=0.093 ms
     64 bytes from agent.example.com (192.168.198.11): icmp_seq=4 ttl=64 time=0.117 ms
     ^C
     --- agent ping statistics ---
     4 packets transmitted, 4 received, 0% packet loss, time 3352ms
     rtt min/avg/max/mdev = 0.093/0.117/0.140/0.016 ms
```
Press **Control-C** to abort the continuous ping.

From the **agent** container, ping the **puppet** container using the short name:

```
     ping puppet
```

You should see something like this:

```
     [root@agent /]# ping puppet
     PING puppet (192.168.198.10) 56(84) bytes of data.
     64 bytes from puppet.example.com (192.168.198.10): icmp_seq=1 ttl=64 time=0.103 ms
     64 bytes from puppet.example.com (192.168.198.10): icmp_seq=2 ttl=64 time=0.137 ms
     64 bytes from puppet.example.com (192.168.198.10): icmp_seq=3 ttl=64 time=0.195 ms
     64 bytes from puppet.example.com (192.168.198.10): icmp_seq=4 ttl=64 time=0.114 ms
     ^C
     --- puppet ping statistics ---
     4 packets transmitted, 4 received, 0% packet loss, time 3276ms
     rtt min/avg/max/mdev = 0.103/0.137/0.195/0.036 ms
```
Press **Control-C** to abort the continuous ping.

Notice that when the ping command reports the hostname of the thing it's pinging, it shows the FQDN.
How did that happen?

The containers are configured to use Docker's internal DNS server, and it configures itself with
both **A**-records (forward) and **PTR**-records (reverse) for all containers that have been created.

Notice that the **/etc/resolv.conf** is configured with the following:

```
     [root@puppet /]# cat /etc/resolv.conf
     search example.com
     nameserver 127.0.0.11
     options ndots:0

```

This tells the resolver library to use **127.0.0.11** as the nameserver, which is Docker's internal DNS server.

Try doing some forward lookups to get the **A**-records:

```
     [root@puppet /]# dig puppet.example.com +short
     192.168.198.10

     [root@puppet /]# dig agent.example.com +short
     192.168.198.11

     [root@puppet /]# dig gitlab.example.com +short
     192.168.198.12
```

Try doing some reverse lookups to get the **PTR**-records:

```
     [root@puppet /]# dig -x 192.168.198.10 +short
     puppet.example.com.

     [root@puppet /]# dig -x 192.168.198.11 +short
     agent.example.com.

     [root@puppet /]# dig -x 192.168.198.12 +short
     gitlab.example.com.
```

If you get the same responses, you can be assured that DNS is working properly.

Note:  Testing these DNS lookups from a single container is sufficient, as
we're just querying the internal DNS server.  If it works from one container,
it will work the same from the others.

---

Okay, our training environment is all setup, and we're ready to start installing software...

---

Continue on to **Lab #2** --> [Prepare to Install Puppet Enterprise](02-Prep-to-Install-Puppet-Master.md#lab-2)

---

===============================================================================================================


<-- [Back](01-Provision-Training-Containers.md#labs)

---

# **Lab #2** - Prepare to Install Puppet Enterprise on the **puppet** Container

---

### Overview ###

Time to complete:  5 minutes

In this lab we will prepare to install Puppet Enterprise 2016.5.1

* Make sure your containers started / running
* Get connected to your **puppet** container

### Startup your Training Containers ###

If you've just landed here after doing Lab #1, your containers should already be
up and running and ready to go.

If you're coming back to this tutorial after some downtime, make sure your Docker Containers
are up and running.

To see running containers:

```
   docker ps
```

To see all containers (even those that are stopped):

```
   docker ps -a
```

Make sure your 3 training containers are up and running, and if not, start them:

```
   docker start puppet
   docker start agent
   docker start gitlab
```

Remember that you can connect to your VM's with the exec command of /bin/bash like this:

```
   docker exec -it puppet /bin/bash
```

So do that!  Get connected to your puppet container, and then proceed to the
next lab where we will do the actual install of Puppet Enterprise.

---

Note:  The **puppet** and **agent** containers have been configured from a centos6
image, and sshd has been configured to allow root to login.  The GitLab image,
however, doesn't allow root login with PasswordAuth, so you will need to use
an exec to get into the container if you want to look around.  There's really no
need to use ssh to connect as we work through this tutorial, but I've configured it
just to show that it's possible.

---

### If something goes wrong...

If something goes wrong, and you want to try starting over, you may simply
stop and delete (remove) your container(s) using:

* docker stop ***container-name***
* docker rm ***container-name***

You can use the container name or hash ID to identify the container you want to stop/rm.
After removing a container, you can re-run the **docker run** command to fire up the 
container fresh.


---

Continue to **Lab #3** --> [Install Puppet Master](03-Install-Puppet-Master.md#lab-3)

---

=================================================================================================================

<-- [Back](02-Prep-to-Install-Puppet-Master.md#lab-2)

---

# **Lab #3** - Install Puppet Enterprise

---

### Overview ###

Time to complete:  30 minutes

In this lab we will install Puppet Enterprise 2016.5.1

* PE is free to install and evaluate
* When running PE without a license, you're limited to 10 agents

---

### Get logged in to your puppet Container ###

We are about to install Puppet Enterprise, so make sure you're
logged into our puppet master container.

### Note about root ###

If you're using Docker, and getting in to your container with an exec of bash,
you will already be the root user.  That's what we want...

### Run The Installer ###

Now let's un-compress/de-archive the PE installation tarball, and install...

Change into the directory with the PE software:

```shell
     cd /share/software/puppet
```
Note:  There are two different installation tarballs.  One for EL6 and one for EL7.

* If following the Docker track, we will use the EL6 tarball

```
     tar xzvf puppet-enterprise-2016.5.1-el-6-x86_64.tar.gz
     cd puppet-enterprise-2016.5.1-el-6-x86_64
```

Then run the installer:

```
     ./puppet-enterprise-installer
```

The installer will prompt you:

```
      How to proceed? [1]:
```

Press Enter to accept the default and then you'll see:

```
     Installing setup packages.
     Please go to https://puppet.example.com:3000 in your browser to continue installation.
     Be sure to use https:// and that port 3000 is reachable through the firewall.
```

Remember that we have **forwarded port 3000 to 22000** on our workstation, so...

* Use web browser to connect to: **<https://127.0.0.1:22000/>**
* Click **Monolithic Install**
* For FQDN, enter **puppet.example.com**
* For DNS alias, enter **puppet**
* Select: Install PostgreSQL on the PuppetDB host for me.
* Click **Submit**
* Click **Continue**

    Note:  Don't worry about the memory and disk space warnings.  The amount of memory and disk space we've provisioned will be just fine for this training exercise.

You will see this:

```
     We're checking to make sure the installation will work correctly
     Verify that 127.0.0.1 can resolve puppet.
     Verify root access on puppet.
     Verify that DNS is properly configured for puppet.
     Verify that your hardware meets requirements on puppet.
     Verify that 127.0.0.1 has a PE installer that matches puppet's OS.
     Verify that '/opt' and '/var' contain enough free space on puppet.
     [puppet] Insufficient space in '/opt' (16 GB); we recommend at least 100 GB for a production environment.
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
VM/Container to our localhost.  Use the link below instead.

---

### Login to the PE Console ###

We've forwarded port 443 from our puppet container to port 22443 on our hosting workstation, so you should be able to connect to the PE Console via the URL:

* PE Console URL:  **<https://127.0.0.1:22443/>**

Login as **admin** and enter the admin password you chose during the install.

If you forgot (or not sure what you typed) you can find the password in the answers file:

```
     grep console_admin_password /opt/puppetlabs/puppet/share/installer/conf.d/puppet.example.com.conf
```

Probabbly a good idea to **change it**...!

---

Believe it or not, that's all there is to installing a 'Monolithic' puppet
server.

Look around the PE console.  You should see 1 agent is registered called **puppet.example.com**.  This is your Puppet Master!

To change the 'admin' account password, click on 'admin' in the top right corner, and select 'My Account'.  You should
find a 'Reset password' link near the top/right of that page.

Test your PE install from the shell prompt by running the agent manually like this:

```
     puppet agent -t
```

Note:  Even on the Puppet Master, the Agent runs regularly.  The Master configures itself through Puppet.  Be aware, if you make
a puppet change that affects all nodes, you will be affecting the master config as well (e.g. a global change to /etc/hosts).
Do not disable the puppet agent on the master thinking that you're guarding against accidental changes that could break your
puppet infrastructure.  The agent runs on the master are required for mcollective key distribution (they get stored in the PuppetDB
and then installed on the other nodes of puppet infra via exported resources).  Also, there are certain configuration params
in the puppet.conf on the master that are managed by puppet itself.


```shell
     [root@puppet ~]# puppet agent -t
     Info: Retrieving pluginfacts
     Info: Retrieving plugin
     Info: Loading facts
     Info: Caching catalog for puppet
     Info: Applying configuration version '1452623722'
     Notice: Finished catalog run in 5.54 seconds
```

Not too exciting.  We have confirmed that the agent run succeeds, so we know the puppetmaster is up and running and able
to build a catalog for itself. Good.

If you're on a terminal that supports ANSI color, you'll notice that the text
is in **GREEN**.  If there are any puppet errors during a puppet run, those errors
would show up in **RED** text.

---

Continue to **Lab #4** --> [Install Puppet Agent on agent node, and do test puppet run](04-Install-Puppet-Agent.md#lab-4)

---

### Further Reading ###

These links are not needed for this Lab, but for reference here's the PE Install Guide at the PuppetLabs web site:

Quick Start Guide:  <https://docs.puppetlabs.com/pe/2016.5/quick_start_install_mono.html>

Detailed Install Guide:  <https://docs.puppetlabs.com/pe/2016.5/install_basic.html>

Split Install:   <https://docs.puppetlabs.com/pe/2016.5/install_pe_split.html>

LEI Install:   <https://docs.puppetlabs.com/pe/2016.5/install_multimaster.html>

---

<-- [Back to Contents](/README.md)

---

===========================================================================================================================================

<-- [Back](03-Install-Puppet-Master.md#lab-3)

---

### **Lab #4:** Install the Puppet Agent

---

### Overview ###

Time to complete:  10 minutes

In this lab we will:
-  install the Puppet Agent on the **agent** container

### Pre-installation Steps ###

Make sure your **agent** container is started, and get logged in.

```
     docker start agent
     docker exec -it agent /bin/bash
```

### Install the Agent ###

To install the puppet agent on the **agent** node, we can take advantage of
a feature of the PE Master:  The PE Master makes available the agent installer
behind its own web server.  You can use **wget** or **curl** to download the
installer script and then pipe it through bash.

* To use **wget**

```
     wget --no-check-certificate --secure-protocol=TLSv1 -O - https://puppet:8140/packages/current/install.bash | bash
```

* To use **curl**

```
     curl -k --tlsv1 https://puppet:8140/packages/current/install.bash | bash
```

If you'd like to browse what else is accessible via that web server, try
opening <https://localhost:22140/packages> in your workstation's web browser.

(Remember we port-forwarded 8140 to 22140 on our hosting workstation)

Go ahead an install the agent if you haven't already done so, and then
try running the puppet agent...

### Run the Puppet Agent ###

Run the puppet agent manually.  This will cause an SSL certificate request
to be generated and sent to the puppetmaster.

```
     [root@agent ~]# puppet agent -t
     Info: Creating a new SSL key for agent.example.com
     Info: Caching certificate for ca
     Info: Caching certificate_request for agent.example.com
     Info: Caching certificate for ca
     Exiting; no certificate found and waitforcert is disabled
```

### Sign the Certificate ###

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

### Run the Puppet Agent Again ###

Run puppet a second time, and you should get a clean run with no changes.

```
     puppet agent -t
```

![Puppet Agent Clean Run](images/Puppet-Agent-Clean-Run.png)

For brevity, I've not included the output on this page, but it's available for viewing
here: 

* Puppet Run Output:  [04-Puppet-Agent-Run-Output.md](04-Puppet-Agent-Run-Output.md)

---

At this point we have 3 running containers, but only 2 running the puppet agent:

- a **Puppet Master node** (hostname **puppet.example.com**) that also runs an agent to configure itself
- a **Puppet Agent node** (hostnamne **agent.example.com**) that runs an agent, and where we will test code and learn more about PE
- a **GitLab server** that we haven't used yet, but will in a later lab...

If you login to the [PE Console](https://127.0.0.1:22443/nodes), you should see these two agents on the 'Nodes' page.
We will not install the puppet agent on the GitLab container at this time, as it is running in an Ubuntu-based container,
and our Puppet Master is running un a CentOS 6 container, and only has the centos packages available out-of-the-box.
We can update the Puppet Master to download packages for other operating systems though.  Since the GitLab container
is based on an Ubuntu 16.04 image, we can add the following class to our PE Master via the PE Console:

```
     pe_repo::platform::ubuntu_1604_amd64
```

1. Navigate to:  Nodes --> Classification --> All Nodes --> PE Infrastructure --> PE Master
2. Click **Classes** Tab
3. Add new class:  ***pe_repo::platform::ubuntu_1604_amd64*** and click **"Add Class"**
4. Click **"Commit 1 change"** at the bottom right of the page
5. Run puppet on the Puppet Master with:   `puppet agent -t`

When Puppet runs, it will download the installation packages for Ubuntu, and then you should
be able to install the Puppet Agent on your GitLab container as well.  However!

We're using Docker in a way it's not really intended to be used.  A docker container does
not necessarily contain a full operating systems release.   In fact, it would be rare to.
Docker container images are built to contain only the minimum packages to run the application.

In the case of the GitLab container image, it doesn't come with **systemd**, which is 
assumed to be there by Puppet.  Puppet wont be able to manage servies, without **systemd**
installed in the container.  Oh well...

---

<-- [Back to Contents](/README.md)

---


Here's the output from the inital puppet run on the **agent** VM...

```
[root@agent ~]# puppet agent -t
Info: Caching certificate for agent.example.com
Info: Caching certificate_revocation_list for ca
Info: Caching certificate for agent.example.com
Info: Retrieving pluginfacts
Info: Retrieving plugin
Notice: /File[/var/opt/lib/pe-puppet/lib/facter]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/facter/custom_auth_conf.rb]/ensure: defined content as '{md5}45f759978989686d9820efb73a0d277c'
Notice: /File[/var/opt/lib/pe-puppet/lib/facter/pe_build.rb]/ensure: defined content as '{md5}f2a752162694029797947d0f88a50def'
Notice: /File[/var/opt/lib/pe-puppet/lib/facter/pe_concat_basedir.rb]/ensure: defined content as '{md5}0ccd3500f29b9dd346a45a61268c7c18'
Notice: /File[/var/opt/lib/pe-puppet/lib/facter/pe_version.rb]/ensure: defined content as '{md5}4a9353952963b011759f3e6652a10da5'
Notice: /File[/var/opt/lib/pe-puppet/lib/facter/platform_symlink_writable.rb]/ensure: defined content as '{md5}1642c4dde30573c1929305f9bfb349fa'
Notice: /File[/var/opt/lib/pe-puppet/lib/facter/platform_tag.rb]/ensure: defined content as '{md5}ba0af12f6068589e99afce76072d8bf6'
Notice: /File[/var/opt/lib/pe-puppet/lib/facter/staging_http_get.rb]/ensure: defined content as '{md5}2c27beb47923ce3acda673703f395e68'
Notice: /File[/var/opt/lib/pe-puppet/lib/facter/windows.rb]/ensure: defined content as '{md5}d8880f6f32905f040f3355e2a40cf088'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/indirector]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/indirector/node]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/indirector/node/console.rb]/ensure: defined content as '{md5}40a74c14f2748b93da7339fa011cc110'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/cookie_secret_key.rb]/ensure: defined content as '{md5}a1a48191d1f0cb934b0c63d8fec70566'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/create_java_args_subsettings_hash.rb]/ensure: defined content as '{md5}b54be02c9f0b0eeee699764df57a2db3'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_bool2str.rb]/ensure: defined content as '{md5}f6189451331df6fd24ec69d7cdc76abe'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_chomp.rb]/ensure: defined content as '{md5}b4f0cb35578710dc4ac315d35e9571a2'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_concat_getparam.rb]/ensure: defined content as '{md5}46df3de760f918b120fb2254f85eff2a'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_concat_is_bool.rb]/ensure: defined content as '{md5}b511d7545ede5abae00951199b67674d'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_create_amq_augeas_command.rb]/ensure: defined content as '{md5}a62e6f52c8a5bdc002436dc6c292fd48'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_delete_undef_values.rb]/ensure: defined content as '{md5}c25bbcdfc6bca2d219e5f42f3eb8fa0b'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_empty.rb]/ensure: defined content as '{md5}01a6574fab1ed1cf94ef1fea4954eeca'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_flatten.rb]/ensure: defined content as '{md5}c781954451d1860ca8f63fd6d2b6cf76'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_getvar.rb]/ensure: defined content as '{md5}f870b47cd38f515662c29555a7c6e91f'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_is_array.rb]/ensure: defined content as '{md5}b451e133e015fc7e8dc4dcfdf059a8d8'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_is_bool.rb]/ensure: defined content as '{md5}2dfe2be70aaff951b59e9fba3e85aa5d'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_is_integer.rb]/ensure: defined content as '{md5}a410ba3f3586b7df90e532b2eb99da37'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_is_string.rb]/ensure: defined content as '{md5}5fe6741e70f2bbb93a0ae43d233eeebc'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_join.rb]/ensure: defined content as '{md5}4f433ea29dffc79247671fb4271d0a10'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_join_keys_to_values.rb]/ensure: defined content as '{md5}3066c3bd5e181a996729691a57cf3d21'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_loadyaml.rb]/ensure: defined content as '{md5}6adef0c167fbe0167a8e7aec7c65317b'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_max.rb]/ensure: defined content as '{md5}e04acd17070545133c83ec5a0e11c022'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_min.rb]/ensure: defined content as '{md5}e6d2b8c614168f4224e3f76f32d9f9cb'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_pick.rb]/ensure: defined content as '{md5}06b3a9e63faf3ca5d64c65fa14803cdf'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_postgresql_acls_to_resources_hash.rb]/ensure: defined content as '{md5}851d972daf92e9e0600f8991a15311be'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_postgresql_escape.rb]/ensure: defined content as '{md5}cc58b659957328d9577336353bc246b2'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_postgresql_password.rb]/ensure: defined content as '{md5}72c33c3b7e4a6e8128fbb0a52bf30282'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_prefix.rb]/ensure: defined content as '{md5}554fcaf9362a544f91bb192047dd5341'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_strip.rb]/ensure: defined content as '{md5}8d60a607f04fc6622eca8ae46e2fef2f'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_suffix.rb]/ensure: defined content as '{md5}a44749c5ef30e258866cb18fd83f77d2'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_to_bytes.rb]/ensure: defined content as '{md5}6ee36cabe336db4c281c7d3b1b1d771e'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_union.rb]/ensure: defined content as '{md5}da3ea966f5468bbdb8420975576d4a3f'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_unique.rb]/ensure: defined content as '{md5}5edb2c537d80f003d71a250bf203c79e'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_upcase.rb]/ensure: defined content as '{md5}4ea67e96c4da45092fb70fcdf1f0692f'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_validate_absolute_path.rb]/ensure: defined content as '{md5}d4bd539a3d7db93d4563cbbe571a16ac'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_validate_array.rb]/ensure: defined content as '{md5}0ab10b81de351aa9f6114c1880cc7155'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_validate_bool.rb]/ensure: defined content as '{md5}4a74954e0502837f11d2eadedb71bc1f'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_validate_hash.rb]/ensure: defined content as '{md5}36a223b1648dec8cb30fd229e3bb74c6'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_validate_re.rb]/ensure: defined content as '{md5}bccac35a2607bf15f1e7d1c565c1d98b'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_validate_single_integer.rb]/ensure: defined content as '{md5}ef8c455e5d58954bc4e78e36534a340c'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/pe_validate_string.rb]/ensure: defined content as '{md5}c4d83c3ef14c2e3e47c4408d49c22437'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/scope_defaults.rb]/ensure: defined content as '{md5}da916d46f3ff3be8359f75c93c2b5532'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/parser/functions/staging_parse.rb]/ensure: defined content as '{md5}605c4de803c65f2c3613653b68921002'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_file_line]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_file_line/ruby.rb]/ensure: defined content as '{md5}79d77c28f8a311684aceec3e08c1a084'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_ini_setting]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_ini_setting/ruby.rb]/ensure: defined content as '{md5}d0520f108a6f0e55320a97f8285a0843'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_ini_subsetting]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_ini_subsetting/ruby.rb]/ensure: defined content as '{md5}7245892fe493f361b4f2fb34188e71db'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_java_ks]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_java_ks/keytool.rb]/ensure: defined content as '{md5}6e16c71a9e74550cfe9aad4ecbb8fd22'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_postgresql_conf]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_postgresql_conf/parsed.rb]/ensure: defined content as '{md5}f0e7fc6f14420d46ebf64635939243af'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_postgresql_psql]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/provider/pe_postgresql_psql/ruby.rb]/ensure: defined content as '{md5}3f7e99833784bdaf4c56c9d8621aa14c'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/reports]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/reports/console.rb]/ensure: defined content as '{md5}4a1a445c9315e0dff869533dca8b6840'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/type]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/type/pe_anchor.rb]/ensure: defined content as '{md5}5505f2e5850c0dd2e56583d214baf197'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/type/pe_file_line.rb]/ensure: defined content as '{md5}5cecf4e63d31bc89f31a9be54a248359'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/type/pe_ini_setting.rb]/ensure: defined content as '{md5}51ff3999c8dfd3a32303c14deb279dc4'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/type/pe_ini_subsetting.rb]/ensure: defined content as '{md5}022dc2b30ed8daa8ce2226017bc95a38'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/type/pe_java_ks.rb]/ensure: defined content as '{md5}5fa3ea1d3972859574bbdcbd1049cb3d'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/type/pe_postgresql_conf.rb]/ensure: defined content as '{md5}7525790ec89f646dd377655a7e87e6eb'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/type/pe_postgresql_psql.rb]/ensure: defined content as '{md5}ffabdb11eb481e45c76795195672436c'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/util]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/util/external_iterator.rb]/ensure: defined content as '{md5}69ad1eb930ca6d8d6b6faea343b4a22e'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/util/pe_ini_file]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/util/pe_ini_file.rb]/ensure: defined content as '{md5}9ba01a79162a1d69ab8e90e725d07d3a'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/util/pe_ini_file/section.rb]/ensure: defined content as '{md5}652d2b45e5defc13fb7989f020e6080f'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppet/util/setting_value.rb]/ensure: defined content as '{md5}a649418f4c767d976f4bf13985575b3c'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppetpe]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppetpe/puppetlabs]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppetpe/puppetlabs/pe_console]/ensure: created
Notice: /File[/var/opt/lib/pe-puppet/lib/puppetpe/puppetlabs/pe_console/config.rb]/ensure: defined content as '{md5}5ef248d7814aa1df12cb44db77d11771'
Notice: /File[/var/opt/lib/pe-puppet/lib/puppetpe/puppetlabs/pe_console/console_http.rb]/ensure: defined content as '{md5}b7525f7af33b49ac101aaa062ec09423'
Info: Loading facts
/bin/nmcli: symbol lookup error: /lib64/libgudev-1.0.so.0: undefined symbol: g_type_class_adjust_private_offset
Info: Caching catalog for agent.example.com
Info: Applying configuration version '1453845214'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/agent/package.ddl]/ensure: defined content as '{md5}12f8dce7d996343068b9372f110279ed'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/agent/package.rb]/ensure: defined content as '{md5}51d279e034f236194a9bf45461cb6033'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/agent/puppet.ddl]/ensure: defined content as '{md5}52cec2616132c6c7a9f256894db6bd34'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/agent/puppet.rb]/ensure: defined content as '{md5}6e2982dd1a087275d33730052cff8112'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/agent/puppetral.ddl]/ensure: defined content as '{md5}7f06f13953847e60818a681c1f2f168b'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/agent/puppetral.rb]/ensure: defined content as '{md5}5bc9d72845574a3fc08c9062b4b28dd3'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/agent/service.ddl]/ensure: defined content as '{md5}59ab37f55d8e16fda6a2103682545934'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/agent/service.rb]/ensure: defined content as '{md5}cbf84ed615eeda9789650b05ec504566'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/aggregate]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/aggregate/boolean_summary.ddl]/ensure: defined content as '{md5}aa581c71a6c7658bffdbaec81590f65d'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/aggregate/boolean_summary.rb]/ensure: defined content as '{md5}0546063313508d8aff603be320af3c44'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/application]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/application/package.rb]/ensure: defined content as '{md5}afcd9a561b087049eccb648a940b592e'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/application/puppet.rb]/ensure: defined content as '{md5}13731d27f1276cdd3314f7fa30aa5eb1'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/application/service.rb]/ensure: defined content as '{md5}799681457f0f707a7166da086f97e473'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/data/puppet_data.ddl]/ensure: defined content as '{md5}5c9912bf5ae5dbc8762109a40c027c63'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/data/puppet_data.rb]/ensure: defined content as '{md5}606e87cd509addf22dd8e93d503d8262'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/data/resource_data.ddl]/ensure: defined content as '{md5}c4e3a46fd3c0b5d3990db0b8af1c747f'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/data/resource_data.rb]/ensure: defined content as '{md5}49be769fb403191af41f1b89697ce4cc'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/data/service_data.ddl]/ensure: defined content as '{md5}e7f7e0bc65ede56fc636505a400b1700'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/data/service_data.rb]/ensure: defined content as '{md5}bc651898c7dcd373d609c933fbd6021f'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/registration/meta.rb]/ensure: defined content as '{md5}e939958bbbc0817e1779c336037e1849'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/security/sshkey.rb]/ensure: defined content as '{md5}8fa3e9125fd917948445e3d2621d40e5'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/actionpolicy.rb]/ensure: defined content as '{md5}e4d6a7024ad7b28e019e7b9931eac027'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/package]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/package/base.rb]/ensure: defined content as '{md5}1bdb7e7a6dcfea6fd2a06c5dc39b7276'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/package/packagehelpers.rb]/ensure: defined content as '{md5}af83db4ea2647516e50358df4166e571'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/package/puppetpackage.rb]/ensure: defined content as '{md5}161b48fa538e0ddd0118ab09f9405c51'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/puppet_agent_mgr]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/puppet_agent_mgr.rb]/ensure: defined content as '{md5}57f035c2def7a9767fb8996d3037e32d'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/puppet_agent_mgr/mgr_v2.rb]/ensure: defined content as '{md5}9a00171022ddb12d0a463e9cefeba481'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/puppet_agent_mgr/mgr_v3.rb]/ensure: defined content as '{md5}b5cb1a9b7311fc3769a3ccaabadeb694'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/puppet_agent_mgr/mgr_windows.rb]/ensure: defined content as '{md5}79a6cf3dac0177f6b9c22d5085324676'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/puppet_server_address_validation.rb]/ensure: defined content as '{md5}1c78390e33e71773e121a902ae91bfd4'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/puppetrunner.rb]/ensure: defined content as '{md5}a4fade81457455fbca9370249defbdf1'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/service]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/service/base.rb]/ensure: defined content as '{md5}abea7b8fadbf3425a7b68b49b9435ff6'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/util/service/puppetservice.rb]/ensure: defined content as '{md5}905db93e1c06ad5a7154fa2f9199f31c'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/validator/puppet_resource_validator.ddl]/ensure: defined content as '{md5}3e45a28e1ba6c8d22ce40934c04b30b4'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/validator/puppet_resource_validator.rb]/ensure: defined content as '{md5}567c7dc4d70ed0db7fd2626c77f6df41'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/validator/puppet_server_address_validator.ddl]/ensure: defined content as '{md5}323e0b9647639fdf32cfbc63a82860f7'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/validator/puppet_server_address_validator.rb]/ensure: defined content as '{md5}e84a56187809c5181b78b2819ee149fe'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/validator/puppet_tags_validator.ddl]/ensure: defined content as '{md5}7ed95b2e5b210db83d12d5034f1ecb0f'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/validator/puppet_tags_validator.rb]/ensure: defined content as '{md5}40b29498e867ba2ecf21dc08bc457d4e'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/validator/puppet_variable_validator.ddl]/ensure: defined content as '{md5}58c9db4ca4503e4d692a016743e01627'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/validator/puppet_variable_validator.rb]/ensure: defined content as '{md5}3cbca3af2e5884f2a807ef005a87151b'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/validator/service_name.ddl]/ensure: defined content as '{md5}2812afa15108103042f706c2201e286b'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Plugins/File[/opt/puppet/libexec/mcollective/mcollective/validator/service_name.rb]/ensure: defined content as '{md5}3f501a9ed252ce2dfe06a2e1e53845ab'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Logs/File[/var/log/pe-mcollective/mcollective.log]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Logs/File[/var/log/pe-mcollective/mcollective-audit.log]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl]/mode: mode changed '0755' to '0770'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/clients]/mode: mode changed '0755' to '0770'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/ca.cert.pem]/ensure: defined content as '{md5}76c8464d2a1030ff09cbcc67a052a396'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/agent.example.com.cert.pem]/ensure: defined content as '{md5}90f9b08774676926723d6ec152ba419b'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/agent.example.com.private_key.pem]/ensure: defined content as '{md5}e64340e4b19c4cb3d0d9da9fb73062f2'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/mcollective-private.pem]/ensure: defined content as '{md5}2921e0468089ab129294028cf2d621f8'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/mcollective-public.pem]/ensure: defined content as '{md5}ef4fbed02b91c77cfbb50fd9dc76849b'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/clients/puppet-dashboard-public.pem]/ensure: defined content as '{md5}cb310f8c69c1d214fc123fd20cb5e651'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Certs/File[/etc/puppetlabs/mcollective/ssl/clients/peadmin-public.pem]/ensure: defined content as '{md5}91862fc230d842b69255985e1ba3a4ab'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Facter/File[/opt/puppet/sbin/refresh-mcollective-metadata]/ensure: defined content as '{md5}3d950cdcfcc2d77efc84909b191eaeea'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server::Facter/Cron[pe-mcollective-metadata]/ensure: created
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]/content:
--- /etc/puppetlabs/mcollective/server.cfg  2015-04-09 17:29:33.000000000 +0000
+++ /tmp/puppet-file20160126-13013-1soaj6x  2016-01-26 21:53:36.630528689 +0000
@@ -1,22 +1,81 @@
-main_collective = mcollective
-collectives = mcollective
-libdir = /opt/puppet/libexec/mcollective/
-logfile = /var/log/pe-mcollective
-loglevel = info
-daemonize = 1

-# Plugins
-securityprovider = psk
-plugin.psk = unset
+# Centrally managed by Puppet version 3.8.4 (Puppet Enterprise 3.8.3)
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
+plugin.activemq.pool.1.host = puppet
+plugin.activemq.pool.1.port = 61613
 plugin.activemq.pool.1.user = mcollective
-plugin.activemq.pool.1.password = marionette
+plugin.activemq.pool.1.password = TxiPPcRRzBsIzFJ9m5kR
+plugin.activemq.pool.1.ssl = true
+plugin.activemq.pool.1.ssl.ca = /etc/puppetlabs/mcollective/ssl/ca.cert.pem
+plugin.activemq.pool.1.ssl.cert = /etc/puppetlabs/mcollective/ssl/agent.example.com.cert.pem
+plugin.activemq.pool.1.ssl.key = /etc/puppetlabs/mcollective/ssl/agent.example.com.private_key.pem
+plugin.activemq.heartbeat_interval = 120
+plugin.activemq.max_hbrlck_fails = 0

-# Facts
+# Security plugin settings (required):
+# -----------------------------------
+securityprovider           = ssl
+
+# SSL plugin settings:
+plugin.ssl_server_private  = /etc/puppetlabs/mcollective/ssl/mcollective-private.pem
+plugin.ssl_server_public   = /etc/puppetlabs/mcollective/ssl/mcollective-public.pem
+plugin.ssl_client_cert_dir = /etc/puppetlabs/mcollective/ssl/clients
+plugin.ssl_serializer      = yaml
+
+# Facts, identity, and classes (recommended):
+# ------------------------------------------
 factsource = yaml
-plugin.yaml = /etc/mcollective/facts.yaml
+plugin.yaml = /etc/puppetlabs/mcollective/facts.yaml
+
+identity = agent.example.com
+
+classesfile = /var/opt/lib/pe-puppet/classes.txt
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
+plugin.rpcaudit.logfile = /var/log/pe-mcollective/mcollective-audit.log
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
+logfile  = /var/log/pe-mcollective/mcollective.log
+loglevel = info
+
+# Platform defaults:
+# -----------------
+daemonize = 1
+libdir = /opt/puppet/libexec/mcollective/

+# Puppet Agent plugin configuration:
+# ---------------------------------
+plugin.puppet.splay = true
+plugin.puppet.splaylimit = 120
+plugin.puppet.signal_daemon = 0
+plugin.puppet.command = /opt/puppet/bin/puppet agent
+plugin.puppet.config  = /etc/puppetlabs/puppet/puppet.conf

Info: Computing checksum on file /etc/puppetlabs/mcollective/server.cfg
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]: Filebucketed /etc/puppetlabs/mcollective/server.cfg to main with sum c0feb345f525816545d9a23ec41469ff
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]/content: content changed '{md5}c0feb345f525816545d9a23ec41469ff' to '{md5}c0a0bf92d7da42c2809ba19d737ab6f1'
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]/mode: mode changed '0644' to '0660'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]: Scheduling refresh of Service[pe-mcollective]
Info: /Stage[main]/Puppet_enterprise::Mcollective::Server/File[/etc/puppetlabs/mcollective/server.cfg]: Scheduling refresh of Service[pe-mcollective]
Notice: /Stage[main]/Puppet_enterprise::Mcollective::Service/Service[pe-mcollective]/ensure: ensure changed 'stopped' to 'running'
Info: /Stage[main]/Puppet_enterprise::Mcollective::Service/Service[pe-mcollective]: Unscheduling refresh on Service[pe-mcollective]
Notice: Finished catalog run in 1.83 seconds
```

Here's the second puppet run.  Notice no more changes are applied.

```
[root@agent ~]# puppet agent -t
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for agent.example.com
Info: Applying configuration version '1453845260'
Notice: Finished catalog run in 0.30 seconds
```

Back to **Lab #4** --> [Install Puppet Agent](04-Install-Puppet-Agent.md#run-the-puppet-agent-again)

Continue to **Lab #5** --> [Get familiar with puppet config files, and puppet code, and CLI](05-Puppet-Config-and-Code.md#lab-5)

---

<-- [Back to Contents](/README.md)

---

=============================================================================================================================







