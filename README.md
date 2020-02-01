## ansible_beginner_handson
Exercise from course [**Ansible for the Absolute Beginner - Hands-On - DevOps**](https://www.udemy.com/course/learn-ansible/learn/lecture/11060796#overview)


## Introduction
Follow [this page](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) to install ansible:
```console
# ansible --version
ansible 2.9.1
config file = /etc/ansible/ansible.cfg
...
```

## Ansible Inventory
For introduction on Ansible Inventory, please access the [course link here](https://www.udemy.com/course/learn-ansible/learn/lecture/7133366#overview). The default inventory file is under `/etc/ansible/hosts`.


## What's YAML
This [section](https://www.udemy.com/course/learn-ansible/learn/lecture/7133354#overview) introduce you what's YAML and how it works.

## Ansible Playbooks
This [section](https://www.udemy.com/course/learn-ansible/learn/lecture/11060794#overview) show you how to run an ansible playbook.

### Run Ansible Playbook
You can ping target with [**ansible**](https://docs.ansible.com/ansible/latest/cli/ansible.html) command as below:
```console
# ansible all -m ping -i inventory.txt
target2 | SUCCESS => {
"ansible_facts": {
"discovered_interpreter_python": "/usr/bin/python3"
},
"changed": false,
"ping": "pong"
}
target1 | SUCCESS => {
"ansible_facts": {
"discovered_interpreter_python": "/usr/bin/python3"
},
"changed": false,
"ping": "pong"
}
```
Or you can create a playbook `ansible-demo-exercises/exercise_pingtest/playbook-pingtest.yml`:
```YAML
-  
    name: Connection testing  
    hosts: all  
    tasks:  
        - name: Ping test  
          ping:  
```
Then you can ping all targets by command "ansible-playbook":
```console
$ ansible-playbook ansible-demo-exercises/exercise_pingtest/playbook-pingtest.yml -i ansible-demo-exercises/inventory/test_targets.txt
...
PLAY RECAP ************************
target1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
target2                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
### Tips & Tricks about Atom IDE with Ansible
This [class](https://www.udemy.com/course/learn-ansible/learn/lecture/11060796#overview) will introduce you how to use [**IDE Atom**](https://atom.io/) to speed up the development process. Our first exercise here is to create a play book [`ansible-demo-exercises/exercise-1-copyfile/playbook-copyfile.yaml`](https://github.com/johnklee/ansible_beginner_handson/blob/master/ansible-demo-exercises/exercise-1-copyfile/playbook-copyfile.yaml) to leverage [**copy module**](https://docs.ansible.com/ansible/latest/modules/copy_module.html) to copy file `test.txt` to each ansible node under path `/etc/test.txt`:
```console
$ cd ansible-demo-exercises/exercise-1-copyfile/
$ ansible-playbook playbook-copyfile.yaml -i ../inventory/test_targets.txt
...
PLAY RECAP ************
target1                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
target2                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

$ sudo docker exec -it ansible_target1 cat /etc/test.txt
This is an testing file for copy module
```

## Ansible Modules
This [section](https://www.udemy.com/course/learn-ansible/learn/lecture/7133378#overview) will introduction many useful ansible modules ([Module Index](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html)):
* [**System module**](https://docs.ansible.com/ansible/latest/modules/list_of_system_modules.html)
* [**Commands module**](https://docs.ansible.com/ansible/latest/modules/list_of_commands_modules.html)
* [**Files module**](https://docs.ansible.com/ansible/latest/modules/list_of_files_modules.html)
* [**Database module**](https://docs.ansible.com/ansible/latest/modules/list_of_database_modules.html)
* [**Cloud module**](https://docs.ansible.com/ansible/latest/modules/list_of_cloud_modules.html)
* [**Windows module**](https://docs.ansible.com/ansible/latest/modules/list_of_windows_modules.html)
* [**More..**](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html)

One important concept introduced here is `idempotent`:
```
The principle that enables Ansible to be declarative and yet reliable is idempotence, a concept borrowed from mathematics. An idempotent operation is one that can be applied multiple times without changing the result beyond the initial application, such as multiplication by zero. Ansible modules are idempotent
```

### Script module
[**script module**](https://docs.ansible.com/ansible/latest/modules/script_module.html) – Runs a local script on a remote node after transferring it. e.g.:
```YAML
-
    name: 'Execute a script on all web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Execute a script on all web server nodes'
            script: /tmp/install_script.sh
```
### Service module
[**service**](https://docs.ansible.com/ansible/latest/modules/service_module.html) – Manage services. e.g.:
```YAML
-
    name: 'Execute a script on all web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Execute a script on all web server nodes'
            script: /tmp/install_script.sh

        -
            name: 'Start httpd services'
            service:
                name: httpd
                state: started
```
### lineinfile module
[**lineinfile**](https://docs.ansible.com/ansible/2.5/modules/lineinfile_module.html) - Manage lines in text files. e.g.:
```YAML
-
    name: 'Execute a script on all web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Add an entry into /etc/resolv.conf'
            lineinfile:
                path: '/etc/resolv.conf'
                line: 'nameserver 10.1.250.10'
        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service:
                name: httpd
                state: present
```
### user module
[**user**](https://docs.ansible.com/ansible/latest/modules/user_module.html) – Manage user accounts. e.g.:
```YAML
-
    name: 'Execute a script on all web server nodes and start httpd service'
    hosts: web_nodes
    tasks:
        -
            name: 'Update entry into /etc/resolv.conf'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'
        -
            name: 'Created new web user'
            user:
                name: 'web_user'
                uid: 1040
                group: developers
        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service:
                name: httpd
                state: present
```
## Ansible Variables
This [section](https://www.udemy.com/course/learn-ansible/learn/lecture/7133380#overview) introduce variable used in Ansible.
* Stores information that varies with each host. e.g.:
```YAML
-
  name: Add DNS server to resolve.config
  hosts: localhost
  vars:
    dns_server: 10.1.250.10
  tasks:
    - lineinfile:
      path: /etc/resolve.conf
      line: 'nameserver {{ dns_server }}'
```

* Keep variables in inventory file and refer to them in playbook. e.g.:
inventory file:
```
# Sample Inventory File

localhost ansible_connection=localhost nameserver_ip=10.1.250.10 snmp_port=160-161
```
Then is playbook:
```YAML
-
    name: 'Update nameserver entry into resolv.conf file on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Update nameserver entry into resolv.conf file'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver {{ nameserver_ip }}'
        -
            name: 'Disable SNMP Port'
            firewalld:
                port: {{ snmp_port }}
                permanent: true
                state: disabled
```
## Conditionals
This [section](https://www.udemy.com/course/learn-ansible/learn/lecture/7133384#overview) will introduce conditionals concept in Ansible.
