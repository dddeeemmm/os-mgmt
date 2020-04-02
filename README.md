Role Name
=========

	os_mgmt
    # Manage OpenStack Projects, Instances and many more

Requirements
------------

	python3 -m pip install openstacksdk
	# https://docs.openstack.org/openstacksdk/latest/install/index.html
	# https://docs.openstack.org/ocata/user-guide/sdk-install.html
	
    source 	openrc.sh   # or use OpenStack variables in deafaults/main.yml
	
	defaults/main.yml => os_keypair_public_key
	

Role Variables
--------------

    os_project_name:                # [required] name of you work OpenStack project (create or destroy you work OpenStack project)
    os_dns_domain:                  # [default: cloud] domain suffix you OpenStack instances: instance_name.project_name.{{ os_dns_domain }}
    os_dns_name_servers:            # [required] dns servers for subnet setup
    os_mode: instances              # [not required] instances | project | (or leave blank for full deploy)
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
		    
    ansible-playbook playbook.yml -e os_mode=project
    # present clear project
    
    ansible-playbook playbook.yml -e os_mode=instances
    # present only instances and clusters
    
    ansible-playbook playbook.yml -e os_mode=all
    # present full project, instances and clusters
    
    ansible-playbook playbook.yml -e os_mode=project -t os_user_groups
    # configure project access rights
    
    ansible-playbook playbook.yml -e os_mode=project -t os_quotas
    # configure quotas
    
    ansible-playbook playbook.yml -e os_mode=project -t os_network
    # configure router, network, subnet, dns and gateway
    
    ansible-playbook playbook.yml -e os_mode=project -t os_sg
    # present and configure only security groups
    
    ansible-playbook playbook.yml -e os_mode=project -t os_keypair
	# present project keypair
	
	ansible-playbook playbook.yml -e os_mode=project -t os_container
	# present object containers
	    

License
-------

    GLP


Author Information
------------------

	Dmitrij Petrov
