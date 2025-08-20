# Ansible tutorial - Learn Linux TV
https://www.youtube.com/watch?v=4REljLsOnXk&list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70&index=4

## STEPS
1. Install ansible
```
sudo apt-get update
sudo apt-get install ansible
```

2. Create inventory, put all the list of IP to manage inside the file 
```
touch inventory
```

3. Ping all server in directory
```
ansible all --key-file /home/ubuntu/.ssh/id_ed25519 -i inventory -m ping
```

If successful will show like this:
```
ubuntu@ip-172-26-8-114:~/ansible_tutorial$ ansible all --key-file /home/ubuntu/.ssh/id_ed25519 -i inventory -m ping
54.255.181.204 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
13.229.121.144 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
54.169.218.213 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

4. Create ansible.cfg file, put in inventory file and ssh key

5. Rerun ping with default cfg file
```
ansible all -m ping
```

6. List all host within inventory
```
ubuntu@ip-172-26-8-114:~/ansible_tutorial$ ansible all --list-hosts
  hosts (3):
    13.229.121.144
    54.255.181.204
    54.169.218.213
```


8. Run gather facts for all inventory
```
ansible all -m gather_facts
```
This will print all server info, softwares, env vars, etc

9. Run sudo apt update using ansible

```
ansible all -m apt -a update_cache=true --become --ask-become-pass 
```

10. Install vim-nox package using sudo
```
ansible all -m apt -a name=vim-nox --become --ask-become-pass
```

11. Installing something that is already installed will return change = false, for example tmux package
```
ubuntu@ip-172-26-8-114:~/ansible_tutorial$ ansible all -m apt -a name=tmux --become --ask-become-pass
BECOME password:
54.169.218.213 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "cache_update_time": 1755702623,
    "cache_updated": false,
    "changed": false
}
13.229.121.144 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "cache_update_time": 1755702570,
    "cache_updated": false,
    "changed": false
}
54.255.181.204 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "cache_update_time": 1755702571,
    "cache_updated": false,
    "changed": false
}
```

12. You can see the logs of apt in /var/log/apt/history 
```
Start-Date: 2025-08-20  15:34:25
Commandline: /usr/bin/apt-get -y -o Dpkg::Options::=--force-confdef -o Dpkg::Options::=--force-confold install vim-nox=2:9.1.0016-1ubuntu7.8
Requested-By: ubuntu (1000)
Install: ruby-sdbm:amd64 (1.0.0-5build4, automatic), zip:amd64 (3.0-13ubuntu0.2, automatic), liblua5.1-0:amd64 (5.1.5-9build2, automatic), fonts-lato:amd64 (2.015-1, automatic), libruby:amd64 (1:3.2~ubuntu1, automatic), ruby-net-telnet:amd64 (0.2.0-1, automatic), rubygems-integration:amd64 (1.18, automatic), libruby3.2:amd64 (3.2.3-1ubuntu0.24.04.5, automatic), rake:amd64 (13.0.6-3, automatic), vim-nox:amd64 (2:9.1.0016-1ubuntu7.8), ruby:amd64 (1:3.2~ubuntu1, auto
matic), ruby3.2:amd64 (3.2.3-1ubuntu0.24.04.5, automatic), libjs-jquery:amd64 (3.6.1+dfsg+~3.5.14-1, automatic), unzip:amd64 (6.0-28ubuntu4.1, automatic),
``` 

13. To Run update with multiple arguments, eg upgrade snapd to latest version, must use quote:
``` 
ansible all -m apt -a "name=snapd state=latest" --become --ask-become-pass
```

14. Create install_apache.yml playbook, and run the playbook:
```
ansible-playbook --ask-become-pass install_apache.yml 
```
Notice the difference in changed=1 and changed=0, when you run it for the first and second time.
ansible will only install if the package has not been installed yet.

```
TASK [install apache2 package] *****************************************************************************************************************************
changed: [54.255.181.204]
changed: [13.229.121.144]
changed: [54.169.218.213]

PLAY RECAP *************************************************************************************************************************************************
13.229.121.144             : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
54.169.218.213             : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
54.255.181.204             : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

ubuntu@ip-172-26-8-114:~/ansible_tutorial$ ls
README.md  ansible.cfg  install_apache.yml  inventory
ubuntu@ip-172-26-8-114:~/ansible_tutorial$ vi README.md
ubuntu@ip-172-26-8-114:~/ansible_tutorial$
ubuntu@ip-172-26-8-114:~/ansible_tutorial$ ansible-playbook --ask-become-pass install_apache.yml
BECOME password:

PLAY [all] *************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [54.169.218.213]
ok: [54.255.181.204]
ok: [13.229.121.144]

TASK [install apache2 package] *****************************************************************************************************************************
ok: [54.169.218.213]
ok: [54.255.181.204]
ok: [13.229.121.144]

PLAY RECAP *************************************************************************************************************************************************
13.229.121.144             : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
54.169.218.213             : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
54.255.181.204             : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

And notice the output when we try to find package that does not exist:
```
BECOME password:


PLAY [all] *************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [54.169.218.213]
ok: [13.229.121.144]
ok: [54.255.181.204]

TASK [install apache2 package] *****************************************************************************************************************************
fatal: [54.169.218.213]: FAILED! => {"changed": false, "msg": "No package matching 'chocobo' is available"}
fatal: [54.255.181.204]: FAILED! => {"changed": false, "msg": "No package matching 'chocobo' is available"}
fatal: [13.229.121.144]: FAILED! => {"changed": false, "msg": "No package matching 'chocobo' is available"}

PLAY RECAP *************************************************************************************************************************************************
13.229.121.144             : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
54.169.218.213             : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
54.255.181.204             : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

15. You can specify the state(version of the package), for example latest will always pull the latest version for you
You can also specify state to absent, in this case ansible will remove the package for you.


16. You can state the user of which ansible should use to ssh in the inventory by adding the username


17. If you want to check info for specific host you can do:  

```
ansible all -m gather_facts --limit 54.254.145.34 | grep ansible_distribution


ubuntu@ip-172-26-8-114:~/ansible_tutorial$ ansible all -m gather_facts --limit 54.254.145.34 | grep ansible_distribution
        "ansible_distribution": "CentOS",
```

18. when condition can be used to target specific server, e.g by `ansible_distribution` or by `ansible_distributiont_version` or both
```
when: ansible_distribution == "CentOS" and ansible_distribution_version == "8.2"
```

