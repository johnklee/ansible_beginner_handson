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
