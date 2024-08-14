amq_broker_split_brain
----------------------

You will deploy three replicated HA amq_broker instances called amq1, amq2 and amq3.

- amq1 primary and amq3 backup will live on first virtual machine
- amq2 primary and amq1 backup will live on second virtual machine
- amq3 primary and amq2 backup will live on third virtual machine

# prerequisites

## virtual machines

* prepare three rhel9 virtual machines (qemu/ec2/....) it does not matter
* setup networking between virtual machines (can be dns, route53, /etc/hosts,..)


## edit ansible.cfg

* insert the user account that ssh into target hosts
* insert the path to the private key that can ssh into target hosts
* insert the automation hub token at the bottom to download certified collections

## edit rhn_creds.yml

* get credentials at https://console.redhat.com/application-services/service-accounts
* use client_id and secret for rhn_username and rhn_password

## Execute deployment

* `ansible-galaxy collection install redhat.amq_broker`
* `ansible-playbook -i inventory/ -e @rhn_creds.yml plaubook.yml`

## Result

```
TASK [Print status] ******************************************************************************************************************************************************************************************************************************************
task path: /home/guido/Development/asciinema/amq_broker_split_brain/playbook.yml:52
ok: [amq3.internal] => {
    "ansible_facts.amq_broker.HAPolicy": "Replication Primary w/quorum voting"
}
ok: [amq1.internal] => {
    "ansible_facts.amq_broker.HAPolicy": "Replication Backup w/quorum voting"
}
```
