# Troubleshooting

## Alias setting for ssh ask-pass
Add the following to your .bashrc file.

## ssh agent
```
alias ssha='eval $(ssh-agent) && ssh-add'
```


## When ansible gives a timeout on a connection during sudo 

Ansible gives the following error during a --become session
"msg": "Task failed: Timeout (12s) waiting for privilege escalation prompt:"

This is caused by the new behaviour of sudo in ubuntu 26.04 and up.
This can be avoided by setting an extra parameter
```
export ANSIBLE_BECOME_EXE=sudo.ws
```

Example:


```
gej@aten:~/ansible$ ansible all -m apt -a update_cache=true --become --ask-become-pass
BECOME password: 
[ERROR]: Task failed: Timeout (12s) waiting for privilege escalation prompt:
Origin: <adhoc 'apt' task>

{'action': 'apt', 'args': {'update_cache': 'true'}, 'timeout': 0, 'async_val': 0, 'poll': 15}

usa.fritz.box | UNREACHABLE! => {
    "changed": false,
    "msg": "Task failed: Timeout (12s) waiting for privilege escalation prompt:",
    "unreachable": true
}
usb.fritz.box | UNREACHABLE! => {
    "changed": false,
    "msg": "Task failed: Timeout (12s) waiting for privilege escalation prompt:",
    "unreachable": true
}
usc.fritz.box | UNREACHABLE! => {
    "changed": false,
    "msg": "Task failed: Timeout (12s) waiting for privilege escalation prompt:",
    "unreachable": true
}
gej@aten:~/ansible$ ansible --version
ansible [core 2.20.1]
  config file = /home/gej/ansible/ansible.cfg
  configured module search path = ['/home/gej/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/gej/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.14.4 (main, Aug 20 2026, 10:41:58) [GCC 15.2.0] (/usr/bin/python3)
  jinja version = 3.1.6
  pyyaml version = 6.0.3 (with libyaml v0.2.5)
gej@aten:~/ansible$ export ANSIBLE_BECOME_EXE=sudo.ws
gej@aten:~/ansible$ ansible all -m apt -a update_cache=true --become --ask-become-pass
BECOME password: 
usa.fritz.box | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.14"
    },
    "cache_update_time": 1793379021,
    "cache_updated": true,
    "changed": true
}
usb.fritz.box | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.14"
    },
    "cache_update_time": 1793379022,
    "cache_updated": true,
    "changed": true
}
usc.fritz.box | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.14"
    },
    "cache_update_time": 1793379022,
    "cache_updated": true,
    "changed": true
}
gej@aten:~/ansible$ 
```

Bugreport:
https://github.com/ansible/ansible/issues/85837

## error message: Depreciation warning inject_facts_as_vars
change
```
when: ansible_distribution == "Ubuntu"
```
to
```
when: ansible_facts["distribution"] == "Ubuntu"
```

