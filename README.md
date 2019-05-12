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
### Installation of the following packages:
- [ansible](https://pypi.org/ansible/)
- [netaddr](https://pypi.org/netaddr/)
- [boto](https://pypi.org/boto/)
- [boto3](https://pypi.org/boto3/)
- [passlib](https://pypi.org/passlib/)  

### Installation as normal user:
```
pip --user install ansible netaddr boto boto3 passlib
```

### Installation as privileged user or from inside a `virtualenv`:
```
pip install ansible netaddr boto boto3 passlib
```

### Licenses and Subscriptions
| Environment | Instructions |
| :------- | :------ |
| AWS | AWS Support ticket to increase **Elastic IPs** to **30**. <br>Default: 5 ([Reference](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html)) |
| Ansible Tower | [Red Hat Ansible Tower license](https://www.ansible.com/license) (**required**). <br> - **Save** the _`license`_ **file** in _`files/licenses/tower`_. |
| Infoblox | - [NIOS CP](https://aws.amazon.com/marketplace/pp/B0182WYGRA) (**required**). <br> - [NIOS TE](https://aws.amazon.com/marketplace/pp/B0182WYFQC) (_optional_). |
| Cisco | - [ASAv BYOL](https://aws.amazon.com/marketplace/pp/B00WRGASUC) (**required**). <br> - [ASAv](https://aws.amazon.com/marketplace/pp/B00WH2LGM0) (_optional_). |
| F5 | - [BigIP PAYG](https://aws.amazon.com/marketplace/pp/B079C44MFH) (**required**). <br> - [BigIP BYOL](https://aws.amazon.com/marketplace/pp/B00KXHN9GC) (_optional_) |
| PaloAlto |  - [Firewall BYOL](https://aws.amazon.com/marketplace/pp/B00OC1T2D4) (**required**). <br> - [Firewall 1](https://aws.amazon.com/marketplace/pp/B00PJ2VDFA) (_optional_). <br> - [Firewall 2](https://aws.amazon.com/marketplace/pp/B00PJ2V04O) (_optional_) |
| Splunk | - [Enterprise](https://aws.amazon.com/marketplace/pp/B00PUXWXNE) (**required**). <br> - [Insights for Infrastructure](https://aws.amazon.com/marketplace/pp/B07GJ4JD93) (_optional_). |
| Fortinet | - [Fortigate](https://aws.amazon.com/marketplace/pp/B00PCZSWDA) (**required**).

## Howtos
### Create my own topology
> _NOTE: In Ansible Tower do standard WEBUI manipulation of Inventories._
1. Copy the directory [inventories/full](inventories/full/) to `inventories/mytopology`.  
```
cp -ap inventories/full inventories/mytopology
```
2. Edit the file [inventories/mytopology/hosts](inventories/full/hosts) to choose the nodes in your topology.  
```
vi inventories/mytopology/hosts
```
3. Edit the file [inventories/mytopology/group_vars/site1.yaml](inventories/full/group_vars/site1.yaml) to customize subnets, vpcs, regions...    
```
vi inventories/mytopoly/group_vars/site1.yaml
```
> _NOTE: For multisite topology, consult [cisco_ios](inventories/cisco_ios/)_

### Define my own ssh-keys  
1. Save the **public ssh_key** in `files/keychain/<ec2_vpc_name>.pub`.
```
cp <my key>.pub files/keychain/site1.pub
cp <my key>.pub files/keychain/site2.pub
cp <my key>.pub files/keychain/site3.pub
```
2. Save the **private ssh_key** in `files/keychain/<ec2_vpc_name>`.  
```
cp <my key> files/keychain/site1
cp <my key> files/keychain/site2
cp <my key> files/keychain/site3
```
> _NOTE: if missing, ssh-keys are generated automatically during `build`_  

### Spawn the entire topology
```
./playbooks/main.yaml -i inventories/full
```  
> _NOTE: By default everything is provisioned in [site1](inventories/full/site1.yaml).  

### Provision all nodes from topology
```
./playbooks/provision.yaml -i inventories/full
```  
> _NOTE: ssh-key are only generated during [build](roles/build/).

### Provision specific group from topology
```
./playbooks/provision.yaml -i inventories/full --limit linux
```  
> _NOTE: ssh-key are only generated during `build`.

### Spawn different topology
```
./playbooks/main.yaml -i inventories/redhat_rhel
```  
> _HINT: Topologies are build from [inventories](inventories/) 

### Terminate specific group of nodes (`ex: site1`)  
```
./playbooks/terminate.yaml -i inventories/cisco_ios --limit site1
```  

### Provision specific group of nodes (`ex: tower`)  
```
./playbooks/provision.yaml -i inventories/cisco_ios --limit tower
```  

### Reprovision specific node (`ex: rtr01-ios`)  
```
./playbooks/reprovision.yaml -i inventories/cisco_ios --limit rtr01-ios
```  

### Reprovision specific group of nodes (`ex: ios`)   
```
./playbooks/reprovision.yaml -i inventories/cisco_ios --limit ios
```  

### Stack topologies
This is straighforward for topologies following the guideline for groups and vpc names.
1. To spawn the `cisco_ios` with 3 sites:
```
./playbooks/main.yaml -i inventories/cisco_ios --limit site1
./playbooks/main.yaml -i inventories/cisco_ios --limit site2  
./playbooks/main.yaml -i inventories/cisco_ios --limit site3
```  
> **NOTE: Multiple sites cannot be provisioned in parallel as part of the same play, because of race conditions**  
> **NOTE: Mutiple sites can be provisioned in parallel from different terminals with different --limit.**  
> **NOTE: Multiple instances are provisioned in parallel.**  

2. To provision `Infoblox` on top of the previous topology:
```
./playbooks/main.yaml -i inventories/infoblox_nios
```  

3. To provision `Splunk` on top of the previous topology:
```
./playbooks/main.yaml -i inventories/splunk_es
```  

### Integration with Ansible Tower
1. Create `project` for `https://www.github.com/victorock/demopoc`.  
2. Create `inventory` and define a `source from project` for `inventories/cisco_ios/hosts`.  
3. Create `job template` and choose a playbook from `playbooks` folder (`ex: playbooks/main.yaml`).

## FAQ 
### What topologies are available?  

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
│   │   └── site1.yaml
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

### Why the public access to Infoblox's WEBUI is not working?
Due to a limitation in the Infoblox's image, **the Infoblox AMI is only accessible through the `inside` interface (LAN1).**.  
As alternative, create a SSH tunnel through Ansible Tower to access Infoblox's WEBUI:  
1. Add ssh-key to ssh-agent:
```
ssh-add files/keychain/<ssh_private_key_file>
``` 
2. Establish SSH Tunnel (localhost:8443 -> 10.1.2.97:443):
```
ssh -l ec2-user@<tower_public_ip> -L 8443:10.1.2.97:443
```
3. Open Browser:
```
open -a "Google Chrome" https://localhost:8443/
```

### What are the overall steps happening in background?
1. **Build:** Run locally, calling the role [build](roles/build/).
2. **Provision:** Run locally, calling the role [provision](roles/provision/).
3. **Deploy:** Run against the provisioned device, calling the role [deploy](roles/deploy/).

### What happens behind the scenes when i run main.yaml?
> - _[main.yaml](playbooks/main.yaml):_
>    - _[build.yaml](playbooks/build.yaml):_
>      - _roles:_
>        - _[build](roles/build/):_
>          - _[download](https://galaxy.ansible.com/victorock/download)_
>    - _[provision.yaml](playbooks/provision.yaml):_
>      - _roles:_
>        - _[provision](roles/provision/):_
>          - _[provision_ec2](roles/provision_ec2/):_
>              - _[playbooks/group_vars/all/ec2.yaml](playbooks/group_vars/all/ec2.yaml)_
>              - _[playbooks/group_vars/tower/ec2.yaml](playbooks/group_vars/tower/ec2.yaml)_
>              - _[playbooks/group_vars/linux/ec2.yaml](playbooks/group_vars/linux/ec2.yaml)_
>              - _[playbooks/host_vars/host01-tower/ec2.yaml](playbooks/host_vars/host01-tower/ec2.yaml)_
>              - _[playbooks/host_vars/host01-linux/ec2.yaml](playbooks/host_vars/host01-linux/ec2.yaml)_
>    - _[deploy.yaml](roles/deploy):_
>      - _roles:_
>        - _[deploy_tower](roles/deploy_tower/):_ 
>          - _[deploy_linux](roles/deploy_linux/):_
>              - _network configuration_
>              - _baseline packages_
>              - _repositories (redhat-rhui)_
>              - _subscription (optional)_
>          - _[tower_setup](https://galaxy.ansible.com/victorock/tower_setup):_
>            - _[playbooks/group_vars/tower/ansible_tower_setup.yaml](playbooks/group_vars/tower/ansible_tower_setup.yaml)_ 
>          - _[tower_configure](https://galaxy.ansible.com/victorock/tower_config):_
>            - _[playbooks/group_vars/tower/ansible_tower_config.yaml](playbooks/group_vars/tower/ansible_tower_config.yaml)_ 
>          - _[tower_facts](https://galaxy.ansible.com/victorock/tower_facts)_
>    - _[test.yaml](roles/test):_
>      - _roles:_
>        - _[test_tower](roles/test_tower/):_  
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
