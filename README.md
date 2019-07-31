# ansible-django

Install `boto`, `boto3` and `botocore` ansible module dependencies.
```
$ pip --version
pip 19.0.3 from ... (python 3.7)
pip install boto boto3 botocore
```

Fill out AWS and GitHub credentials in the `vars` folder.
```
$ cp aws.yml{.template,}
aws_access_key=xxx
aws_secret_key=xxx

$ cp github.yml{.template,}
github_access_token=xxx # requires write:public_key permission
```

Execute the `site.yml` playbook to provision and configure a new EC2 instance with a demo [django app](https://github.com/hankehly/uwsgi-quickstart). Ansible will create an EC2 instance, wait for it to become available, run a configuration play and output a message containing a link to the public app when configuration is complete.
```
$ ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook --ask-vault-pass site.yml
Vault password: ansible-django
```

TODO:
- provide way to terminate instance
