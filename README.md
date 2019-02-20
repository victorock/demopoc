Provision disposable topology with network devices from AWS marketplace.

Topology:

```
                                +--------+    +----------+   +---------+    +-----------+
                                |fw01-asa|    |fw01-panos|   |lb01-tmos|    |ipam01-nios|
                                |        |    |          |   |         |    |           |
                                | Cisco  |    | PaloAlto |   |   F5    |    | Infoblox  |
                                |        |    |          |   |         |    |           |
                                +--------+    +----------+   +---------+    +-----------+
                                |253|253|253  |254|254|254   |252|252|252   |251|251|251
                                |   |   |     |   |   |      |   |   |      |   |   |
                                |   |   |     |   |   |      |   |   |      |   |   |
                                |   |   |     |   |   |      |   |   |      |   |   |
                                |   |   |     |   |   |      |   |   |      |   |   |
+-+-------------------+---------+-------------+--------------+--------------+--------------+ec2_vpc_mgmt: 10.1.0./24
  |                   |             |   |         |   |          |   |          |   |
+------+----------------------------+-------------+--------------+--------------+----------+ec2_vpc_outside: 10.1.1.0/24
  |    |              |                 |             |              |              |
+-----------------+---------------------+-------------+--------------+--------------+------+ec2_vpc_inside: 10.1.2.0/24
  |    |          |   |
  |    |          |   |
  |    |          |   |
  |    |          |   |
  |10  |10        |11 |11
  +-----------+   +-------------+
  |lin01 rhel7|   |win01-win2016|
  |           |   |             |
  |  Red Hat  |   |  Microsoft  |
  |           |   |             |
  +-----------+   +-------------+
```

## Folder structure

```
├── inventory  
│   ├── group_vars <- Groups corresponding to the different Operating Systems.  
│   │   ├── all <- Variables that applies to all Operating Systems.  
│   │   │   └── ec2.yaml <- Global EC2 configurations.  
│   │   ├── asa <- Variables specific to this Operating System.  
│   │   │   ├── ansible.yaml <- Operating System's Ansible Configurations.  
│   │   │   └── ec2.yaml <- Operating System's EC2 Configurations.  
│   │   ├── inode <- Variables specific to this Operating System.  
│   │   │   ├── ansible.yaml <- Operating System's Ansible Configurations.  
│   │   │   └── ec2.yaml <- Operating System's EC2 Configurations.  
│   │   ├── linux <- Variables specific to this Operating System.  
│   │   │   ├── ansible.yaml <- Operating System's Ansible Configurations.  
│   │   │   └── ec2.yaml <- Operating System's EC2 Configurations.  
│   │   ├── nios <- Variables specific to this Operating System.  
│   │   │   ├── ansible.yaml <- Operating System's Ansible Configurations.  
│   │   │   └── ec2.yaml <- Operating System's EC2 Configurations.  
│   │   ├── panos <- Variables specific to this Operating System.  
│   │   │   ├── ansible.yaml <- Operating System's Ansible Configurations.  
│   │   │   └── ec2.yaml <- Operating System's EC2 Configurations.  
│   │   ├── tmos <- Variables specific to this Operating System.  
│   │   │   ├── ansible.yaml <- Operating System's Ansible Configurations.  
│   │   │   └── ec2.yaml <- Operating System's EC2 Configurations.  
│   │   └── windows <- Variables specific to this Operating System.  
│   │       ├── ansible.yaml <- Operating System's Ansible Configurations.  
│   │       └── ec2.yaml <- Vendor EC2 Configurations.  
│   ├── host_vars  
│   │   ├── fw01-asa <- Variables specific to this node.  
│   │   │   └── ec2.yaml <- Node's Specific Specs.  
│   │   ├── fw01-panos <- Variables specific to this node.  
│   │   │   └── ec2.yaml <- Node's Specific Specs.  
│   │   ├── ipam01-nios <- Variables specific to this node.  
│   │   │   └── ec2.yaml <- Node's Specific Specs.  
│   │   ├── lb01-tmos <- Variables specific to this node.  
│   │   │   └── ec2.yaml <- Node's Specific Specs.  
│   │   ├── lin01-rhel7 <- Variables specific to this node.  
│   │   │   └── ec2.yaml <- Node's Specific Specs.  
│   │   ├── tower01-inode <- Variables specific to this node.  
│   │   │   └── ec2.yaml <- Node's Specific Specs.  
│   │   └── win01-win2016 <- Variables specific to this node.  
│   │       └── ec2.yaml <- Node's Specific Specs.  
│   └── hosts <- Inventory file mapping vendor appliances and nodes.  
├── keychain <- Folder containing the ssh keys to deploy in EC2.  
│   ├── demobox <- Private key to connect to the instances.  
│   └── demobox.pub <- Public key to install in EC2.  
├── provision.yaml <- Playbook to provision the environment.  
└── teardown.yaml <- Playbook to teardown the environment  
```

## Howto

To provision the whole topology: `ansible-playbook provision.yaml`

To provision specific appliances:
- F5 TMOS: `ansible-playbook provision.yaml -e demopoc=tmos`
- Infoblox NIOS: `ansible-playbook provision.yaml -e demopoc=tmos`
- PaloAlto PANOS: `ansible-playbook provision.yaml -e demopoc=panos`

To provision specific hosts:
- Red Hat Enterprise Linux: `ansible-playbook provision.yaml -e demopoc=linux`
- Microsoft Windows: `ansible-playbook provision.yaml -e demopoc=windows`

To Destroy:
ansible-playbook teardown.yaml

**NOTE: VPCs and subnets are always destroyed**