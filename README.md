# ansible-django

Make sure you have EC2 credentials loaded in your shell.
```
export AWS_ACCESS_KEY_ID=xxx
export AWS_SECRET_ACCESS_KEY=xxx
```

Install the boto library.
```
$ pip --version
pip 19.0.3 from ... (python 3.7)
pip install boto
```

Execute the `ec2.yml` playbook to create an EC2 instance named `test_instance`.
You can now execute other playbooks without specifying the IP address or private key, because this information is obtained dynamically through the `ec2.y` script.
```
$ cat connection-test.yml
- hosts: tag_Name_test_instance
  remote_user: ubuntu
  gather_facts: false
  tasks:
    - name: check connection
      ping:

$ ansible-playbook -i ec2.py site.yml
PLAY [tag_Name_test_instance] ************************************************************************************

TASK [check connection] ******************************************************************************************
ok: [34.xxx.xxx.115]

PLAY RECAP *******************************************************************************************************
34.xxx.xxx.115             : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

You can target your EC2 instances via instance ID, region and [other mappings](https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html#inventory-script-example-aws-ec2) too. No need to specify any kwargs.
```
# ping all EC2 instances in us-east-1 region
# specify login user here because we are not executing a playbook with a remote_user setting
$ ansible -i ec2.py -u ubuntu us-east-1 -m ping
34.xxx.xxx.182 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
34.xxx.xxx.115 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}

# target a single instance with the instance ID
$ ansible -i ec2.py -u ubuntu i-abcdefghxxxxxxxx -m ping
34.xxx.xxx.115 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Ansible doesn't specifically provide a way to specify hosts at runtime; but a workaround is to use the `--extra-vars` option.
```
$ cat dynamic-host-playbook.yml
- hosts: '{{ host }}'
  tasks:
    - name: check connection
      ping:
  	
$ ansible-playbook -i ec2.py dynamic-host-playbook.yml --extra-vars "host=us-east-1"
```
