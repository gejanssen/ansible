# Example of use

gej@aten:~/ansible$ vi install_apache.yml
---
- hosts: all
  become: true
  tasks: 

  - name: install apache2 package
    ansible.builtin.apt:
      name: apache2

gej@aten:~/ansible$ ansible-playbook --ask-become-pass install_apache.yml 
BECOME password: 

PLAY [all] ******************************************************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************************************
ok: [usb.fritz.box]
ok: [usc.fritz.box]
ok: [usa.fritz.box]

TASK [install apache2 package] **********************************************************************************************************************************************
changed: [usb.fritz.box]
changed: [usa.fritz.box]
changed: [usc.fritz.box]

PLAY RECAP ******************************************************************************************************************************************************************
usa.fritz.box              : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
usb.fritz.box              : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
usc.fritz.box              : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

gej@aten:~/ansible$ 
