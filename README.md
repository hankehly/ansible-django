# ansible-django

Install the boto library.
```
$ pip --version
pip 19.0.3 from ... (python 3.7)
pip install boto
```

Fill out credentials
```
$ cp external_vars.yml{.template,}
aws_access_key=xxx
aws_secret_key=xxx
github_access_token=xxx # requires write:public_key permission
```

Create an EC2 keypair named `ubuntu_18.04_LTS`.
Make sure your default security group allows access to TCP 8080 port.

Execute the `site.yml` playbook to provision and configure a new EC2 instance with a django app. Ansible will create the instance, wait for connection and run a configuration play.
```
$ ansible-playbook --private-key ~/.ssh/id_rsa --ssh-common-args="-o StrictHostKeyChecking=no" --ask-vault-pass site.yml
Vault password: ansible-django
```

TODO:
- provide way to terminate instance
- setup security group with open TCP 8080 port
- create EC2 keypair
- tag app version for backward compatibility
