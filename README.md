os_mgmt
=========

    Manage OpenStack Projects, Instances and many more
    https://galaxy.ansible.com/dddeeemmm/os_mgmt


Requirements
------------

    python3 -m pip install openstacksdk
    # https://docs.openstack.org/openstacksdk/latest/install/index.html
    # https://docs.openstack.org/ocata/user-guide/sdk-install.html

    source 	openrc.sh   # or use OpenStack variables in deafaults/main.yml

    set os_keypair_public_key in defaults/main.yml


Role Variables
--------------

    os_project_name:                # [required] name of you work OpenStack project (create or destroy you work OpenStack project)
    os_dns_domain:                  # [default: cloud] domain suffix you OpenStack instances: instance_name.project_name.{{ os_dns_domain }}
    os_dns_name_servers:            # [required] dns servers for subnet setup
    os_mode: instances              # [default: instances] instances | project | quota | keypair | container | network | sg | bastion | clusters | all
    os_project_state:               # [default: present] present | absent
    os_project_description:         # [not required] description of project
    os_keypair_public_key:          # [required] keypair for project
    os_containers:                  # [not required] containers in project
    os_bastion_state:               # [not required] present | absent (deploy {{ os_bastion }} instance)

    os_instances:                   # [not required] instances of project
        - { state: present,         # [default: present] present | absent
            name: instance-1,       # [default: instance] name of you instance 
            img: centos-7-1901,     # [default: {{ ubuntu }}] image 
            hdd: 15,                # [not required] main HDD in Datastore
            hdd_adv: 50,            # [not required] second HDD in Datastore
            sg: '{{ sg_web }}',     # [default: {{ sg_default }}] security groups
            fv: m1.small,           # [default: {{ fv_default }}]  flavor
            user: root              # [default: centos | ubuntu (according on image name)] system user  
            gr: 'web' }             # [not required] groups in metadata

    os_clusters:                    # [not required] dict clusters of project
      cluster:                      # [required] name of instances: cluster-[1:999]
        - { state: present,         # [default: present] present | absent
            count: 3,               # [default: 3] count VM's of cluster
            img: centos-7-1901,     # [default: {{ ubuntu }}] image 
            hdd: 15,                # [not required] main HDD in Datastore
            hdd_adv: 50,            # [not required] second HDD in Datastore
            sg: '{{ sg_web }}',     # [default: {{ sg_default }}] security groups
            fv: m1.small,           # [default: {{ fv_default }}]  flavor
            user: root              # [default: centos | ubuntu (equal image name)] system user
            gr: 'web' }             # [not required] groups in metadata


Example Playbook
----------------

    tests/test.yml


Example Run
----------------

    ansible-playbook playbook.yml -e os_mode=instances
    ansible-playbook playbook.yml -e os_mode=project
    ansible-playbook playbook.yml -e os_mode=roles
    ansible-playbook playbook.yml -e os_mode=quota
    ansible-playbook playbook.yml -e os_mode=keypair
    ansible-playbook playbook.yml -e os_mode=container
    ansible-playbook playbook.yml -e os_mode=network
    ansible-playbook playbook.yml -e os_mode=sg
    ansible-playbook playbook.yml -e os_mode=bastion
    ansible-playbook playbook.yml -e os_mode=clusters
    ansible-playbook playbook.yml -e os_mode=all


License
-------

    GLP


Author Information
------------------

    Dmitrij Petrov
