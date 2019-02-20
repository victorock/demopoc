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


├── inventory

│   ├── group_vars <- Groups corresponding to the different appliances.

│   │   ├── all <- Variables that applies to all appliances.

│   │   │   └── ec2.yaml <- Global EC2 configurations.

│   │   ├── asa <- Variables that applies to asa appliances.

│   │   │   ├── ansible.yaml 

│   │   │   └── ec2.yaml <- Appliance's EC2 Configurations: `ec2_image`, 
`ec2_instance_type`, `ec2_instance_role`.

│   │   ├── inode <- Variables that applies to Isolated NODE appliances

│   │   │   ├── ansible.yaml

│   │   │   └── ec2.yaml <- Appliance's EC2 Configurations: `ec2_image`, `ec2_instance_type`, `ec2_instance_role`.

│   │   ├── linux <- Variables that applies to linux hosts

│   │   │   ├── ansible.yaml

│   │   │   └── ec2.yaml <- Appliance's EC2 Configurations: `ec2_image`, `ec2_instance_type`, `ec2_instance_role`.

│   │   ├── nios <- Variables that applies to nios appliances

│   │   │   ├── ansible.yaml

│   │   │   └── ec2.yaml <- Appliance's EC2 Configurations: `ec2_image`, `ec2_instance_type`, `ec2_instance_role`.

│   │   ├── panos <- Variables that applies to panos appliances

│   │   │   ├── ansible.yaml

│   │   │   └── ec2.yaml <- Appliance's EC2 Configurations: `ec2_image`, `ec2_instance_type`, `ec2_instance_role`.

│   │   ├── tmos <- Variables that applies to tmos appliances

│   │   │   ├── ansible.yaml

│   │   │   └── ec2.yaml <- Appliance's EC2 Configurations: `ec2_image`, `ec2_instance_type`, `ec2_instance_role`.

│   │   └── windows <- Variables that applies to windows hosts

│   │       ├── ansible.yaml

│   │       └── ec2.yaml <- Appliance's EC2 Configurations: `ec2_image`, `ec2_instance_type`, `ec2_instance_role`.

│   ├── host_vars

│   │   ├── fw01-asa <- Variables that applies to this specific host.

│   │   │   └── ec2.yaml <- Node's Specific Specs: `ec2_instance_interface`

│   │   ├── fw01-panos <- Variables that applies to this specific host.

│   │   │   └── ec2.yaml <- Node's Specific Specs: `ec2_instance_interface`

│   │   ├── ipam01-nios <- Variables that applies to this specific host.

│   │   │   └── ec2.yaml <- Node's Specific Specs: `ec2_instance_interface`

│   │   ├── lb01-tmos <- Variables that applies to this specific host.

│   │   │   └── ec2.yaml <- Node's Specific Specs: `ec2_instance_interface`

│   │   ├── lin01-rhel7 <- Variables that applies to this specific host.

│   │   │   └── ec2.yaml <- Node's Specific Specs: `ec2_instance_interface`

│   │   ├── tower01-inode <- Variables that applies to this specific host.

│   │   │   └── ec2.yaml <- Node's Specific Specs: `ec2_instance_interface`

│   │   └── win01-win2016 <- Variables that applies to this specific host.

│   │       └── ec2.yaml <- Node's Specific Specs: `ec2_instance_interface`

│   └── hosts <- Inventory file specifying appliances and nodes member of each appliance group.

├── keychain <- Folder containing the ssh keys to deploy in EC2.

│   ├── demobox <- Private key to connect to the instances: `ssh-add demobox` to have it in ssh-agent.

│   └── demobox.pub <- Public key to install in EC2. The filename must be the same as the value of variable `{{ ec2_vpc_name }}.pub`.

├── provision.yaml <- Provision the environment. Provide the variable `demopoc` with the proper appliance or host (Default: all).

└── teardown.yaml <- Teardown the environment. Provide the variable `demopoc` with the proper appliance or host (Default: all).


## Howto Use

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