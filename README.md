# ansible-django

Install the boto library.
```
$ pip --version
pip 19.0.3 from ... (python 3.7)
pip install boto
```

Load EC2 credentials into your shell
```
$ cp .env{.template,}
export AWS_ACCESS_KEY_ID=xxx
export AWS_SECRET_ACCESS_KEY=xxx
```

Execute the `site.yml` playbook to provision and configure a new EC2 instance with a django app. Ansible will create the instance, wait for connection and run a configuration play.
```
# specify the private key file when executing ansible-playbook
$ ansible-playbook --private-key ~/.ssh/id_rsa site.yml
```
