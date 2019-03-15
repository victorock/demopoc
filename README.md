# Summary
Provision disposable topology with network devices from AWS marketplace.

# Introduction
The content of this repository must be seen as an environment, responsible to perform the following:

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
> - [netaddr](https://pypi.org/netaddr/)
> - [boto](https://pypi.org/boto/)
> - [boto3](https://pypi.org/boto3/)
> - [passlib](https://pypi.org/passlib/)

- Installation as normal user:
> `pip --user install ansible netaddr boto boto3 passlib`

- Installation as privileged user or from a `virtualenv`:
> `pip install ansible netaddr boto boto3 passlib`

### Environments
#### Tower
Request `Red Hat Ansible Tower` subscription [here](https://www.ansible.com/license):
  - **Save** the _`license`_ **file** in _`licenses/tower`_.

### Infoblox
**The Infoblox AMI is only accessible through the `inside` interface (LAN1).**  
  _NOTE: This is an limitation of Infoblox's image_  
**Accept** the following subscriptions:
  - NIOS CP (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B0182WYGRA)
  - NIOS TE (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B0182WYFQC)

### Cisco
**Accept** the following subscriptions:
  - ASAv BYOL (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00WRGASUC)
  - ASAv (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00WH2LGM0)

### F5
**Accept** the following subscriptions:
  - BigIP (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B0182WYGRA)

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

## Howto
How to create my own `topology`?
> - Copy the file [`topologies/default.yaml`](topologies/environment.yaml) to `topologies/mytopology.yaml`.  
> - Edit the file [`topologies/mytopology.yaml`](topologies/mytopology.yaml).  
> - Customize the subnets, vpcs, regions...  
> - Pick and choose the list of nodes, vendors, technologies and put them in `environments`.  

How to define my own ssh-keys?  
_NOTE: if missing, ssh-keys are generated automatically_
> - Save the **public ssh_key** in `keychain/<ec2_vpc_name>`.
> - Replace the **private ssh_key** in `keychain/<ec2_vpc_name>`.  

How to **`build`, `provision`** and **`deploy` all nodes**:
> - `./main.yaml`  

How to choose `specific environments`?
> In order to **`build`, `provision`** and **`deploy specific environments`** in the topology:  
> - `./main.yaml -e topology=cisco_ios_multisite_site1.yaml`  
> - `./main.yaml -e topology=cisco_ios_multisite_site2.yaml`  
> - `./main.yaml -e topology=cisco_ios_multisite_site3.yaml`  

_What are the overall steps happening in background?_
> 1. [`Build`](##Build): Runs locally, calling the role [`build`](runner/roles/build/).
> 2. [`Provision`](##Provision): Runs locally, calling the role [`provision`](runner/roles/provision).
> 3. [`Deploy`](##Deploy): Runs against the provisioned device, calling the role [`deploy`](runner/roles/deploy).

_What happens behind the scenes? What is the logical path of the topology `main.yaml`?_
> - _[`main.yaml`](runner/project/main.yaml):_
>    - _[`build.yaml`](runner/project/build.yaml):_
>      - _roles:_
>        - _[`build`](runner/roles/build):_
>          - _[`download`](https://galaxy.ansible.com/victorock/download)_
>    - _[`provision.yaml`](runner/projectprovision.yaml):_
>      - _roles:_
>        - _[`provision`](runner/roles/provision):_
>          - _[`provision_ec2`](runner/roles/provision_ec2):_
>              - _[`inventory/group_vars/all/ec2.yaml`](runner/inventory/group_vars/all/ec2.yaml)_
>              - _[`inventory/group_vars/tower/ec2.yaml`](runner/inventory/group_vars/tower/ec2.yaml)_
>              - _[`inventory/group_vars/linux/ec2.yaml`](runner/inventory/group_vars/linux/ec2.yaml)_
>              - _[`host_vars/host01-tower/ec2.yaml`](runner/inventory/group_vars/host01-tower/ec2.yaml)_
>              - _[`host_vars/host01-linux/ec2.yaml`](runner/inventory/group_vars/host01-linux/ec2.yaml)_
>    - _[`deploy.yaml`](runner/roles/deploy):_
>      - _roles:_
>        - _[`deploy_tower`](runner/roles/deploy_tower):_ 
>          - _[`deploy_linux`](runner/roles/deploy_linux):_
>              - _network configuration_
>              - _baseline packages_
>              - _repositories (redhat-rhui)_
>              - _subscription (optional)_
>          - _[`tower_setup`](https://galaxy.ansible.com/victorock/tower_setup):_
>            - _[`inventory/group_vars/tower/ansible_tower_setup.yaml`](runner/inventory/group_vars/tower/ansible_tower_setup.yaml)_ 
>          - _[`tower_configure`](https://galaxy.ansible.com/victorock/tower_config):_
>            - _[`inventory/group_vars/tower/ansible_tower_config.yaml`](runner/inventory/group_vars/tower/ansible_tower_config.yaml)_ 
>          - _[`tower_facts`](https://galaxy.ansible.com/victorock/tower_facts)_
>    - _[`test.yaml`](runner/roles/test):_
>      - _roles:_
>        - _[`test_tower`](runner/roles/test_tower):_  
>          - _application tests_


### Build
[Build](runner/project/roles/build) **environment's** resources **locally**:
- `download files...`
- `create files...`
- `compile code...`
- `etc...`

### Provision 
[`Provision`](runner/project/provision.yaml) the following [environment's](####Environments) resources in:  
- [Amazon Web Services](https://aws.amazon.com):
  - [`vpc`](runner/roles/provision_ec2/present/)
  - [`subnets`](runner/roles/provision_ec2/present)
  - [`gateways`](runner/roles/provision_ec2/present/)
  - [`enis`](runner/roles/provision_ec2/present/)
  - [`eips`](runner/roles/provision_ec2/present/)
  - [`instances`](runner/roles/provision_ec2/present/)  

### Deploy 
[Deploy](runner/project/deploy.yaml) the following [**environments**](####Environments).

### Unprovision
[`Unprovision`](runner/project/unprovision.yaml) the following [environment's](####Environments) resources in:  
- [Amazon Web Services](https://aws.amazon.com):
  - [`enis`](runner/roles/provision_ec2/terminated/)
  - [`eips`](runner/roles/provision_ec2/terminated/)
  - [`instances`](runner/roles/provision_ec2/terminated/)  

###  Teardown
[`Teardown`](runner/project/teardown.yaml) the following [environment's](####Environments) resources in:  
- [Amazon Web Services](https://aws.amazon.com):
  - [`vpc`](runner/roles/provision_ec2/absent/)
  - [`subnets`](runner/roles/provision_ec2/absent)
  - [`gateways`](runner/roles/provision_ec2/absent/)
  - [`enis`](runner/roles/provision_ec2/absent/)
  - [`eips`](runner/roles/provision_ec2/absent/)
  - [`instances`](runner/roles/provision_ec2/absent/)  

### Remain
[`Remain`](runner/project/remain.yaml): [`Teardown`](###Teardown) then `build`, `provision` and `deploy`.  

### Reprovision
[`Reprovision`](runner/project/remain.yaml): [`Terminate`](###Terminate) then [`Provision`](###Provision).


## TODO
deploy_\<environment\> for the following:
- ios
- panos
- asa
- tmos
- fortios
- nios

Performing the following tasks:
- configure environment administrative password according to the value of variable `deploy_password`.

## Disclaimer  
Don't use any of the content from this repository to manage real production environments.  

## Future 
Split the content of this repository in different repositories, adopting a lab/topology/scenario oriented approach.
