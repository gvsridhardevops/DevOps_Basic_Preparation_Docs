######################################################################
## Tomas Nevar <tomas@lisenet.com>
## Study notes for EX407 Ansible Automation exam (RHEL7)
######################################################################

## Exam objectives:

* Understand core components of Ansible
  - Inventories
  - Modules
  - Variables
  - Facts
  - Plays
  - Playbooks
  - Configuration files
* Install and configure an Ansible control node
  - Install required packages
  - Create a static host inventory file
  - Create a configuration file
* Configure Ansible managed nodes
  - Create and distribute SSH keys to managed nodes
  - Configure privilege escalation on managed nodes
  - Validate a working configuration using ad-hoc Ansible commands
* Create simple shell scripts that run ad hoc Ansible commands
* Use both static and dynamic inventories to define groups of hosts
* Utilize an existing dynamic inventory script
* Create Ansible plays and playbooks
  - Know how to work with commonly used Ansible modules
  - Use variables to retrieve the results of running commands
  - Use conditionals to control play execution
  - Configure error handling
  - Create playbooks to configure systems to a specified state
* Use Ansible modules for system administration tasks that work with:
  - Software packages and repositories
  - Services
  - Firewall rules
  - File systems
  - Storage devices
  - File content
  - Archiving
  - Scheduled tasks
  - Security
  - Users and groups
* Create and use templates to create customized configuration files
* Work with Ansible variables and facts
* Create and work with roles
* Download roles from an Ansible Galaxy and use them
* Manage parallelism
* Use Ansible Vault in playbooks to protect sensitive data
* Use provided documentation to look up specific information about Ansible modules and commands

## When installed, the ansible package provides a base configuration
## file located at /etc/ansible/ansible.cfg
##
## Priority in which the configuration files are processed:
##  1) $ANSIBLE_CONFIG (an environment variable)
##  2) ./ansible.cfg (in the current directory)
##  3) ~/.ansible.cfg (the user's home directory)
##  4) /etc/ansible/ansible.cfg

#---------------------------------------------------------------------

## *** Ansible Get Started **

## To find out what config file is in use, run the following:

$ ansible --version
ansible 2.7.9
  config file = /home/ansible/ansible.cfg

## To get started, copy configuration files to the home directory:

$ cp /etc/ansible/ansible.cfg ~/ansible.cfg
$ cp /etc/ansible/hosts ~/inventory

## Note: the maximum number of simultaneous connections that Ansible
## makes is controlled by the forks parameter (ansible.cfg):

forks 5

## Example inventory configuration:

$ cat ~/inventory
[myself]
127.0.0.1 ansible_connection=local
[web]
ansible[2:3].hl.local
[db]
ansible[4:5].hl.local ansible_user=dbadmin
[lab:children]
web
db

## There is a special group named "all" that matches all managed hosts
## in the inventory.

- hosts: all

## There is also a special group named "ungrouped" which matches all
## managed hosts in the inventory that are not members of any group.

- hosts: ungrouped

## Quote host patterns used on the CLI to protect them from unwanted
## shell expansion:

- hosts: '*.hl.local'
- hosts: '10.11.1.*'

## Multiple entries in the inventory can be referenced using lists:

- hosts: web,ansible3.hl.local

## If you have static and dynamic inventory files in the same
## directory, then they are merged and treated as one inventory!

## Show inventory for all:

$ ansible "*" -i ~/inventory --list-hosts


## Privilege escalation settings:

$ vim ansible.cfg
[privilege_escalation]
become = false
become_method = sudo
become_user = root
become_ask_pass = false


## How to set the default user to use for playbooks:

$ vim ansible.cfg
remote_user = ansible

## How to set the log file:

$ vim ansible.cfg
log_path = /home/ansible/ansible.log


## If no module is defined, Ansible uses the internally predefined
## "command" module. Note that this is not a shell!
## Ad hoc command that generates one line input for each operation:

$ ansible all -m command -a /usr/bin/hostname -o

## Use the copy module to change content of a file:

$ ansible all -m copy \
  -a 'content="Host managed by Ansible\n" dest=/etc/motd' \
  -u ansible --become

## NOTE: if possible, try to avoid the command, shell and raw modules
## in playbooks! It's easy to write non-idempotent playbooks this way.

## Ansible module documentation and playbook snippets:

$ ansible-doc -l
$ ansible-doc -s <module_name>
$ ansible-doc <module_name>


## Ansible executes plays and tasks in the order they are presented!
## How to check for YAML syntax errors:

$ ansible-playbook --syntax-check <filename>.yaml

## Which host(s) the playbook applies to?

$ ansible-playbook <playbook>.yaml --list-hosts

## Whats tasks will be performed?

$ ansible-playbook <playbook>.yaml --list-tasks

## Run one task at a time:

$ ansible-playbook <playbook>.yaml --step

## Playbook dry-run:

$ ansible-playbook -C <playbook>.yaml

## Playbook step-by-step execution:

$ ansible-playbook --step <playbook>.yaml


## The beginning of each play begins with a single dash followed by a space.
## Playbook attributes:
##
## name - can be used to give a descriptive label to a play.
## hosts - must be defined in every play.
## remote_user - can be used to define the user that runs the tasks.
## become - can be used to enable privilege escalation.
## become_method - can be used to define the privilege escalation method.
## become_user - can define the user account to be used for privilege escalation.
## tasks - is defined as a list of dictionaries.
## blocks - can be used to group related tasks together.

## For each play in a playbook, you get to choose which machines in your
## infrastructure to target and what remote user to complete the steps as.
## You can use keyword "become" on a particular task instead of the play:

---
- hosts: webservers
  remote_user: ansible
  serial: 2
  tasks:
    - service:
        name: httpd
        state: started
      become: yes
      become_method: sudo

## Use the serial keyword to run the hosts through the play in batches.
## Host variables take precedence over group variables, but variables
## defined by a playbook take precedence over both.

#---------------------------------------------------------------------

## ** Including and Importing Files **

## When you include content, then Ansible processes included content
## during the run of the playbook, as content is required.

## When you import content, Ansible processes imported content when
## the playbook is initially read, before the run starts.

## Note: "include" was replaced in Ansible 2.4 with new directives such
## as include_tasks, import_tasks, and import_playbook.
## These can be used to enhance the ability to reuse tasks and playbooks:

import_playbook:
import_tasks:
include_tasks:

## Examples:

tasks:
  - name: Import task file and use variables
    import_tasks: task.yml
    vars:
      package: vsftpd
      service: vsft


  - name: Install the {{ package }} package
    yum:
      name: "{{ package }}"
      state: latest
  - name: Start the {{ service }} service
    service:
      name: "{{ service }}"
      enabled: true
      state: started

#---------------------------------------------------------------------

## *** Ansible and VIM ***

## When writing playbooks in vim editor, modify its action in response
## to Tab key entries. Add the following line to $HOME/.vimrc.
## It will perform a two space indentation when the Tab key is pressed.

autocmd FileType yaml setlocal ai ts=2 sw=2 et

## Settings explained:

ai   - autoindent (turns it on)
sw=2 - shiftwidth (indenting is 2 spaces)
et   - expandtab (do not use actual tab character)
ts=2 - tabstop (tabs are at proper location)

## Two other helpful but optional vim settings:

nu (show line number)
cursorline (line to show current cursor position)

#---------------------------------------------------------------------

## *** Ansible Vault ***

$ ansible-vault [create|decrypt|edit|encrypt|rekey|view] vaultfile.yml

## By default, Ansible uses functions from the python-crypto package to
## encrypt and decrypt vault files. To speed up decryption at startup,
## you install the python-cryptography package.

$ ansible-vault create vaultfile.yml
$ ansible-vault view vaultfile.yml

## Check syntax of a playbook that uses the vault file:

$ ansible-playbook --syntax-check --ask-vault-pass playbook.yml

## Create a password file to use for the playbook execution:

$ echo "ansible" > key
$ ansible-playbook --syntax-check --vault-password-file=key playbook.yml

#---------------------------------------------------------------------

## *** Ansible Facts ***
## The same thing as facts in Puppet. How to print facts for all hosts?

- name: Ansible fact dump
  hosts: all
  tasks:
    - name: Print all Ansible facts
      debug:
        var: ansible_facts

- name: Ansible package fact dump
  hosts: all
  tasks:
    - name: Gather package facts
      package_facts:
        manager: auto

## Before Ansible 2.5, facts were injected as individual variables
## prefixed with the string "ansible_" instead of being part of the
## ansible_facts variable.

## At the time of writing this, Ansible recognises both the new fact
## naming system (using ansible_facts) and the old pre 2.5 naming system
## where facts are injected as separate variables.

## You can use an ad hoc command to run the setup module to print the
## value of all facts:

$ ansible localhost -m setup

## Filter results:

$ ansible localhost -m setup -a 'filter=*distribution*'

## Some commonly used facts (from my experience with Puppet).
## In no particular order:

ansible_facts['hostname']
ansible_facts['fqdn']
ansible_facts['default_ipv4']['address']
ansible_facts['distribution']
ansible_facts['distribution_major_version']
ansible_facts['domain']
ansible_facts['memtotal_mb']
ansible_facts['processor_count']


## To disable fact gathering for a play, you can set the gather_facts
## keyword to "no":

gather_facts: no

## Similar to Puppet, Ansible supports custom facts. Custom facts can be
## defined in a static file, formatted as an INI file or using JSON and
## placed in /etc/ansible/facts.d and the file name must end in ".fact".
## They can also be executable scripts which generate JSON output,
## just like a dynamic inventory script.
## Note: custom fact files cannot be in YAML format!

#---------------------------------------------------------------------

## *** Ansible Loops, Conditionals and Lookups ***

## Ansible supports iterating a task over a set of items using the loop
## keyword. A simple loop iterates a task over a list of items.
## Since Ansible 2.5, the recommended way to write loops is to use the
## loop keyword. The old syntax used loops prefixed with "with_".

- name: Ensure that packages are present
  package:
    name: "{{ item }}"
    state: present
  loop:
    - firewalld
    - httpd
    - vsftpd

- name: Ensure that web server ports are open
  firewalld:
    service: "{{ item }}"
    immediate: true
    permanent: true
    state: enabled
  loop:
    - http
    - https

## Example conditionals:

Equal				|	==
Less than			|	<
Greater than 			|	>
Variable exists 		|	is defined
Variable does not exist		|	is not defined
Boolean is true			|	values of 1, True, or yes evaluate to true

## Logical AND and OR operations are supported:

when: ansible_domain == "hl.local" and ansible_distribution == "RedHat"
when: ansible_processor_cores == "1" or ansible_processor_cores == "2"

## Loops and conditionals can be combined:

- name: Ensure that packages are present on RedHat
  package:
    name: "{{ item }}"
    state: present
  loop:
    - firewalld
    - httpd
  when: ansible_distribution == "RedHat"

## Lookup plugins allow access to outside data sources. Lookups occur
## on the local computer, not on the remote computer.
## One way of using lookups is to populate variables:

vars:
  motd_value: "{{ lookup('file', '/etc/motd') }}"
tasks:
  - debug:
      msg: "motd value is {{ motd_value }}"

## *** Ansible Handlers ***
## Handlers always run in the order specified by the handlers section
## of the play. A handler called by a task in the tasks part of the
## playbook will not run until all of the tasks under tasks have been
## processed.

notify: restart httpd service

handlers:
  - name: restart httpd service
    service:
      name: httpd
      state: restarted

## If a task fails and the play aborts on that host, any handlers that
## had been notified by earlier tasks in the play will not run.
## Use the following to force execution of the handler:

force_handlers: yes

## Handlers are notified when a task reports a "changed" result.
## Handlers are not notified when it reports an "ok" or "failed" result.

## Ignore errors:

ignore_errors: yes

#---------------------------------------------------------------------

## *** Commonly Used Files Modules with Examples ***

## The file module acts like chcon when setting file contexts:

- name: Create a file
  hosts: localhost
  become: true
  tasks:
    - name: Create a file and set permissions
      file:
        path: /root/file
        owner: root
        group: root
        mode: 0640
        state: touch
        setype: user_tmp_t
    - name: SELinux type is persistently set to user_tmp_t
      sefcontext:
        target: /root/file
        setype: user_tmp_t
        state: present


- name: Copy a file
  hosts: localhost
  become: true
  tasks:
    - name: Copy a file
      copy:
        src: file
        dest: /root/file
        force: yes
        owner: root
        group: ansible
        mode: 0640


- name: Add a line to a file
  hosts: localhost
  become: true
  tasks:
    - name: Add a new line to a file
      lineinfile:
        path: /root/file
        line: "this is the new line"
        state: present


- name: Add block of text to a file
  hosts: localhost
  become: true
  tasks:
    - name: Add a block of text to an existing file
      blockinfile:
        path: /root/file
        block: |
          This is the first line to be added.
          This is the second line to be added.
        state: present


- name: Download a file from managed hosts
  hosts: localhost
  become: false
  tasks:
    - name: Fetch a file
      fetch:
        src: /etc/hosts
        dest: my-folder
        flat: no

- name: Download a file from remote host
  hosts: localhost
  become: false
    - name: Download file from URL
      get_url:
        url: http://katello.hl.local/pub/index.html
        dest: /tmp/index.html

- name: Disable SSH root login
  hosts: localhost
  become: true
  tasks:
  - name: Modify SSH server config
    lineinfile:
      dest: "/etc/ssh/sshd_config"
      regexp: "^PermitRootLogin"
      line: "PermitRootLogin no"

#---------------------------------------------------------------------

## How to generate SHA512 crypted passwords for the user module?
## The answer is taken from Ansible FAQ.

- name: Create user with password
    user:
      name: sandy
      password: "{{ user_pw | password_hash('sha512') }}"

#---------------------------------------------------------------------

## *** Jinja2 Templates ***

## Similar to Puppet. Puppet templates are based upon Ruby's ERB,
## Ansible templates are based upon Jinja2.

## A file containing a Jinja2 template does not need to have any
## specific file extension.

## Use the template module to deploy it:

- name: Use a template file
  hosts: localhost
  gather_facts: yes
  tasks:
    - name: Deploy index.html
      template:
        src: templates/index.j2
        dest: /var/www/html/index.html

$ cat templates/index.j2
The system kernel is: {{ ansible_kernel }}

#---------------------------------------------------------------------

## *** Ansible Roles ***

## Similar to Puppet modules, reusable code in a modular fashion.
## Create a role skeleton:

$ mkdir roles && cd roles
$ ansible-galaxy init my_test_role

$ tree my_test_role/
my_test_role/
????????? defaults
???   ????????? main.yml
????????? files
????????? handlers
???   ????????? main.yml
????????? meta
???   ????????? main.yml
????????? README.md
????????? tasks
???   ????????? main.yml
????????? templates
????????? tests
???   ????????? inventory
???   ????????? test.yml
????????? vars
    ????????? main.yml

## As of Ansible 2.4, you can use roles inline with any other tasks
## using import_role or include_role:

---
- hosts: webservers
  tasks:
  - debug:
      msg: "before we run our role"
  - import_role:
      name: example
  - include_role:
      name: example
  - debug:
      msg: "after we ran our role"

## Search for roles from the CLI:

$ ansible-galaxy search --author redhat
$ ansible-galaxy search --galaxy-tags tomcat
$ ansible-galaxy search tomcat --platforms EL

## Ansible Galaxy is a public library of Ansible roles written by users.
## Similar to Puppet Forge. Install a role from Galaxy:

$ ansible-galaxy install <author.name> -p ~/.ansible/roles

## List installed roles:

$ ansible-galaxy list -p ~/.ansible/roles/
- <author.name>, 1.9.5

## To define role source, use requirements.yml:

- src: http://katello.hl.local/pub/ansible-haproxy.tar.gz
  name: haproxy
- src: http://katello.hl.local/pub/ansible-mysql.tar.gz
  name: mysql

$ ansible-galaxy init -r requirements.yml

## The original way to use roles is via the roles: option for a play:

---
- hosts: webservers
  roles:
     - webservers

## The order of execution for your playbook is as follows:

1. Any pre_tasks defined in the play.
2. Any handlers triggered so far will be run.
3. Each role listed in roles will execute in turn (with dependencies).
4. Any tasks defined in the play.
5. Any handlers triggered so far will be run.
6. Any post_tasks defined in the play.
7. Any handlers triggered so far will be run.

## Role dependencies are always executed before the role that includes
## them, and may be recursive.

## RHEL system roles:

$ sudo yum install rhel-system-roles
$ ls /usr/share/doc/rhel-system-roles-1.0/
kdump
network
postfix
selinux
timesync

$ ansible-galaxy list
- rhel-system-roles.selinux, (unknown version)
- linux-system-roles.network, (unknown version)
- rhel-system-roles.postfix, (unknown version)
- rhel-system-roles.timesync, (unknown version)
- rhel-system-roles.kdump, (unknown version)
- linux-system-roles.timesync, (unknown version)
- linux-system-roles.kdump, (unknown version)
- linux-system-roles.postfix, (unknown version)
- rhel-system-roles.network, (unknown version)
- linux-system-roles.selinux, (unknown version)

#---------------------------------------------------------------------

## *** Ansible Variables ***

## Variables for hosts and host groups can be defined by creating two
## directories, group_vars and host_vars, in the same working directory
## as the inventory file or directory.

$ cat group_vars/webserver
my_package: httpd

$ cat group_vars/database
my_package: mariadb

## Whether or not you define any variables, you can access information
## about your hosts with the Special Variables Ansible provides,
## including "magic" variables, facts, and connection variables.

## The most commonly used magic variables are:

hostvars
groups
group_names
inventory_hostname

## The group_names variable contains a list of all the groups the
## current host is in. We can use it to install specific packages:

tasks:
  - name: Install webserver packages
    package:
      name: "{{ item }}"
      state: latest
    loop:
      - httpd
      - firewalld
    when: "'webserver' in group_names"

## The hostvars variable lets you access variables for another host,
## including facts that have been gathered about that host.

{{ hostvars['ansible2.hl.local']['ansible_default_ipv4']['address'] }}
{{ hostvars['ansible2.hl.local']['ansible_fqdn'] }}

## The inventory_hostname variable is the name of the hostname as
## configured in Ansible's inventory host file.
## The groups variable is a list of all the groups in the inventory.

when: inventory_hostname in groups["webservers"]

#---------------------------------------------------------------------

## *** meta - Execute Ansible actions ***

## Meta tasks are a special kind of task which can influence Ansible
## internal execution or state. Choices:

* flush_handlers
* refresh_inventory
* noop
* clear_facts
* clear_host_errors
* end_play
* reset_connection

## Example meta task for how to run handlers after an error occurred:

 tasks:
   - name: Attempt and graceful roll back demo
     block:
       - debug:
           msg: 'I execute normally'
         notify: run me even after an error
       - command: /bin/false
     rescue:
       - name: make sure all handlers run
         meta: flush_handlers
 handlers:
    - name: run me even after an error
      debug:
        msg: 'This handler runs even on error'

## Note: meta is not really a module as such it cannot be overwritten.

#---------------------------------------------------------------------

## *** Ansible Check Mode (Dry Run) ***

## When ansible-playbook is executed with --check it will not make any
## changes on remote systems. To modify the check mode behavior of
## individual tasks, you can use the check_mode option:

- name: this task will run under checkmode and not change the system
  lineinfile:
    line: "PermitRootLogin no"
    dest: /etc/ssh/sshd_config
    state: present
  check_mode: yes

## The above will force a task to run in check mode, even when the
## playbook is called without --check.

#---------------------------------------------------------------------

## *** Ansible Playbook Debugger ***

## Ansible includes a debugger as part of the strategy plugins.
## This debugger enables you to debug as task.

## The debugger keyword can be used on any block where you provide
## a name attribute, such as a play, role, block or task.

---
- hosts: ansible2.hl.local
  tasks:
    - unarchive:
        src: files/archive.tgz
        dest: /tmp/
        remote_src: no
    - name: archive file
      debugger: on_failed
      archive:
        path: /tmp/archive.html
        dest: /tmp/file.gz
        format: tgz

#---------------------------------------------------------------------

## *** Ansible Ignoring Failed Commands ***

## Playbooks will stop executing any more steps on a host that has a
## task fail. To ignore this behaviour, set "ignore_errors" to true:

- name: this will not be counted as a failure
  command: /bin/false
  ignore_errors: yes

## This is useful when you expect a task to fail. For example, you
## are checking if Apache website is reachable. It may be unreachable,
## but you don't want the play to fail because of that.

#---------------------------------------------------------------------

## *** Ansible Run Once ***

## There may be a need to only run a task one time for a batch of hosts:
## This can be achieved by configuring "run_once" on a task:

- command: /tmp/upgrade_database.sh
  run_once: true

#---------------------------------------------------------------------

## *** Ansible Aborting the Play ***

## There will be cases when you will need to abort the entire play on
## failure, not just skip remaining tasks for a host, to avoid breaking
## the system. The "any_errors_fatal" play option will mark all hosts
## as failed if any fails, causing an immediate abort:

- hosts: all
  any_errors_fatal: true
  roles:
    - database

#---------------------------------------------------------------------

## *** Ansible Keywords ***

## These are some of the keywords available on common playbook objects.

## Notable play keywords:

any_errors_fatal
become
become_method
become_user
check_mode
debugger
force_handlers
gather_facts
handlers
hosts
ignore_errors
name
no_log
order
port
post_tasks
pre_tasks
remote_user
roles
run_once
serial
tasks
vars
vars_files

## Notable block keywords:

always
block
rescue
when

## Notable task keywords:

loop
register

#---------------------------------------------------------------------

## *** Ansible Setting Defaults for Modules ***

## It can be useful to define default arguments for a particular module
## using the "module_defaults" attribute:

- hosts: all
  become: true
  module_defaults:
    file:
      owner: root
      group: devops
      mode: "664"
      state: touch
  tasks:
    - file:
        path: /tmp/file1
    - file:
        path: /tmp/file2
    - file:
        path: /tmp/file3

## It is a time saver when you need to use the same module repeatedly.

#---------------------------------------------------------------------

## *** Ansible Best Practices ***

## Tips for making the most of Ansible and Ansible playbooks.

1. Always mention the state. The "state" parameter is optional to a
lot of modules. Whether "state=present" or "state=absent", it is always
best to leave that parameter in your playbooks to make it clear.

2. Generous use of whitespace to break things up, and use of comments,
which start with "#", is encouraged.

3. Always name tasks. It is recommended to provide a description about
why something is being done!

4. Keep it simple. Do not attemtp to use every feature of Ansible
together, all at once. Use what works for you!

5. Ansible best practice has no limit on the amount of variable and
vault files or their names.