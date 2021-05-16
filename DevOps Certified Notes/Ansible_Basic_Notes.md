# Ansible notes

Notes I prepared while learning Ansible.

## Table of contents
### Day 1
1. [Install Ansible in Amazon AMI Environment](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#to-install-ansible-in-an-amazon-ami-env)
2. [Creating Inventory files](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#creating-inventory-files)
3. [How to check if a machine is reachable using Ansible](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#how-to-check-if-a-machine-is-reachable-using-ansiblevia-ssh)
4. [Shell module](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#shell-module)
5. [Different modules to run a shell command in ansible](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#different-modules-to-run-a-shell-command-in-ansible)
6. [Another example to depict between the differences of these 3 modules](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#another-example-to-depict-between-the-differences-of-these-3-modules)
7. [Resetting the default inventory files](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#resetting-the-default-inventory-files)
8. [Yum module](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#yum-module)
9. [Service Module](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#service-module)
10. [Copy module](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#copy-module-in-ansible)
11. [Introduction to Playbooks](https://github.com/syamj/Ansible-tutorials./blob/master/My-Ansible-docs-1.md#playbooks)

### Day 2
1. [Facts Gathering Using Ansible](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-2.md#facts-gathering-using-ansible)
2. [Conditional Execution Using Facts and When Statement](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-2.md#conditional-execution-using-facts-and-when-statement)
3. [Include tasks](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-2.md#include-tasks)
4. [Debug module and Register keyword](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-2.md#debug-module-and-register-keyword)
5. [Playbook to restart service only when a change is applied](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-2.md#playbook-to-restart-service-only-when-a-change-is-applied)
6. [Jinja2 Templates (template module)](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-2.md#jinja2-templates-template-module)

### Day 3
1. [Template module demo to modify wp.conf](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-3.md#template-module-demo-to-modify-wpconf)
2. [Installing and configuring Mysql(Mariadb)](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-3.md#installing-and-configuring-mysqlmariadb)
3. [Ignoring errors](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-3.md#ignoring-errors)
4. [Reading file from external file](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-3.md#reading-file-from-external-file)
5. [Encrypting files using Ansible Vault](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-3.md#encrypting-files-using-ansible-vault)
6. [Ansible Handlers](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-3.md#ansible-handlers)

### Day 4
1. [get_url](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-4.md#get_url)
2. [Unarchive module](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-4.md#unarchive-module)
3. [To remove LAMP Stack from remote machine](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-4.md#to-remove-lamp-stack-from-remote-machine)




### Day 5
1. [Stat module](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-5.md#stat-module)
2. [To delete the directory contents only](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-5.md#to-delete-the-directory-contents-only)
3. [To get the kernel status](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-5.md#to-get-the-kernel-status)
4. [Ansible Roles](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-5.md#ansible-roles)
5. [Different parts of a playbook](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-5.md#different-parts-of-a-playbook)
6. [Creating an empty role](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-5.md#creating-an-empty-role)

### Day 6
1. [Deploying a repo from Git](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-6.md#deploying-a-repo-from-git)
2. [Read value from csv file](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-6.md#read-value-from-csv-file)
3. [To Add multiple Users to a remote machine](https://github.com/syamj/Ansible-tutorials./blob/master/My-ansible-docs-6.md#to-add-multiple-users-to-a-remote-machine)



# Ansible


```python
#Ansible is a configuration management tool.

#Ansible is agentless, so we do not need to install anything in remote VM.

#Ansible is required only in control machine and it connects to remote machine via ssh.

#Passwordless ssh should be configured for ansible to run.
```

## To install Ansible in an Amazon AMI env

sudo amazon-linux-extras install ansible2

## Creating Inventory files

$ cat inventory.ini 

[amazon]
172.31.18.26 ansible_port=22 ansible_user='ec2-user' # ansible_ssh_private_key_file='path_to_ssh_key'

## How to check if a machine is reachable using Ansible(via ssh)


```python
#Ping module can be used to check if ansible is able to reach a remote server and execute code there.

ec2-user@ip-172-31-22-216 ~]$ ansible -i inventory.ini amazon -m ping
172.31.18.26 | SUCCESS => {
    "changed": false, 
    "failed": false, 
    "ping": "pong"
}
[ec2-user@ip-172-31-22-216 ~]$ ansible -i inventory.ini all -m ping
172.31.18.26 | SUCCESS => {
    "changed": false, 
    "failed": false, 
    "ping": "pong"
    }
```

## Shell module


```python
#Shell module is used to run shell commands in a remote VM using ansible

[ec2-user@ip-172-31-22-216 ~]$ ansible -i inventory.ini amazon -m shell -a 'free -m'
172.31.18.26 | SUCCESS | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:            983          59         600           0         323         772
Swap:             0           0           0
```

## Different modules to run a shell command in ansible


```python
#We can run shell commands using 3 different modules.

1. Shell

[ec2-user@ip-172-31-22-216 ~]$ ansible -i inventory.ini amazon -m shell -a 'free -m'
172.31.18.26 | SUCCESS | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:            983          59         600           0         323         772
Swap:             0           0           0

#This invokes a shell env in the remote vm and then executes the command and returns the result

2. Command Module

[ec2-user@ip-172-31-22-216 ~]$ ansible -i inventory.ini amazon -m command -a 'free -m'
172.31.18.26 | SUCCESS | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:            983          59         600           0         323         772
Swap:             0           0           0

#Command module executes a script without being preceeded by a shell. So shell variables like $HOME etc will not be available 
# And also stream operations like  <, >, | and & will not work.
# Command module is more secure, because it will not be affected by the user’s environment.

3. RAW Module


[ec2-user@ip-172-31-22-216 ~]$ ansible -i inventory.ini amazon -m raw -a 'free -m'
172.31.18.26 | SUCCESS | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:            983          48         612           0         323         784
Swap:             0           0           0
Shared connection to 172.31.18.26 closed.

# Executes a low-down and dirty SSH command, not going through the module subsystem.
# ie it basicall works like ssh remote-vm -e "free -m"
```

## Another example to depict between the differences of these 3 modules


```python
[ec2-user@ip-172-31-22-216 ~]$ ssh ec2-user@172.31.18.26 'ls /etc/*.conf | wc -l'
27
[ec2-user@ip-172-31-22-216 ~]$ ansible -i inventory.ini amazon -m shell -a 'ls /etc/*.conf | wc -l'
172.31.18.26 | SUCCESS | rc=0 >>
27

[ec2-user@ip-172-31-22-216 ~]$ ansible -i inventory.ini amazon -m command -a 'ls /etc/*.conf | wc -l'
172.31.18.26 | FAILED | rc=2 >>
ls: cannot access /etc/*.conf: No such file or directory
ls: cannot access |: No such file or directory
ls: cannot access wc: No such file or directorynon-zero return code

[ec2-user@ip-172-31-22-216 ~]$ ansible -i inventory.ini amazon -m raw -a 'ls /etc/*.conf | wc -l'
172.31.18.26 | SUCCESS | rc=0 >>
27
Shared connection to 172.31.18.26 closed.
```

## Resetting the default inventory files.


```python
# /etc/ansible/ansible.cfg is the default inventory file. We can add our own inventory file so that we donot want to mention 
# the inventory file where executing the commands each and every time.b

[ec2-user@ip-172-31-22-216 ~]$ grep inventory /etc/ansible/ansible.cfg 
#inventory      = /etc/ansible/hosts
inventory      = /home/ec2-user/inventory.ini
```

## Yum module


```python
#Below shows an amsible ad hoc command to install httpd on a remote computer.

[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -m yum -a 'name=httpd state=present'
172.31.18.26 | FAILED! => {
    "changed": false, 
    "failed": true, 
    "msg": "You need to be root to perform this command.\n", 
    "rc": 1, 
    "results": [
        "Loaded plugins: extras_suggestions, langpacks, priorities, update-motd\n"
    ]
}

#The above didn't work because we need to be root to install an package so, -b option is added(for being sudo)


[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -b -m yum -a 'name=httpd state=present'
172.31.18.26 | SUCCESS => {
    "changed": true, 
    "failed": false, 
    "msg": "", 
    "rc": 0, 
    "results": [
        "Loaded plugins: extras_suggestions, langpacks, priorities, update-motd\nResolving Dependencies\n--> Running transaction check\n---> Package httpd.x86_64 0:2.4.39-1.amzn2.0.1 will be installed\n--> Processing Dependency: httpd-tools = 2.4.39-1.amzn2.0.1 for package: httpd-2.4.39-1.amzn2.0.1.x86_64\n--> Processing Dependency: httpd-filesystem = 2.4.39-1.amzn2.0.1 for package: httpd-2.4.39-1.amzn2.0.1.x86_64\n--> Processing Dependency: system-logos-httpd for package: httpd-2.4.39-1.amzn2.0.1.x86_64\n--> Processing Dependency: mod_http2 for package: httpd-2.4.39-1.amzn2.0.1.x86_64\n--> Processing Dependency: httpd-filesystem for package: httpd-2.4.39-1.amzn2.0.1.x86_64\n--> Processing Dependency: /etc/mime.types for package: httpd-2.4.39-1.amzn2.0.1.x86_64\n--> Processing Dependency: libaprutil-1.so.0()(64bit) for package: httpd-2.4.39-1.amzn2.0.1.x86_64\n--> Processing Dependency: libapr-1.so.0()(64bit) for package: httpd-2.4.39-1.amzn2.0.1.x86_64\n--> Running transaction check\n---> Package apr.x86_64 0:1.6.3-5.amzn2.0.2 will be installed\n---> Package apr-util.x86_64 0:1.6.1-5.amzn2.0.2 will be installed\n--> Processing Dependency: apr-util-bdb(x86-64) = 1.6.1-5.amzn2.0.2 for package: apr-util-1.6.1-5.amzn2.0.2.x86_64\n---> Package generic-logos-httpd.noarch 0:18.0.0-4.amzn2 will be installed\n---> Package httpd-filesystem.noarch 0:2.4.39-1.amzn2.0.1 will be installed\n---> Package httpd-tools.x86_64 0:2.4.39-1.amzn2.0.1 will be installed\n---> Package mailcap.noarch 0:2.1.41-2.amzn2 will be installed\n---> Package mod_http2.x86_64 0:1.14.1-1.amzn2 will be installed\n--> Running transaction check\n---> Package apr-util-bdb.x86_64 0:1.6.1-5.amzn2.0.2 will be installed\n--> Finished Dependency Resolution\n\nDependencies Resolved\n\n================================================================================\n Package                 Arch       Version                Repository      Size\n================================================================================\nInstalling:\n httpd                   x86_64     2.4.39-1.amzn2.0.1     amzn2-core     1.3 M\nInstalling for dependencies:\n apr                     x86_64     1.6.3-5.amzn2.0.2      amzn2-core     118 k\n apr-util                x86_64     1.6.1-5.amzn2.0.2      amzn2-core      99 k\n apr-util-bdb            x86_64     1.6.1-5.amzn2.0.2      amzn2-core      19 k\n generic-logos-httpd     noarch     18.0.0-4.amzn2         amzn2-core      19 k\n httpd-filesystem        noarch     2.4.39-1.amzn2.0.1     amzn2-core      23 k\n httpd-tools             x86_64     2.4.39-1.amzn2.0.1     amzn2-core      87 k\n mailcap                 noarch     2.1.41-2.amzn2         amzn2-core      31 k\n mod_http2               x86_64     1.14.1-1.amzn2         amzn2-core     147 k\n\nTransaction Summary\n================================================================================\nInstall  1 Package (+8 Dependent packages)\n\nTotal download size: 1.9 M\nInstalled size: 5.1 M\nDownloading packages:\n--------------------------------------------------------------------------------\nTotal                                              8.8 MB/s | 1.9 MB  00:00     \nRunning transaction check\nRunning transaction test\nTransaction test succeeded\nRunning transaction\n  Installing : apr-1.6.3-5.amzn2.0.2.x86_64                                 1/9 \n  Installing : apr-util-bdb-1.6.1-5.amzn2.0.2.x86_64                        2/9 \n  Installing : apr-util-1.6.1-5.amzn2.0.2.x86_64                            3/9 \n  Installing : httpd-tools-2.4.39-1.amzn2.0.1.x86_64                        4/9 \n  Installing : generic-logos-httpd-18.0.0-4.amzn2.noarch                    5/9 \n  Installing : mailcap-2.1.41-2.amzn2.noarch                                6/9 \n  Installing : httpd-filesystem-2.4.39-1.amzn2.0.1.noarch                   7/9 \n  Installing : mod_http2-1.14.1-1.amzn2.x86_64                              8/9 \n  Installing : httpd-2.4.39-1.amzn2.0.1.x86_64                              9/9 \n  Verifying  : apr-util-1.6.1-5.amzn2.0.2.x86_64                            1/9 \n  Verifying  : apr-util-bdb-1.6.1-5.amzn2.0.2.x86_64                        2/9 \n  Verifying  : httpd-tools-2.4.39-1.amzn2.0.1.x86_64                        3/9 \n  Verifying  : httpd-2.4.39-1.amzn2.0.1.x86_64                              4/9 \n  Verifying  : httpd-filesystem-2.4.39-1.amzn2.0.1.noarch                   5/9 \n  Verifying  : apr-1.6.3-5.amzn2.0.2.x86_64                                 6/9 \n  Verifying  : mod_http2-1.14.1-1.amzn2.x86_64                              7/9 \n  Verifying  : mailcap-2.1.41-2.amzn2.noarch                                8/9 \n  Verifying  : generic-logos-httpd-18.0.0-4.amzn2.noarch                    9/9 \n\nInstalled:\n  httpd.x86_64 0:2.4.39-1.amzn2.0.1                                             \n\nDependency Installed:\n  apr.x86_64 0:1.6.3-5.amzn2.0.2                                                \n  apr-util.x86_64 0:1.6.1-5.amzn2.0.2                                           \n  apr-util-bdb.x86_64 0:1.6.1-5.amzn2.0.2                                       \n  generic-logos-httpd.noarch 0:18.0.0-4.amzn2                                   \n  httpd-filesystem.noarch 0:2.4.39-1.amzn2.0.1                                  \n  httpd-tools.x86_64 0:2.4.39-1.amzn2.0.1                                       \n  mailcap.noarch 0:2.1.41-2.amzn2                                               \n  mod_http2.x86_64 0:1.14.1-1.amzn2                                             \n\nComplete!\n"
    ]
}



#Now if we run the same command again, ansible would do nothing because httpd is alread present in the remote VM.

[ec2-user@ip-172-31-22-216 ~]$ 
[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -b -m yum -a 'name=httpd state=present'
172.31.18.26 | SUCCESS => {
    "changed": false, 
    "failed": false, 
    "msg": "", 
    "rc": 0, 
    "results": [
        "httpd-2.4.39-1.amzn2.0.1.x86_64 providing httpd is already installed"
    ]
}
[ec2-user@ip-172-31-22-216 ~]$ 
```

## Service Module


```python
#Service module is used to check status/start/stop/restart any specific service running in the remote VM.

#The below will restart httpd in the remote VM

[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -b -m service -a 'name=httpd state=restarted'
172.31.18.26 | SUCCESS => {
    "changed": true, 
    "failed": false, 
    "name": "httpd", 
    "state": "started", 

    
   
    
#Below will check if httpd is started, if not will start it. If alread started, it will do nothing.    
    [ec2-user@ip-172-31-22-216 ~]$ ansible amazon -b -m service -a 'name=httpd state=started'
172.31.18.26 | SUCCESS => {
    "changed": false, 
    "failed": false, 
    "name": "httpd", 
    "state": "started", 
```

## Copy module in ansible


```python
#Copy module is used to copy a file/text to a file in remote VM.

#This will copy index.html from the execution directory and copy it to /var/www/html/ of remote VM.

[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -b -m copy -a 'src=index.html dest=/var/www/html/'
172.31.18.26 | SUCCESS => {
    "changed": true, 
    "checksum": "754b1df117c447f8caf63d0fff4aa002066a5fe0", 
    "dest": "/var/www/html/index.html", 
    "failed": false, 
    "gid": 0, 
    "group": "root", 
    "md5sum": "7ef01811d5b57b410f436204a04e416a", 
    "mode": "0644", 
    "owner": "root", 
    "size": 51, 
    "src": "/home/ec2-user/.ansible/tmp/ansible-tmp-1560501987.83-1848944088840/source", 
    "state": "file", 
    "uid": 0
}

#Now if we run the command again, nothing will change because both the files are exactly same.


[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -b -m copy -a 'src=index.html dest=/var/www/html/'
172.31.18.26 | SUCCESS => {
    "changed": false, 
    "checksum": "754b1df117c447f8caf63d0fff4aa002066a5fe0", 
    "dest": "/var/www/html/index.html", 
    "failed": false, 
    "gid": 0, 
    "group": "root", 
    "mode": "0644", 
    "owner": "root", 
    "path": "/var/www/html/index.html", 
    "size": 51, 
    "state": "file", 
    "uid": 0
}

#After making changes in the index.html file, if we run the command, it will execute the copy and update remote VM with new file.


[ec2-user@ip-172-31-22-216 ~]$ vim index.html 
[ec2-user@ip-172-31-22-216 ~]$ 
[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -b -m copy -a 'src=index.html dest=/var/www/html/'
172.31.18.26 | SUCCESS => {
    "changed": true, 
    "checksum": "10fc37f59d03f64a6cc8aa6cc59923f2bc812ee8", 
    "dest": "/var/www/html/index.html", 
    "failed": false, 
    "gid": 0, 
    "group": "root", 
    "md5sum": "012322d1b2bda728e68e82d8ef98380a", 
    "mode": "0644", 
    "owner": "root", 
    "size": 51, 
    "src": "/home/ec2-user/.ansible/tmp/ansible-tmp-1560502055.65-6558282353603/source", 
    "state": "file", 
    "uid": 0
}
[ec2-user@ip-172-31-22-216 ~]$ 
```

## Playbooks


```python
#Playbooks are written in yml format. 
#Intendation is really important for yml files.
# A yml file starts with ---


#Here we write the above examples of httpd install, restart service and copy index.html using a single playbook.

[ec2-user@ip-172-31-22-216 ~]$ cat httpd-install.yml 
---
- name: installing httpd on amazon linux
  hosts: amazon
  become: yes
  tasks: 
    - name: installing httpd
      yum: 
        name: httpd
        state: present
    - name: restarting httpd
      service:
        name: httpd
        state: restarted
    - name: creating index.html
      copy:
        src: index.html
        dest: /var/www/html/
   
[ec2-user@ip-172-31-22-216 ~]$ ansible-playbook httpd-install.yml 

PLAY [installing httpd on amazon linux] ******************************************************************************

TASK [Gathering Facts] ***********************************************************************************************
ok: [172.31.18.26]

TASK [installing httpd] **********************************************************************************************
ok: [172.31.18.26]

TASK [restarting httpd] **********************************************************************************************
changed: [172.31.18.26]

TASK [creating index.html] *******************************************************************************************
changed: [172.31.18.26]

PLAY RECAP ***********************************************************************************************************
172.31.18.26               : ok=4    changed=2    unreachable=0    failed=0   
```


```python

```


=========================================================================================================================================


## Facts Gathering Using Ansible


```python
#Ansible uses setup module to gather facts about the remote VM.
# Before a playbook/module is run at remote VM, ansible automatically run the setup module and grab details about the remote VM.

[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -m setup | grep "ansible_distribution"
        "ansible_distribution": "Amazon", 
        "ansible_distribution_file_parsed": true, 
        "ansible_distribution_file_path": "/etc/system-release", 
        "ansible_distribution_file_variety": "Amazon", 
        "ansible_distribution_major_version": "NA", 
        "ansible_distribution_release": "NA", 
        "ansible_distribution_version": "(Karoo)",
            
#We'll get a whole lot of info about the remote VM using the setup module.

[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -m setup | grep "ansible_os_family"
        "ansible_os_family": "RedHat", 
[ec2-user@ip-172-31-22-216 ~]$ 

[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -m setup | grep "memory" -A 15
        "ansible_memory_mb": {
            "nocache": {
                "free": 893, 
                "used": 90
            }, 
            "real": {
                "free": 762, 
                "total": 983, 
                "used": 221
            }, 
            "swap": {
                "cached": 0, 
                "free": 0, 
                "total": 0, 
                "used": 0
            }
            
[ec2-user@ip-172-31-22-216 ~]$ ansible amazon -m setup | grep "ansible_all_ipv4_addresses" -A 2
        "ansible_all_ipv4_addresses": [
            "172.31.18.26"
        ], 
```

# Conditional Execution Using Facts and When Statement


```python
# In a case where we have VM's with different flavours of OS in our inventory.

# Our aim is to install httpd in all these VM's.

# While debian uses apt module to install packages, rhel uses yum package.

# We can use the when statement here.

---
- name: Playbook to install apazhe in different OS falvour.
  hosts: amazon
  become: yes
  tasks:
    - name: Install apache2 Debian
      when: ansible_os_family == "Debian"
      apt:
        name: apache2
        state: present
        update_cache: yes
            
    - name: Debian service restart
      when: ansible_os_family == "Debian"
      service:
        name: apache2
        state: restarted
        enabled: yes
            
    - name: Install httpd Red Hat
      when: ansible_os_distribution == "RedHat"
      yum:
        name: httpd
        state: present
        
    - name: Redhat service restart
      when: ansible_os_distribution == "RedHat"
      service:
        name: httpd
        state: restarted
        enabled: yes
            
            
            

            
            [ec2-user@ip-172-31-22-216 ~]$ ansible-playbook 03-01-httpd-on-redhat-debian.yml 

PLAY [Playbook to install apazhe in different OS falvour.] ***************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [172.31.18.26]

TASK [Install apache2 Debian] ********************************************************************************************************************************
skipping: [172.31.18.26]

TASK [Debian service restart] ********************************************************************************************************************************
skipping: [172.31.18.26]

TASK [Install httpd Red Hat] *********************************************************************************************************************************
ok: [172.31.18.26]

TASK [Redhat service restart] ********************************************************************************************************************************
changed: [172.31.18.26]

PLAY RECAP ***************************************************************************************************************************************************
172.31.18.26               : ok=3    changed=1    unreachable=0    failed=0   
```


```python
#Ansible will gather facts(run setup module) at the start of execution of every playbook.

#We can disable it for faster execution of playbook.

[ec2-user@ip-172-31-22-216 ~]$ cat 03-01-httpd-on-redhat-debian.yml 
---
- name: Playbook to install apazhe in different OS falvour.
  hosts: amazon
  become: yes
  gather_facts: no
  tasks:
```

## Include tasks


```python
#The playbook written in above example to install httpd in different OS flavour can be written in another way.

# We can split them into 3 different playbooks.


#Debian.yml which will have tasks only for debian falvoured OS
$vim debian.yml

---
- name: Install apache2 Debian
  apt:
    name: apache2
    state: present
    update_cache: yes
            
- name: Debian service restart
  service:
    name: apache2
    state: restarted
    enabled: yes

#Redhat.yml which will have tasks only for RHEL flavoured OS.
$vim redhat.yml

---
- name: Install httpd Red Hat
  yum:
    name: httpd
    state: present
        
- name: Redhat service restart
  service:
    name: httpd
    state: restarted
    enabled: yes

#Now we'll write a main playbook to execute the above 2 playbooks according to OS flavour.

$vim main.yml

---
- name: Install apache on multiple OS flavour
  become: yes
  hosts: amazon
  tasks:
    - include_tasks: debian.yml
      when: ansible_os_family == "Debian"
    - include_tasks: redhat.yml
      when: ansible_os_family == "RedHat"
        
#So basically we just have to execute this main.yml and it will automatically call debian or redhat.yml according to need.


```

## Debug module and Register keyword


```python
#The below playbook is written to create a file /tmp/test-syam.txt and write "This is test file" to it.
#Note : **contend** is used inside copy module to write the text to to dest file.

#The logs of a task will not be visible b default, so we regiter it to a variable "var_tmp_write"

#To view the logs we use debug module. Syntax below:

$vim 03-03-register-demo.yml

---
- name: Register and debug test
  hosts: localhost
  gather_facts: no
  tasks:
    - name: Creating a tmp file
      copy:
        content: "This is test file"
        dest: /tmp/test-syam.txt
      register: var_tmp_write
        
    - name: Debugging the copy using the register variable
      debug:
        vars: var_tmp_write
```


```python
#Below is the output of the abov playbook.


[ec2-user@ip-172-31-22-216 ~]$ ansible-playbook 03-03-register-demo.yml 

PLAY [Register and debug test] *******************************************************************************************************************************

TASK [Creating a tmp file] ***********************************************************************************************************************************
changed: [localhost]

TASK [Debugging the copy using the register variable] ********************************************************************************************************
ok: [localhost] => {
    "var_tmp_write": {
        "changed": true, 
        "checksum": "857504db1244e427e1cce312af67ec65be34bf91", 
        "dest": "/tmp/test-syam.txt", 
        "diff": [], 
        "failed": false, 
        "gid": 1000, 
        "group": "ec2-user", 
        "md5sum": "8a813afddc95f11494a907466bd32e59", 
        "mode": "0664", 
        "owner": "ec2-user", 
        "size": 17, 
        "src": "/home/ec2-user/.ansible/tmp/ansible-tmp-1560744252.94-128626510313672/source", 
        "state": "file", 
        "uid": 1000
    }
}

PLAY RECAP ***************************************************************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0   

#Now if we run the playbook again, it will give below result.
#No changes will be made because the file is already there and its contends is also there.
    
    
[ec2-user@ip-172-31-22-216 ~]$ ansible-playbook 03-03-register-demo.yml 

PLAY [Register and debug test] *******************************************************************************************************************************

TASK [Creating a tmp file] ***********************************************************************************************************************************
ok: [localhost]

TASK [Debugging the copy using the register variable] ********************************************************************************************************
ok: [localhost] => {
    "var_tmp_write": {
        "changed": false, 
        "checksum": "857504db1244e427e1cce312af67ec65be34bf91", 
        "dest": "/tmp/test-syam.txt", 
        "diff": {
            "after": {
                "path": "/tmp/test-syam.txt"
            }, 
            "before": {
                "path": "/tmp/test-syam.txt"
            }
        }, 
        "failed": false, 
        "gid": 1000, 
        "group": "ec2-user", 
        "mode": "0664", 
        "owner": "ec2-user", 
        "path": "/tmp/test-syam.txt", 
        "size": 17, 
        "state": "file", 
        "uid": 1000
    }
}

PLAY RECAP ***************************************************************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0   

[ec2-user@ip-172-31-22-216 ~]$ 
```

## Playbook to restart service only when a change is applied.




```python
$vim 03-04-restart-only-when-task-changed.yml

---
- name: Restart httpd only when a change is applied.
  hosts: amazon
  become: yes
  tasks:
    - name: Installing LAMP
      yum:
        name:
          - httpd
          - php
          - php-mysql
        state: present
      register: installation_status
    
    - name: Creating PHP file
      copy:
        content: '</php phpinfo(); ?>'
        dest: /var/www/html/index.php
      register: php_content_status
        
    - name: Restarting httpd
      service:
        name: httpd
        state: restarted
      when: installation_status.changed == true or php_content_status.changed == true
```

## Jinja2 Templates (template module)


```python
#Here we use a template file(sample.j2) and create it in a remote VM using the name variable.

[ec2-user@ip-172-31-22-216 ~]$ cat 03-05-jinja-template-demo.yml 
---
- hosts: localhost
  vars:
    name: Syam
  tasks:
    - name: "Template Demo"
      template: 
         src: sample.j2
         dest: /tmp/{{name}}.txt
            
            

[ec2-user@ip-172-31-22-216 ~]$ ansible-playbook 03-05-jinja-template-demo.yml 
 [WARNING]: Found variable using reserved name: name


PLAY [localhost] *********************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [localhost]

TASK [Template Demo] *****************************************************************************************************************************************
changed: [localhost]

PLAY RECAP ***************************************************************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0   

[ec2-user@ip-172-31-22-216 ~]$ 

```

===============================================================================================================================================================


# Ansible Day 3

## Template module demo to modify wp.conf


```python
#wp-conf.j2 is the sameple template file. Variables mentioned in the yml file will be substituted here.

[ec2-user@ip-172-31-22-216 ~]$ cat wp-config.j2 
define( 'DB_NAME', '{{mysql_db_name}}' );

define( 'DB_USER', '{{mysql_username}}' );

define( 'DB_PASSWORD', '{{mysql_password}}' );


#The playbook to create a wp-config.php in the /tmp and write the config values.

[ec2-user@ip-172-31-22-216 ~]$ cat wpconfig-change.yml
---
- hosts: localhost
  vars: 
    mysql_username: uuseradmin
    mysql_password: password123
    mysql_db_name: testdb
   
  tasks:
    - name: substitute wpconf with exact values
      template:
        src: wp-config.j2
        dest: /tmp/wp-config.php
            
            
#Executing playbook.
    
[ec2-user@ip-172-31-22-216 ~]$ ansible-playbook wpconfig-change.yml 

PLAY [localhost] *********************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [localhost]

TASK [substitute wpconf with exact values] *******************************************************************************************************************
changed: [localhost]

PLAY RECAP ***************************************************************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0   
    
#checking the /tmp/wp-config.php file.
#The variables are replacd with exact values.

[ec2-user@ip-172-31-22-216 ~]$ cat /tmp/wp-config.php 
define( 'DB_NAME', 'testdb' );

define( 'DB_USER', 'uuseradmin' );

define( 'DB_PASSWORD', 'password123' );
```

# Installing and configuring Mysql(Mariadb)


```python
[ec2-user@ip-172-31-22-216 ~]$ cat mariadb_install.yml 
---
- name: "MariaDB Installation and Configuration for Wordpress"
  hosts: amazon
  become: yes
#Variable declaration

  vars:
    mysql_username: mysqluser
    mysql_password: password123
    mysql_db_name: test_mysqldb
    mysql_root_passwd: root123


  tasks:
    - name: MariaDB Server Installation
      yum:
        name:
          - MySQL-python
          - mariadb-server
        state: present

    - name: "Restarting Mysql"
      service:
        name: mariadb
        state: restarted

    - name: "MariaDB Server Resetting root password"
      mysql_user:
        login_user: root
        login_password: ''
        name: root
        host_all: yes
        password: "{{mysql_root_passwd}}"

    - name: "Removing anonymous Users from Mysql DB"
      mysql_user:
        login_user: root
        login_password: "{{mysql_root_passwd}}"
        name: ''
        host_all: yes
        state: absent

    - name: "DB Creation"
      mysql_db:
        login_user: root
        login_password: "{{mysql_root_passwd}}"
        db: "{{mysql_db_name}}"
        state: present
    
    - name: "Creating User and granting privilege"
      mysql_user:
        login_user: root
        login_password: "{{mysql_root_passwd}}"
        name: "{{mysql_username}}"
        host: localhost
        password: "{{mysql_password}}"
        priv: "{{mysql_db_name}}.*:ALL"
[ec2-user@ip-172-31-22-216 ~]$ 
```

### Ignoring errors


```python
#To ignore errors while executing a task, ignore_errors: yes needs to be used inside a task.

#Example: In the above playbook, if a new user needs to be created, adding a line in for resetting mysql root password will help.

    - name: "MariaDB Server Resetting root password"
      ignore_errors: yes

        

PLAY [MariaDB Installation and Configuration for Wordpress] **************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [172.31.18.26]

TASK [MariaDB Server Installation] ***************************************************************************************************************************
ok: [172.31.18.26]

TASK [Restarting Mysql] **************************************************************************************************************************************
changed: [172.31.18.26]

TASK [MariaDB Server Resetting root password] ****************************************************************************************************************
fatal: [172.31.18.26]: FAILED! => {"changed": false, "failed": true, "msg": "unable to connect to database, check login_user and login_password are correct or /root/.my.cnf has the credentials. Exception message: (1045, \"Access denied for user 'root'@'localhost' (using password: NO)\")"}
...ignoring

TASK [Removing anonymous Users from Mysql DB] ****************************************************************************************************************
ok: [172.31.18.26]

TASK [DB Creation] *******************************************************************************************************************************************
changed: [172.31.18.26]

TASK [Creating User and granting privilege] ******************************************************************************************************************
changed: [172.31.18.26]

PLAY RECAP ***************************************************************************************************************************************************
172.31.18.26               : ok=7    changed=3    unreachable=0    failed=0 
```

### Reading file from external file


```python
# Create a new file with all variables

[ec2-user@ip-172-31-22-216 ~]$ cat mysql-vars.yml 
---
mysql_username: mysqluser
mysql_password: password123
mysql_db_name: test_mysqldb
mysql_root_passwd: root123

#In the playbook mention the filename under vars_files: module


  vars_files:
   - mysql-vars.yml

```

## Encrypting files using Ansible Vault


```python
#We can encrypt files with important data using ansible-vault

ec2-user@ip-172-31-22-216 ~]$ ansible-vault encrypt mysql-vars.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
[ec2-user@ip-172-31-22-216 ~]$ 
[ec2-user@ip-172-31-22-216 ~]$ 
[ec2-user@ip-172-31-22-216 ~]$ cat mysql-vars.yml 
$ANSIBLE_VAULT;1.1;AES256
62313362623265636564623930306464313661666635616130666565363362653566643831396134
3865343466376565393730336535373232653661636566650a336565383232623966376664383534
38333436336132323133303163393332303965336563366362363637376631623437356437316539
3261353035383239660a323032326466333435613230613362323331646564333462383538623337
66663963373435363931656463373239313363613163313662363430343662353737323230363763
62336537346333663034366462363730363832633134316335366638653335623934666131313830
36316664336235343739333561383461373565653032356232346661306130656166306230303232
63393531643464376337316135626139346439396461323435303561666161363033376666666466
36303039393839653834393236633738383730636133383531393163333634643264313331333061
3933623138643863373136376136646538303539616333383165

```


```python
#--ask-vault-pass should be used with playbook command to decrypt the file while executing playbook, else it will return error.

[ec2-user@ip-172-31-22-216 ~]$ ansible-playbook mariadb_install.yml 
ERROR! Attempting to decrypt but no vault secrets found


#--ask-vault-pass should be used with playbook command and password should entered.

[ec2-user@ip-172-31-22-216 ~]$ ansible-playbook --ask-vault-pass mariadb_install.yml 
Vault password: 

PLAY [MariaDB Installation and Configuration for Wordpress] **************************************************************************************************

```


```python
#To decrypt the encrypted file below command shoudl be used.


[ec2-user@ip-172-31-22-216 ~]$ ansible-vault decrypt mysql-vars.yml 
Vault password: 
Decryption successful
[ec2-user@ip-172-31-22-216 ~]$ cat mysql-vars.yml 
---
mysql_username: mysqluser
mysql_password: password123
mysql_db_name: test_mysqldb
mysql_root_passwd: root123
[ec2-user@ip-172-31-22-216 ~]$ 
```

# Ansible Handlers


```python
#Playbook to install LAMP using handlers.
#A notify is set after a task which will only be executed if a task return value is changed.
#Note: The handler task will only be executed after all the tasks are completed.

[ec2-user@ip-172-31-22-216 ~]$ cat handler.yml 
---
- hosts: amazon
  become: yes
  tasks:
    - name: 'Installing Httpd'
      yum:
        name: httpd
        state: present
      notify: restarting-httpd

    - name: 'Installing php'
      yum:
        name:
          - php
          - php-mysql
          - php-mbstring
        state: present
      notify: 
         - dummy-task1
         - dummy-task2
  
    - name: 'Creating phpsite'
      copy:
        content: '<h1>sample 1 content</h1>'
        dest: /var/www/html/index.html
      notify: restarting-httpd

  handlers:

    - name: 'restarting-httpd'
      service:
        name: httpd
        state: restarted

    - name: 'dummy-task1'
      debug:
        msg: 'executing dummy task1'

    - name: 'dummy-task2'
      debug:
        msg: 'executing dummy task2'
[ec2-user@ip-172-31-22-216 ~]$ 

[ec2-user@ip-172-31-22-216 ~]$ ansible-playbook handler.yml 

PLAY [amazon] ************************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [172.31.18.26]

TASK [Installing Httpd] **************************************************************************************************************************************
ok: [172.31.18.26]

TASK [Installing php] ****************************************************************************************************************************************
changed: [172.31.18.26]

TASK [Creating phpsite] **************************************************************************************************************************************
changed: [172.31.18.26]

RUNNING HANDLER [restarting-httpd] ***************************************************************************************************************************
changed: [172.31.18.26]

RUNNING HANDLER [dummy-task1] ********************************************************************************************************************************
ok: [172.31.18.26] => {
    "msg": "executing dummy task1"
}

RUNNING HANDLER [dummy-task2] ********************************************************************************************************************************
ok: [172.31.18.26] => {
    "msg": "executing dummy task2"
}

PLAY RECAP ***************************************************************************************************************************************************
172.31.18.26               : ok=7    changed=3    unreachable=0    failed=0   


    

```

===============================================================================================================================================================


# Ansible Day 4

## get_url:


```python
#get_url module can be used to download a file from the internet.

get_url: 
  url: https://wordpress.org/wordpress-4.5.8.tar.gz
  dest: /tmp/wordpress.tar.gz
```

## Unarchive module


```python
#Unarchive module unzips/untars compressed files and put it in the destination directory of localhost.
# If the source and dest is remote server remote_src = yes parameter should also be given.

unarchive:
  src: /tmp/wordpress.tar.gz
  dest: /tmp/
  remote_src: yes
```


```python
#Below is a sample Wordpress conf file.

# vim wp-config.php.j2



<?php

define( 'DB_NAME', '{{mysql_database}}' );

define( 'DB_USER', '{{mysql_username}}' );

define( 'DB_PASSWORD', '{{mysql_password}}' );

define( 'DB_HOST', 'localhost' );

define( 'DB_CHARSET', 'utf8' );

define( 'DB_COLLATE', '' );


define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );


$table_prefix = 'wp_';


define( 'WP_DEBUG', false );


if ( ! defined( 'ABSPATH' ) ) {
	define( 'ABSPATH', dirname( __FILE__ ) . '/' );
}

require_once( ABSPATH . 'wp-settings.php' );
```


```python
#Playbook to download and install Wordpress in a remote Server.

# vim 05-01-wordpress-demo.yml

---
- hosts: amazon
  become: yes
  vars:
    mysql_username: wordpress
    mysql_password: wordpress123
    mysql_database: wordpress
  
  tasks:
    - name: 'Wordpress - Downloading.' 
      get_url:
        url: https://wordpress.org/wordpress-4.5.8.tar.gz
        dest: /tmp/wordpress.tar.gz
    
    - name: 'Wordpress - Extraction.'
      unarchive:
        src: /tmp/wordpress.tar.gz
        dest: /tmp/
        remote_src: yes
    
    - name: 'Wordpress - Copying To DocumentRoot.'
      shell: 'cp -r /tmp/wordpress/*  /var/www/html/'
    
    - name: 'Wordpress - Creating wp-config.php'
      template:
        src: wp-config.php.j2
        dest: /var/www/html/wp-config.php
```

# To remove LAMP Stack from remote machine


```python
ansible amazon -m shell -b  -a 'yum remove httpd php-* mariadb-* mariadb -y'
ansible amazon -m shell -b  -a 'rm -rf /var/www/html/* ; rm -rf /etc/httpd/ ; rm -rf /var/lib/mysql/* ; rm -rf /tmp/*'
```


```python
#To install Wordpress with LAMP Stack

# vim 05-02-wordpress-install.yml

---
- name: 'Wordpress Installation On Amazon Linux'
  hosts: all
  become: yes
  vars:
    domain: www.example.com
    mysql_root: mysqlroot123
    wordpress_user: wordpress
    wordpress_password: wordpress123
    wordpress_database: wordpress
      
  tasks:
    - name: 'LampStack - Installation'
      yum:
        name:
          - httpd
          - php
          - php-mysql
          - mariadb-server
          - MySQL-python
        state: present
          
    - name: 'LampStack - Restarting/Enabling'
      service:
        name: "{{item}}"
        state: restarted
        enabled: yes
      with_items:
        - mariadb
        - httpd
          
    - name: 'LampStack - Creating Virtualhost'
      template:
        src: virtualhost.j2
        dest: /etc/httpd/conf.d/{{domain}}.conf
          
    - name: 'LampStack - Creating Documentroot'
      file:
        path: /var/www/html/{{domain}}
        state: directory  
          
          
    - name: 'LampStack - Creating info.html'
      copy:
        content: "<h1><center>info.html is working</center></h1>"
        dest: /var/www/html/{{domain}}/info.html      
          
    - name: 'LampStack - Creating info.php'
      copy:
        content: "<?php phpinfo(); ?>"
        dest: /var/www/html/{{domain}}/info.php
          
    - name: 'MariaDb - Resetting Root Password'
      mysql_user:
        login_user: root
        login_password: ''
        name: root
        host_all: yes
        password: "{{ mysql_root }}"
          
    - name: 'MariaDb - Removing Anonymous Users'
      mysql_user:
        login_user: root
        login_password: "{{ mysql_root }}"
        name: ''
        host_all: yes
        state: absent
          
    - name: 'MariaDb - Creating Wordpress Database'
      mysql_db:
        login_user: root
        login_password: "{{ mysql_root }}"
        db: "{{ wordpress_database }}"
        state: present
          
          
    - name: 'MariaDb - Creating WordPress User and Privileges'
      mysql_user:
        login_user: root
        login_password: "{{ mysql_root }}"
        name: "{{ wordpress_user }}"
        host: localhost
        password: "{{wordpress_password}}"
        priv: "{{wordpress_database}}.*:ALL"
          
    - name: 'Wordpress - Downloading'
      get_url:
        url: https://wordpress.org/wordpress-4.5.8.tar.gz
        dest: /tmp/wordpress.tar.gz
          
    - name: 'Wordpress - Extraction'
      unarchive:
        src: /tmp/wordpress.tar.gz
        dest: /tmp/
        remote_src: yes
    
    - name: 'Wordpress - Copying Wordpress To DocumentRoot'
      shell: 'cp -r /tmp/wordpress/*  /var/www/html/{{domain}}'
        
    - name: 'Wordpress - Creating wp-config.php'
      template:
        src: wp-config.php.j2
        dest: /var/www/html/{{domain}}/wp-config.php
          
    - name: 'LampStack - Final Restarting'
      service:
        name: "{{item}}"
        state: restarted
      with_items:
        - mariadb
        - httpd
    
```

=============================================================================================================================================


# Ansible Day 5

## Stat module


```python
#Stat module is used to find the info of a file/directory.
#It can also be used to check if a file/folder exists or not.

[ec2-user@ip-172-31-22-216 playbooks]$ cat stat-demo.yml 
---
- hosts: localhost
  tasks:
    - name: 'Stat of /etc/passwd'
      stat: path=/etc/passwd
      register: passwdStatus

    - debug: var=passwdStatus

    - name: 'Stat of /etc'
      stat: path=/etc
      register: etcStatus
    
    - debug: var=etcStatus

    - name: 'Stat of /unknown'
      stat: path=/unknown
      register: unknownStatus

    - debug: var=unknownStatus
      
[ec2-user@ip-172-31-22-216 playbooks]$ 
```


```python
#Executing the above playbook

[ec2-user@ip-172-31-22-216 playbooks]$ ansible-playbook stat-demo.yml 

PLAY [localhost] *********************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [localhost]

TASK [Stat of /etc/passwd] ***********************************************************************************************************************************
ok: [localhost]

TASK [debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "passwdStatus": {
        "changed": false, 
        "failed": false, 
        "stat": {
            "atime": 1560498803.6619854, 
            "attr_flags": "", 
            "attributes": [], 
            "block_size": 4096, 
            "blocks": 8, 
            "charset": "us-ascii", 
            "checksum": "df1ec03c04e409d94def5bfe689a937f3f725362", 
            "ctime": 1560498803.6619854, 
            "dev": 51713, 
            "device_type": 0, 
            "executable": false, 
            "exists": true, 
            "gid": 0, 
            "gr_name": "root", 
            "inode": 8711679, 
            "isblk": false, 
            "ischr": false, 
            "isdir": false, 
            "isfifo": false, 
            "isgid": false, 
            "islnk": false, 
            "isreg": true, 
            "issock": false, 
            "isuid": false, 
            "md5": "155dc66f6c88851c2784e875e7f60e1e", 
            "mimetype": "text/plain", 
            "mode": "0644", 
            "mtime": 1560498803.6619854, 
            "nlink": 1, 
            "path": "/etc/passwd", 
            "pw_name": "root", 
            "readable": true, 
            "rgrp": true, 
            "roth": true, 
            "rusr": true, 
            "size": 1238, 
            "uid": 0, 
            "version": "18446744073442412900", 
            "wgrp": false, 
            "woth": false, 
            "writeable": false, 
            "wusr": true, 
            "xgrp": false, 
            "xoth": false, 
            "xusr": false
        }
    }
}

TASK [Stat of /etc] ******************************************************************************************************************************************
ok: [localhost]

TASK [debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "etcStatus": {
        "changed": false, 
        "failed": false, 
        "stat": {
            "atime": 1557345885.972871, 
            "attr_flags": "", 
            "attributes": [], 
            "block_size": 4096, 
            "blocks": 24, 
            "charset": "binary", 
            "ctime": 1560499512.5882022, 
            "dev": 51713, 
            "device_type": 0, 
            "executable": true, 
            "exists": true, 
            "gid": 0, 
            "gr_name": "root", 
            "inode": 8409184, 
            "isblk": false, 
            "ischr": false, 
            "isdir": true, 
            "isfifo": false, 
            "isgid": false, 
            "islnk": false, 
            "isreg": false, 
            "issock": false, 
            "isuid": false, 
            "mimetype": "inode/directory", 
            "mode": "0755", 
            "mtime": 1560499512.5882022, 
            "nlink": 81, 
            "path": "/etc", 
            "pw_name": "root", 
            "readable": true, 
            "rgrp": true, 
            "roth": true, 
            "rusr": true, 
            "size": 8192, 
            "uid": 0, 
            "version": "18446744071717444343", 
            "wgrp": false, 
            "woth": false, 
            "writeable": false, 
            "wusr": true, 
            "xgrp": true, 
            "xoth": true, 
            "xusr": true
        }
    }
}

TASK [Stat of /unknown] **************************************************************************************************************************************
ok: [localhost]

TASK [debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "unknownStatus": {
        "changed": false, 
        "failed": false, 
        "stat": {
            "exists": false
        }
    }
}

PLAY RECAP ***************************************************************************************************************************************************
localhost                  : ok=7    changed=0    unreachable=0    failed=0   

```


```python
#To check if a file/folder exists in remote server

[ec2-user@ip-172-31-22-216 playbooks]$ cat stat-demo.yml 
---
- hosts: localhost
  tasks:
    - name: 'Stat of /etc/passwd'
      stat: path=/etc/passwd
      register: passwdStatus

    - debug: var=passwdStatus.stat.exists

    - name: 'Stat of /etc'
      stat: path=/etc
      register: etcStatus
    
    - debug: var=etcStatus.stat.exists

    - name: 'Stat of /unknown'
      stat: path=/unknown
      register: unknownStatus

    - debug: var=unknownStatus.stat.exists

```


```python
[ec2-user@ip-172-31-22-216 playbooks]$ ansible-playbook stat-demo.yml 

PLAY [localhost] *********************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [localhost]

TASK [Stat of /etc/passwd] ***********************************************************************************************************************************
ok: [localhost]

TASK [debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "passwdStatus.stat.exists": true
}

TASK [Stat of /etc] ******************************************************************************************************************************************
ok: [localhost]

TASK [debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "etcStatus.stat.exists": true
}

TASK [Stat of /unknown] **************************************************************************************************************************************
ok: [localhost]

TASK [debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "unknownStatus.stat.exists": false
}

PLAY RECAP ***************************************************************************************************************************************************
localhost                  : ok=7    changed=0    unreachable=0    failed=0  
```

## To delete the directory contents only


```python
#file module is used here to delete the entire directory and then create new directory with same name.

---
- hosts: localhost
  tasks: '
    - name: Deleting only /tmp/bin Contents'
      file: path=/tmp/bin state="{{item}}"
      with_items:
        - absent
        - directory
  
```

## To get the kernel status


```python
---
- hosts: localhost
  tasks:
    - name: 'Getting the kernel version'
      shell: 'uname -r'
      register: kernelStatus
    
    - debug: var=kernelStatus
```


```python
[ec2-user@ip-172-31-22-216 playbooks]$ ansible-playbook kernel-status.yml 

PLAY [localhost] *********************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [localhost]

TASK [Getting the kernel version] ****************************************************************************************************************************
changed: [localhost]

TASK [debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "kernelStatus": {
        "changed": true, 
        "cmd": "uname -r", 
        "delta": "0:00:00.003425", 
        "end": "2019-06-20 03:53:00.860280", 
        "failed": false, 
        "rc": 0, 
        "start": "2019-06-20 03:53:00.856855", 
        "stderr": "", 
        "stderr_lines": [], 
        "stdout": "4.14.114-105.126.amzn2.x86_64", 
        "stdout_lines": [
            "4.14.114-105.126.amzn2.x86_64"
        ]
    }
}

PLAY RECAP ***************************************************************************************************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0   
```

# Ansible Roles


```python
#The concept of an Ansible role is simple; it is a group of variables, tasks, files, and handlers that are stored in a standardized file structure.
```

## Different parts of a playbook


```python
- tasks.
- Variables.
- Files.
- Jinja Templates.
- Handlers.
```


```python
#Default path of a role.

/etc/ansible/roles
```


```python
#Uncomment roles_path in ansible.cfg to enable ansible roles

[ec2-user@ip-172-31-22-216 wordpress-project]$ grep roles_path /etc/ansible/ansible.cfg 
roles_path    = /etc/ansible/roles
[ec2-user@ip-172-31-22-216 wordpress-project]$ 
```

## Creating an empty role


```python
[ec2-user@ip-172-31-22-216 playbooks]$ cd /etc/ansible/roles/
[ec2-user@ip-172-31-22-216 roles]$ ls



[ec2-user@ip-172-31-22-216 roles]$ sudo ansible-galaxy init lamp #Command to initialize a role named lamp
- lamp was created successfully # A directory named lamp will be created.
[ec2-user@ip-172-31-22-216 roles]$ ls
lamp
[ec2-user@ip-172-31-22-216 roles]$ cd lamp/
[ec2-user@ip-172-31-22-216 lamp]$ ls #default directories will be created and default files will also be automatically created.
defaults  files  handlers  meta  README.md  tasks  templates  tests  vars

#Below is the tree of lamp directory.

[ec2-user@ip-172-31-22-216 lamp]$ tree
.
|-- defaults
|   `-- main.yml
|-- files
|-- handlers
|   `-- main.yml
|-- meta
|   `-- main.yml
|-- README.md
|-- tasks
|   `-- main.yml
|-- templates
|-- tests
|   |-- inventory
|   `-- test.yml
`-- vars
    `-- main.yml

8 directories, 8 files
```


```python
#sudo -i

#All the variables will be written inside the main.yml of vars directory

# vim lamp/vars/main.yml

---

domain: www.example.com
mysql_root: mysqlroot123
wordpress_user: wordpress
wordpress_password: wordpress123
wordpress_database: wordpress

```


```python
#Files which needs to Created should be placed inside the files directory.

#vim lamp/files/info.html

<h1><center>info.html is working</center></h1>


$vim lamp/files/info.php

<?php phpinfo(); ?>
```


```python
#Template files should be created under templates directory

#virtualhost.j2
#wp-config.php.j2
```


```python
#Tasks can be splitted and written seperate yml files.

#Here we create apache.yml for installing httpd and its requied files.

# vim lamp/task/apache.yml

---

- name: 'LampStack - Installation'
  yum:
    name:
      - httpd
      - php
      - php-mysql
      - mariadb-server
      - MySQL-python
    state: present
      
- name: 'LampStack - Restarting/Enabling'
  service:
    name: "{{item}}"
    state: restarted
    enabled: yes
  with_items:
    - mariadb
    - httpd
      
- name: 'LampStack - Creating Virtualhost'
  template:
    src: virtualhost.j2
    dest: /etc/httpd/conf.d/{{domain}}.conf
      
- name: 'LampStack - Creating Documentroot'
  file:
    path: /var/www/html/{{domain}}
    state: directory  
      
      
- name: 'LampStack - Creating info.html'
  copy:
    src: info.html
    dest: /var/www/html/{{domain}}/info.html      
      
- name: 'LampStack - Creating info.php'
  copy:
    src: info.php
    dest: /var/www/html/{{domain}}/info.php

```


```python
#Now we write another playbook to download and install Wordpress, configure mysql etc.

#So basically, different tasks can be splitted into different yml files for identification.

$vim lamp/tasks/mariadb.yml

---
- name: 'MariaDb - Resetting Root Password'
  mysql_user:
    login_user: root
    login_password: ''
    name: root
    host_all: yes
    password: "{{ mysql_root }}"
      
- name: 'MariaDb - Removing Anonymous Users'
  mysql_user:
    login_user: root
    login_password: "{{ mysql_root }}"
    name: ''
    host_all: yes
    state: absent
      
- name: 'MariaDb - Creating Wordpress Database'
  mysql_db:
    login_user: root
    login_password: "{{ mysql_root }}"
    db: "{{ wordpress_database }}"
    state: present
      
      
- name: 'MariaDb - Creating WordPress User and Privileges'
  mysql_user:
    login_user: root
    login_password: "{{ mysql_root }}"
    name: "{{ wordpress_user }}"
    host: localhost
    password: "{{wordpress_password}}"
    priv: "{{wordpress_database}}.*:ALL"
      
- name: 'Wordpress - Downloading'
  get_url:
    url: https://wordpress.org/wordpress-4.5.8.tar.gz
    dest: /tmp/wordpress.tar.gz
      
- name: 'Wordpress - Extraction'
  unarchive:
    src: /tmp/wordpress.tar.gz
    dest: /tmp/
    remote_src: yes

- name: 'Wordpress - Copying Wordpress To DocumentRoot'
  shell: 'cp -r /tmp/wordpress/*  /var/www/html/{{domain}}'
    
- name: 'Wordpress - Creating wp-config.php'
  template:
    src: wp-config.php.j2
    dest: /var/www/html/{{domain}}/wp-config.php
      
```


```python
$vim lamp/tasks/post-install.yml

---
- name: 'LampStack - Post Restarting'
  service:
    name: "{{item}}"
    state: restarted
  with_items:
    - mariadb
    - httpd
```


```python
:
$vim lamp/templates/wp-config.php.j2

<?php

define( 'DB_NAME', '{{wordpress_database}}' );
define( 'DB_USER', '{{wordpress_user}}' );
define( 'DB_PASSWORD', '{{wordpress_password}}' );
define( 'DB_HOST', 'localhost' );
define( 'DB_CHARSET', 'utf8' );
define( 'DB_COLLATE', '' );

define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );

$table_prefix = 'wp_';

define( 'WP_DEBUG', false );


if ( ! defined( 'ABSPATH' ) ) {
	define( 'ABSPATH', dirname( __FILE__ ) . '/' );
}

require_once( ABSPATH . 'wp-settings.php' );
```


```python
#Now inside main.yml include_taks module is used to call different playbooks written in the tasks directory.


vim lamp/tasks/main.yml

---
- include_tasks: apache.yml
- include_tasks: mariadb.yml
- include_tasks: post-install.yml
```


```python
#Result

[ec2-user@ip-172-31-22-216 playbooks]$ ansible-playbook play-book-roles-demo.yml 

PLAY [amazon] ************************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [172.31.18.26]

TASK [lamp : include_tasks] **********************************************************************************************************************************
included: /etc/ansible/roles/lamp/tasks/apache.yml for 172.31.18.26

TASK [lamp : LampStack - Installation] ***********************************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : LampStack - Restarting/Enabling] ****************************************************************************************************************
changed: [172.31.18.26] => (item=mariadb)
changed: [172.31.18.26] => (item=httpd)

TASK [lamp : LampStack - Creating Virtualhost] ***************************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : LampStack - Creating Documentroot] **************************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : LampStack - Creating info.html] *****************************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : LampStack - Creating info.php] ******************************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : include_tasks] **********************************************************************************************************************************
included: /etc/ansible/roles/lamp/tasks/mariadb.yml for 172.31.18.26

TASK [lamp : MariaDb - Resetting Root Password] **************************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : MariaDb - Removing Anonymous Users] *************************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : MariaDb - Creating Wordpress Database] **********************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : MariaDb - Creating WordPress User and Privileges] ***********************************************************************************************
changed: [172.31.18.26]

TASK [lamp : Wordpress - Downloading] ************************************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : Wordpress - Extraction] *************************************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : Wordpress - Copying Wordpress To DocumentRoot] **************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : Wordpress - Creating wp-config.php] *************************************************************************************************************
changed: [172.31.18.26]

TASK [lamp : include_tasks] **********************************************************************************************************************************
included: /etc/ansible/roles/lamp/tasks/post-install.yml for 172.31.18.26

TASK [lamp : LampStack - Post Restarting] ********************************************************************************************************************
changed: [172.31.18.26] => (item=mariadb)
changed: [172.31.18.26] => (item=httpd)

PLAY RECAP ***************************************************************************************************************************************************
172.31.18.26               : ok=19   changed=15   unreachable=0    failed=0   
```


```python

```

==============================================================================================================================================================


# Deploying a repo from Git.


```python
[ec2-user@ip-172-31-22-216 git-deployment]$ cat main.yml 
---
- name: Deploying website from Git repository
  hosts: all
  become: yes
  tasks:
    - name: Installing softwares
      yum: name=git,httpd state=present

    - name: Creating local code repository
      file: path=/var/git-repo state=absent

    - name: Creating local code repository
      file: path=/var/git-repo state=directory mode=700 owner=root group=root
    
    - name: Cloning latest commit from git
      git: repo=https://github.com/syamj/ansible-git.git dest=/var/git-repo
      register: repo_download

    - name: Copying contents to Docroot
      shell: 'cp -r /var/git-repo/* /var/www/html/'
      when: repo_download.changed == true

    - name: Ensure directories are set to apache.apache
      shell: find /var/www/html -exec chown apache:apache {} \;

    - name: Ensure directories are set to 755
      shell: find /var/www/html -type d -exec chmod 0755 {} \;

    - name: Ensure files are 644 
      shell: find /var/www/html -type f -exec chmod 0644 {} \;

    - name: restart softwares
      service: name=httpd state=restarted enabled=yes
      when: repo_download.changed
```

# Read value from csv file


```python
- hosts: localhost
  become: yes
  tasks:
    - debug: msg="The default shell of Syam is {{ lookup('csvfile', 'syam file=users.txt delimiter=,') }}"

[ec2-user@ip-172-31-22-216 playbooks]$ cat users.txt 
john,/bin/bash
doe,/bin/sh
sam,/sbin/nologin
syam,/bin/bash
fuji,/bin/sh
[ec2-user@ip-172-31-22-216 playbooks]$ 
```


```python
[ec2-user@ip-172-31-22-216 playbooks]$ ansible-playbook shell-check.yml 

PLAY [localhost] *********************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [localhost]

TASK [debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "The default shell of Syam is /bin/bash"
}

PLAY RECAP ***************************************************************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0   
```

# To Add multiple Users to a remote machine



```python

[ec2-user@ip-172-31-22-216 playbooks]$ cat multiple-user-add.yml 
- hosts: amazon
  become: yes
  tasks:
    - shell: cat users.txt
      register: content
      delegate_to: localhost
    
    - user: name="{{item.split(',')[0]}}" shell="{{item.split(',')[1]}}"
      with_items: 
        - "{{ content.stdout.split() }}"
[ec2-user@ip-172-31-22-216 playbooks]$ 
```