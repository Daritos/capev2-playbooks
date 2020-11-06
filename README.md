# capev2-playbooks
This repository contains a few ansible roles and playbook to assist with deploying CAPEv2 and it's dependencies on Ubuntu 20.04 (may work on other versions as well) and is based of doomdraven's [cape2.sh](https://github.com/doomedraven/Tools/blob/master/Sandbox/cape2.sh) install script.

## Setup
Install git

```bash
sudo apt install git -y
```

and clone this repo

```bash
git clone https://github.com/Daritos/capev2-playbooks.git
cd capev2-playbooks
```

### Install ansible
Install dependencies

```bash
sudo apt -y install curl python3 python3-dev apt-transport-https ca-certificates software-properties-common
```

Add the Ansible repository NOTE! Skip if on Ubuntu 20.04

```bash
sudo apt-add-repository --yes --update ppa:ansible/ansible
```

Install Ansible

```bash
sudo apt update
sudo apt install -y ansible
```

### Run the playbook
Create your ansible inventory file

```bash
nano ./inventory
```

Insert the following and adjust according to your needs

```bash
{
    "cape_host": {
        "hosts": {
            "127.0.0.1": null
        },
        "vars" : {
            "ansible_python_interpreter": "/usr/bin/python3",
            "cape_user": "cape",
            "cape_db_user": "cape",
            "cape_db_password": "changeme",
            "cape_ip": "127.0.0.1",
            "cape_vm_subnet_ip": "192.168.100.1",
            "cape_vm_subnet_interface": "virbr1",
            "cape_nr_processor": 4,
            "cape_machinery": "kvm",
            "tor_listening_ip": "127.0.0.1",
            "cert_common_name": "localhost",
            "external_fqdn": "example.tld",
        }
    }
}
```

Launch the playbook

```bash
ansible-playbook --connection=local --inventory inventory --limit 127.0.0.1 capev2_deploy.yml
```