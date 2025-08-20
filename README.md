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
