# ansible-playbooks


## 1. install ansible
https://docs.ansible.com/ansible/latest/installation_guide/index.html

https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html


## 2.setting
```
git clone https://github.com/roccqqck/ansible-playbooks
cd ansible-playbooks
```


edit your ```inventory``` about ip port username key

check list
```
ansible-inventory -i ./inventory --list
```
```
{
    "_meta": {
        "hostvars": {
            "debian1": {
                "ansible_ssh_host": "127.0.0.1",
                "ansible_ssh_port": 2222,
                "ansible_ssh_private_key_file": "/Users/barry/projects/github/vagrantfiles/debian11/.vagrant/machines/default/virtualbox/private_key",
                "ansible_user": "vagrant"
            },
            "debian2": {
                "ansible_ssh_host": "127.0.0.1",
                "ansible_ssh_port": 2200,
                "ansible_ssh_private_key_file": "/Users/barry/projects/github/vagrantfiles/debian11-2/.vagrant/machines/default/virtualbox/private_key",
                "ansible_user": "vagrant"
            }
        }
    },
    "all": {
        "children": [
            "group1",
            "ungrouped"
        ]
    },
    "group1": {
        "hosts": [
            "debian1",
            "debian2"
        ]
    }
}
```


Test connecting
```
ansible all -i ./inventory -m ping
```
```
debian1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
debian2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Test command
```
ansible all -i ./inventory -m command -a 'echo Hello World on Vagrant.'
```
```
debian1 | CHANGED | rc=0 >>
Hello World on Vagrant.
debian2 | CHANGED | rc=0 >>
Hello World on Vagrant.
```


edit ```ansible.cfg``` then you don't need to select inventory file
```
[defaults]

inventory = inventory
```

```
ansible-inventory --list
ansible all -m ping
ansible all -m command -a 'echo Hello World on Vagrant.'
```




## 3. run playbook


test (required ansible-lint installed)
```
ansible-lint ./PATH/playbook.yml
```

run
```
ansible-playbook ./PATH/playbook.yml
```
```
PLAY [group1] **************************************************************************

TASK [Gathering Facts] *****************************************************************
ok: [debian2]
ok: [debian1]

TASK [test connection] *****************************************************************
ok: [debian1]
ok: [debian2]

TASK [print debug message] *************************************************************
ok: [debian1] => {
    "msg": {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
}
ok: [debian2] => {
    "msg": {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
}

PLAY RECAP *****************************************************************************
debian1                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
debian2                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```