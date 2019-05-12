# Summary
Collection to provision disposable topologies.

# Introduction
## Roles
The content of this repository is subdivided in the following categories:

1. `build`
> - Manipulation of **`local artifacts`**, mainly files operatios.  

2. `provision`
> - Provisonning of **`resources`** in an cloud provider.

3. `deploy`
> - Setup of **`environments`** in the provisionned instances.

4. `test`
> - Validation of the **`environments`** from end-to-end.

## Topology
### Subnets
| name        | cidr          |
| :---------- | :----------: |
| **mgmt**    | 10.1.**0**.0 |
| **outside** | 10.1.**1**.0 |
| **inside**  | 10.1.**2**.0 |

### IP Allocation Ranges
| Range            | Start          | End            |
| :--------------- | :------------: | :------------: |
| **Network**      | 10.1.X.**254** | 10.1.X.**124** |  
| **Host**         | 10.1.X.**123** | 10.1.X.**1**   |

### Network Devices
#### Firewalls
| mgmt         | outside         | inside         | groups     | inventory_hostname |  
| :----------: | :-------------: | :------------: | :--------: | :----------------: |
| `10.1.0.254` | `10.1.1.254`    | `10.1.2.254`   | `panos`    | `fw01-panos`       |  
| `10.1.0.253` | `10.1.1.253`    | `10.1.2.253`   | `asa`      | `fw01-asa`         |  
| `10.1.0.252` | `10.1.1.252`    | `10.1.2.252`   | `fortios`  | `fw01-fortios`     |  

#### Load Balancers
| mgmt         | outside         | inside         | groups     | inventory_hostname |  
| :----------: | :-------------: | :------------: | :--------: | :----------------: |
| `10.1.0.200` | `10.1.1.200`    | `10.1.2.200`   | `tmos`     | `lb01-tmos`        | 

#### Routers
| mgmt         | outside         | inside         | groups     | inventory_hostname |  
| :----------: | :-------------: | :------------: | :--------: | :----------------: |
| `10.1.0.150` | `10.1.1.150`    | `10.1.2.150`   | `ios`      | `rtr01-ios`        | 
| `10.1.0.149` | `10.1.1.149`    | `10.1.2.149`   | `ios`      | `rtr02-ios`        | 
| `10.1.0.148` | `10.1.1.148`    | `10.1.2.148`   | `ios`      | `rtr03-ios`        | 
| `10.1.0.147` | `10.1.1.147`    | `10.1.2.147`   | `ios`      | `rtr04-ios`        | 
| `10.1.0.146` | `10.1.1.146`    | `10.1.2.146`   | `ios`      | `rtr05-ios`        | 
| `10.1.0.145` | `10.1.1.145`    | `10.1.2.145`   | `ios`      | `rtr06-ios`        | 

### Host Devices
| mgmt         | inside         | groups     | inventory_hostname |  
| :----------: | :------------: | :--------: | :----------------: |
| `10.1.0.100` | `10.1.2.100`   | `tower`    | `host01-tower`     |  
| `10.1.0.99`  | `10.1.2.99`    | `linux`    | `host01-linux`     |  
| `10.1.0.98`  | `10.1.2.98`    | `windows`  | `host01-windows`   |   
| `10.1.0.97`  | `10.1.2.97`    | `nios`     | `host01-nios`      |  
| `10.1.0.96`  | `10.1.2.96`    | `splunk`   | `host01-splunk`    |  

## Preparation
### General

Installation of the following packages:
> - [ansible](https://pypi.org/ansible/)
> - [ansible-runner](https://pypi.org/ansible-runner/)
> - [netaddr](https://pypi.org/netaddr/)
> - [boto](https://pypi.org/boto/)
> - [boto3](https://pypi.org/boto3/)
> - [passlib](https://pypi.org/passlib/)

- Installation as normal user:
> `pip --user install ansible ansible-runner netaddr boto boto3 passlib`

- Installation as privileged user or from inside a `virtualenv`:
> `pip install ansible ansible-runner netaddr boto boto3 passlib`

### Environments
### AWS
Open a ticket and request to increase the number of `Elastic IPs` for your account.  
By default AWS only allows 5 `Elastic IPs` per VPC, meanwhile some labs may required more.  
Reference: [Elastic IP Addresses (IPv4)](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html)  

#### Tower
Request `Red Hat Ansible Tower` subscription [here](https://www.ansible.com/license):
  - **Save** the _`license`_ **file** in _`files/licenses/tower`_.

### Infoblox
**Accept** the following subscriptions:
  - NIOS CP (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B0182WYGRA)
  - NIOS TE (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B0182WYFQC)

### Cisco
**Accept** the following subscriptions:
  - ASAv BYOL (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00WRGASUC)
  - ASAv (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00WH2LGM0)

### F5
**Accept** the following subscriptions:
  - BigIP PAYG (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B079C44MFH)
  - BigIP BYOL (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00KXHN9GC)

### PaloAlto
**Accept** the following subscriptions:
  - Firewall BYOL (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00OC1T2D4)
  - Firewall 1 (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00PJ2VDFA)
  - Firewall 2 (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00PJ2V04O)

### Splunk
**Accept** the following subscriptions:
  - Insights for Infrastructure (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B07GJ4JD93)
  - Enterprise (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00PUXWXNE)

### Fortinet
**Accept** the following subscriptions:
  - Fortigate (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00PCZSWDA)

## Howtos

How to create my own `topology`?
> - Copy the directory [`inventories/full`](inventories/full) to `inventories/mytopology`.  
> - Edit the file [`inventories/mytopology/hosts`](inventories/full/hosts) to choose the nodes in your topology.  
> - Edit the file [`inventories/mytopology/group_vars/all.yaml`](inventories/full/group_vars/all.yaml) to customize subnets, vpcs, regions...    
> - For multisite topology, consult [`cisco_ios`](inventories/cisco_ios/)

How to define my own ssh-keys?  
_NOTE: if missing, ssh-keys are generated automatically_
> - Copy the **public ssh_key** in `files/keychain/<ec2_vpc_name>.pub`.
> - Copy the **private ssh_key** in `files/keychain/<ec2_vpc_name>`.  

How to **`build`, `provision`** and **`deploy` all nodes**:
> - `./playbooks/main.yaml -i inventories/full`  

How to spawn `specific topologies`?
> In order to **`build`, `provision`** and **`deploy specific environment`** in the topology:  
> - `./playbooks/main.yaml -i inventories/cisco_ios`  

How to manipulate `specific nodes`?
> Hence topology is build on top of inventory, there is no magic:
> - In order to **`terminate`** a group (`ex: site1`) in the topology:  
>   - `s`  
> - In order to **`provision`** a group (`ex: tower`) in the topology:  
>   - `./playbooks/terminate.yaml -i inventories/cisco_ios --limit tower`  
> - In order to **`reprovision`** a `node` in the topology:  
>   - `./playbooks/reprovision.yaml -i inventories/cisco_ios --limit rtr01-ios`  
> - In order to **`reprovision`** a group (`ex: ios`) in the topology:  
>   - `./playbooks/reprovision.yaml -i inventories/cisco_ios --limit ios`  

How to stack topologies together?
> **NOTES:**
>   - **Multiple sites cannot be provisioned in parallel in the same play because of race conditions**  
>   - **Multiple instances are provisioned in parallel.**  
>   - **Mutiple sites can be provisioned in parallel from different terminals with different --limit.**  

> If the topology follows the guideline for groups and vpc names, then the process is straighforward:
> - To provision the `cisco_ios`, which includes 3 sites, where site1 is the main site:
>   - `./playbooks/provision.yaml -i inventories/cisco_ios --limit site1`  
>   - `./playbooks/provision.yaml -i inventories/cisco_ios --limit site2`  
>   - `./playbooks/provision.yaml -i inventories/cisco_ios --limit site3`  
> - To provision Infoblox on top of the previous topology:
>   - `./playbooks/provision.yaml -i inventories/infoblox_nios`  
> - To provision Splunk on top of the previous topology:
>   - `./playbooks/provision.yaml -i inventories/splunk_es`  

How to integrate with Ansible Tower?
> Create a project for `https://www.github.com/victorock/demopoc`.  
> Create an inventory and define a source from project (`ex: inventories/cisco_ios/hosts`).  
> Create a `job template` and choose a playbook from `playbooks` (`ex: playbooks/main.yaml`).

### FAQ 
What topologies are available?  

```
inventories/
.
├── cisco_ios
│   ├── group_vars
│   │   ├── site1.yaml
│   │   ├── site2.yaml
│   │   └── site3.yaml
│   └── hosts
├── f5_tmos
│   ├── group_vars
│   │   └── site1.yaml
│   └── hosts
├── full
│   ├── group_vars
│   │   └── all.yaml
│   └── hosts
├── infoblox_nios
│   ├── group_vars
│   │   └── site1.yaml
│   └── hosts
├── microsoft_windows
│   ├── group_vars
│   │   └── site1.yaml
│   └── hosts
├── paloalto_panos
│   ├── group_vars
│   │   └── site1.yaml
│   └── hosts
├── redhat_rhel
│   ├── group_vars
│   │   └── site1.yaml
│   └── hosts
└── splunk_es
    ├── group_vars
    │   └── site1.yaml
    └── hosts
```

Why the public access to Infoblox's WEBUI is not working?
> Due to a limitation in the Infoblox's image, **the Infoblox AMI is only accessible through the `inside` interface (LAN1).**.  
> As alternative, create a SSH tunnel to access Infoblox's WEBUI:  
> - Add ssh-key to ssh-agent:
>   - `ssh-add files/keychain/<ssh_private_key_file>` 
> - Establish SSH Tunnel (localhost:8443 -> 10.1.2.97:443):
>   - `ssh -l ec2-user@<tower_public_ip> -L 8443:10.1.2.97:443`
> - Open Browser:
>   - `open -a "Google Chrome" https://localhost:8443/`

_What are the overall steps happening in background?_
> 1. [`Build`](##Build): Runs locally, calling the role [`build`](roles/build/).
> 2. [`Provision`](##Provision): Runs locally, calling the role [`provision`](roles/provision/).
> 3. [`Deploy`](##Deploy): Runs against the provisioned device, calling the role [`deploy`](roles/deploy/).

_What happens behind the scenes? What is the logical path of the topology when i run `main.yaml`?_
> - _[`main.yaml`](main.yaml):_
>    - _[`build.yaml`](build.yaml):_
>      - _roles:_
>        - _[`build`](roles/build/):_
>          - _[`download`](https://galaxy.ansible.com/victorock/download)_
>    - _[`provision.yaml`](projectprovision.yaml):_
>      - _roles:_
>        - _[`provision`](roles/provision/):_
>          - _[`provision_ec2`](roles/provision_ec2/):_
>              - _[`inventories/group_vars/all/ec2.yaml`](inventories/group_vars/all/ec2.yaml)_
>              - _[`inventories/group_vars/tower/ec2.yaml`](inventories/group_vars/tower/ec2.yaml)_
>              - _[`inventories/group_vars/linux/ec2.yaml`](inventories/group_vars/linux/ec2.yaml)_
>              - _[`host_vars/host01-tower/ec2.yaml`](inventories/host_vars/host01-tower/ec2.yaml)_
>              - _[`host_vars/host01-linux/ec2.yaml`](inventories/host_vars/host01-linux/ec2.yaml)_
>    - _[`deploy.yaml`](roles/deploy):_
>      - _roles:_
>        - _[`deploy_tower`](roles/deploy_tower/):_ 
>          - _[`deploy_linux`](roles/deploy_linux/):_
>              - _network configuration_
>              - _baseline packages_
>              - _repositories (redhat-rhui)_
>              - _subscription (optional)_
>          - _[`tower_setup`](https://galaxy.ansible.com/victorock/tower_setup):_
>            - _[`inventories/group_vars/tower/ansible_tower_setup.yaml`](inventories/group_vars/tower/ansible_tower_setup.yaml)_ 
>          - _[`tower_configure`](https://galaxy.ansible.com/victorock/tower_config):_
>            - _[`inventories/group_vars/tower/ansible_tower_config.yaml`](inventories/group_vars/tower/ansible_tower_config.yaml)_ 
>          - _[`tower_facts`](https://galaxy.ansible.com/victorock/tower_facts)_
>    - _[`test.yaml`](roles/test):_
>      - _roles:_
>        - _[`test_tower`](roles/test_tower):_  
>          - _application tests_

## Additional Details
- [Playbooks](playbooks/README.md)
- [Roles](roles/README.md)

## TODO
deploy_\<environment\> for the following:
- asa
- fortios

Performing the following tasks:
- configure environment administrative password according to the value of variable `deploy_password`.

## Disclaimer  
Don't use any of the content from this repository to manage real production environments.  
