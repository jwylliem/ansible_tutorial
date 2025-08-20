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
