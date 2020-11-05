# capev2-playbooks
This repository contains ansible roles and playbook to deploy CAPEv2 on Ubuntu 20.04 (may work on other versions as well) and is based of doomdraven's [cape2.sh](https://github.com/doomedraven/Tools/blob/master/Sandbox/cape2.sh) install script.

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
Install dependencies for ansible

```bash
sudo apt -y install curl python python-dev apt-transport-https ca-certificates software-properties-common
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
            "elastic_version": "7.9.3",
            "npcap_version": "1.00",
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
            "inetsim_ip": "127.0.0.1",
            "inetsim_dns_port": 53,
            "misp_enabled": "no",
            "misp_url": "https://localhost",
            "misp_apikey": "REPLACE_ME",
            "misp_auto_publish": "no",
            "misp_min_malscore": 0,
            "misp_enrich_cape_report": "no",
            "misp_distribution": 0,
            "misp_report_threat_level_id": 2,
            "misp_report_analysis_state": 2,
            "misp_send_ioc": "no",
            "misp_include_network_ioc": "yes",
            "misp_include_ids_ioc": "yes",
            "misp_include_dropedfiles_ioc": "yes",
            "misp_include_registry_ioc": "no",
            "misp_include_mutexes_ioc": "no",
            "proxmox_ip": "REPLACE_ME",
            "proxmox_user": "REPLACE_ME",
            "proxmox_password": "REPLACE_ME",
            "proxmox_active_vms": "win7",
            "proxmox_vms": [
                {
                    "name": "win7",
                    "platform": "windows",
                    "ip": "192.168.100.10",
                    "snapshot": "clean",
                    "tags": "64x"
                }
            ]
        }
    }
}
```

Launch the playbook

```bash
ansible-playbook --connection=local --inventory inventory --limit 127.0.0.1 capev2_deploy.yml
```