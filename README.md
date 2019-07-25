# ansible-django

Install the boto library.
```
$ pip --version
pip 19.0.3 from ... (python 3.7)
pip install boto
```

Fill out external variables
```
$ cp external_vars.yml{.template,}
aws_access_key=xxx
aws_secret_key=xxx
github_access_token=xxx # requires write:public_key permission
```

Execute the `site.yml` playbook to provision and configure a new EC2 instance with a django app. Ansible will create the instance, wait for connection and run a configuration play.
```
# specify the private key file when executing ansible-playbook
$ ansible-playbook --private-key ~/.ssh/id_rsa --ssh-common-args="-o StrictHostKeyChecking=no" site.yml
```
