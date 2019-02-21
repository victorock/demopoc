Provision disposable topology with network devices from AWS marketplace.

## Topology

| node | ec2_vpc_mgmt | ec2_vpc_outside | ec2_vpc_inside |
| :--: | :----------: | :-------------: | :------------: |
| `fw01-panos` | `10.1.0.254` | `10.1.1.254` | `10.1.2.254` |  
| `fw01-asa` | `10.1.0.253` | `10.1.1.253` | `10.1.2.253` |  
| `lb01-tmos` | `10.1.0.252` | `10.1.1.252` | `10.1.2.252` |  
| `ipam01-nios` | `10.1.0.251` | `10.1.1.251` | `10.1.2.251` |  
| `tower01-inode` | `10.1.0.250` | `-` | `10.1.2.250` |  
| `fw01-fortios` | `10.1.0.249` | `10.1.1.249` | `10.1.2.249` |  
| `lin01-rhel7` | `10.1.0.10` | `-` | `10.1.2.10` |  
| `win01-win2016` | `10.1.0.11` | `-` | `10.1.2.11` |  

## Nodes

PaloAlto PanOS:
```
  +---------------+ ec2_vpc_mgmt: 10.1.0.0/24
  |   fw01-panos  +-------------------------------+.254
  |               | ec2_vpc_outside: 10.1.1.0/24
  |    PaloAlto   +-------------------------------+.254
  |               | ec2_vpc_inside: 10.1.2.0/24
  +---------------+-------------------------------+.254
```

Cisco ASA:
```
  +---------------+ ec2_vpc_mgmt: 10.1.0.0/24
  |   fw01-asa    +-------------------------------+.253
  |               | ec2_vpc_outside: 10.1.1.0/24
  |    Cisco      +-------------------------------+.253
  |               | ec2_vpc_inside: 10.1.2.0/24
  +---------------+-------------------------------+.253
```

F5 BigIP (tmos):
```
  +---------------+ ec2_vpc_mgmt: 10.1.0.0/24
  |   lb01-tmos   +-------------------------------+.252
  |               | ec2_vpc_outside: 10.1.1.0/24
  |      F5       +-------------------------------+.252
  |               | ec2_vpc_inside: 10.1.2.0/24
  +---------------+-------------------------------+.252
```

Infoblox NIOS:
```
  +---------------+ ec2_vpc_mgmt: 10.1.0.0/24
  |  ipam01-nios  +-------------------------------+.251
  |               | ec2_vpc_outside: 10.1.1.0/24
  |   Infoblox    +-------------------------------+.251
  |               | ec2_vpc_inside: 10.1.2.0/24
  +---------------+-------------------------------+.251
```

FortiNet FortiOS:
```
  +---------------+ ec2_vpc_mgmt: 10.1.0.0/24
  | fw01-fortios  +-------------------------------+.249
  |               | ec2_vpc_outside: 10.1.1.0/24
  |   FortiNet    +-------------------------------+.249
  |               | ec2_vpc_inside: 10.1.2.0/24
  +---------------+-------------------------------+.249
```

Red Hat Enterprise Linux:
```
  +---------------+ ec2_vpc_mgmt: 10.1.0.0/24
  |  lin01-rhel7  +-------------------------------+.10
  |               | 
  |    Red Hat    |
  |               | ec2_vpc_inside: 10.1.2.0/24
  +---------------+-------------------------------+.10
```

Microsoft Windows:
```
  +---------------+ ec2_vpc_mgmt: 10.1.0.0/24
  | win01-win2016 +-------------------------------+.11
  |               | 
  |  Microsoft    |
  |               | ec2_vpc_inside: 10.1.2.0/24
  +---------------+-------------------------------+.11
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

To provision the entire topology: `ansible-playbook provision.yaml`

To provision specific appliances:
- Cisco ASA: `ansible-playbook provision.yaml -e demopoc=asa`
- F5 TMOS: `ansible-playbook provision.yaml -e demopoc=tmos`
- Infoblox NIOS: `ansible-playbook provision.yaml -e demopoc=tmos`
- PaloAlto PANOS: `ansible-playbook provision.yaml -e demopoc=panos`

To provision specific hosts:
- Red Hat Enterprise Linux: `ansible-playbook provision.yaml -e demopoc=linux`  
- Microsoft Windows: `ansible-playbook provision.yaml -e demopoc=windows`  
- Tower Node: `ansible-playbook provision.yaml -e demopoc=tower`  

To Destroy:
ansible-playbook teardown.yaml

**NOTE: VPCs and subnets are always destroyed**