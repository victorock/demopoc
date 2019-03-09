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
How to customize my `environment`?
> - Edit the file [`config/environment.yaml`](config/environment.yaml).  
```YAML
---
# EC2 Region: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions
ec2_region: "eu-central-1"

# VPC Name
ec2_vpc_name: "demobox"

# Network CIDR
ec2_vpc_cidr: "10.1.0.0/16"

# Default password for deployers to configure appliances (Useful for interaction through the webinterface).
#  NOTE: The ssh-key located in project/keychain/<ec2_vpc_name> is injected by the cloud provider.
deploy_password: "demobox123!"

# Environments:
#  Leave only the environments that you want to Build, Provision and Deploy.
environments:
  - linux
  - nios
  - tower
  - splunk
  - fortios
  - asa
  - panos
  - windows
  - tmos
```
How to define my own ssh-keys?  
_NOTE: ssh-keys are generated automatically_
> - Save the **public ssh_key** in `keychain/<ec2_vpc_name>`.
> - Replace the **private ssh_key** in `env/ssh_key`.  

How to **`build`, `provision`** and **`deploy` all environments** in the topology:
> Foreground:  
> - `ansible-playbook main.yaml `  
> Background:
> - `ansible-playbook main.yaml `

How to choose `specific environments`?
> In order to **`build`, `provision`** and **`deploy specific environments`** in the topology:  
>  - Just use the standard `ansible-runner` command-line argument `--limit`:  
>    - `ansible-playbook main.yaml --limit tower,linux `  

_What happens when the previous command ends?_  
> It will **`build`, `provision` and `deploy`** `Red Hat Enterprise Linux` and `Red Hat Ansible Tower`.  

_What are the overall steps?_
> 1. [`Build`](##Build): Runs locally, calling the role [`build`](roles/build/).
> 2. [`Provision`](##Provision): Runs locally, calling the role [`provision`](roles/provision).
> 3. [`Deploy`](##Deploy): Runs against the provisioned device, calling the role [`deploy`](roles/deploy).

_What happens behind the scenes? What is the logical path of the playbook `main.yaml`?_
> - _[`main.yaml`](main.yaml):_
>    - _[`build.yaml`](build.yaml):_
>      - _roles:_
>        - _[`build`](roles/build):_
>          - _[`download`](https://galaxy.ansible.com/victorock/download)_
>    - _[`provision.yaml`](provision.yaml):_
>      - _roles:_
>        - _[`provision`](roles/provision):_
>          - _[`provision_ec2`](roles/provision_ec2):_
>              - _[`inventory/group_vars/all/ec2.yaml`](inventory/group_vars/all/ec2.yaml)_
>              - _[`inventory/group_vars/tower/ec2.yaml`](inventory/group_vars/tower/ec2.yaml)_
>              - _[`inventory/group_vars/linux/ec2.yaml`](inventory/group_vars/linux/ec2.yaml)_
>              - _[`host_vars/host01-tower/ec2.yaml`](inventory/group_vars/host01-tower/ec2.yaml)_
>              - _[`host_vars/host01-linux/ec2.yaml`](inventory/group_vars/host01-linux/ec2.yaml)_
>    - _[`deploy.yaml`](roles/deploy):_
>      - _roles:_
>        - _[`deploy_tower`](roles/deploy_tower):_ 
>          - _[`deploy_linux`](roles/deploy_linux):_
>              - _network configuration_
>              - _baseline packages_
>              - _repositories (redhat-rhui)_
>              - _subscription (optional)_
>          - _[`tower_setup`](https://galaxy.ansible.com/victorock/tower_setup):_
>            - _[`inventory/group_vars/tower/ansible_tower_setup.yaml`](inventory/group_vars/tower/ansible_tower_setup.yaml)_ 
>          - _[`tower_configure`](https://galaxy.ansible.com/victorock/tower_config):_
>            - _[`inventory/group_vars/tower/ansible_tower_config.yaml`](inventory/group_vars/tower/ansible_tower_config.yaml)_ 
>          - _[`tower_facts`](https://galaxy.ansible.com/victorock/tower_facts)_
>    - _[`test.yaml`](roles/test):_
>      - _roles:_
>        - _[`test_tower`](roles/test_tower):_  
>          - _application tests_


### Build
[Build](roles/build) **environment's** resources **locally**:
- `download files...`
- `create files...`
- `compile code...`
- `etc...`

### Provision 
[`Provision`](provision.yaml) the following [environment's](####Environments) resources in:  
- [Amazon Web Services](https://aws.amazon.com):
  - [`vpc`](roles/provision_ec2/present/)
  - [`subnets`](roles/provision_ec2/present)
  - [`gateways`](roles/provision_ec2/present/)
  - [`enis`](roles/provision_ec2/present/)
  - [`eips`](roles/provision_ec2/present/)
  - [`instances`](roles/provision_ec2/present/)

- Examples:
  - `Cisco ASAv` and `Red Hat Enterprise Linux`:  
    > `ansible-playbook provision.yaml --limit asa,linux `  
  - `F5 BigIP`, `Infoblox IPAM` and `Red Hat Enterprise Linux`:  
    > `ansible-playbook provision.yaml --limit tmos,linux,nios `  
  - `PaloAlto NGFW` and `Red Hat Enterprise Linux`:  
    > `ansible-playbook provision.yaml --limit panos,linux `  
  - `Red Hat Ansible Tower` and `Splunk Insights for Infrastructure`:  
    > `ansible-playbook provision.yaml --limit tower,splunk `  
  - **All** `Routers`:
    > `ansible-playbook provision.yaml --limit router `  
  - **All** `Firewalls`, `Load Balancers` and `Hosts`:
    > `ansible-playbook provision.yaml --limit firewall,loadbalance,host `  

### Deploy 
[Deploy](deploy.yaml) the following [**environments**](####Environments).

Examples:
  - `Cisco ASAv` and `Red Hat Enterprise Linux`:  
    > `ansible-playbook deploy.yaml --limit asa,linux `  
  - `Cisco CSR` and `Red Hat Enterprise Linux`:  
    > `ansible-playbook deploy.yaml --limit ios,linux `  
  - `F5 BigIP`, `Infoblox IPAM` and `Red Hat Enterprise Linux`:  
    > `ansible-playbook deploy.yaml --limit tmos,linux,nios `  
  - `Palo Alto NGFW` and `Red Hat Enterprise Linux`:  
    > `ansible-playbook deploy.yaml --limit panos,linux `  
  - `Fortinet FortiGate` and `Red Hat Enterprise Linux`:  
    > `ansible-playbook deploy.yaml --limit fortios,linux `  
  - `Red Hat Ansible Tower` and `Splunk Insights for Infrastructure`:  
    > `ansible-playbook deploy.yaml --limit tower,splunk `  
  - **All** `Firewalls`:
    > `ansible-playbook provision.yaml --limit firewall `  
  - **All** `Firewalls` and `Load Balancers`:
    > `ansible-playbook provision.yaml --limit firewall,loadbalance `  

### Unprovision
[`Unprovision`](unprovision.yaml) the following [environment's](####Environments) resources in:  
- [Amazon Web Services](https://aws.amazon.com):
  - [`enis`](roles/provision_ec2/terminated/)
  - [`eips`](roles/provision_ec2/terminated/)
  - [`instances`](roles/provision_ec2/terminated/)  

Examples:  
- `Cisco ASAv` and `Red Hat Enterprise Linux`:  
  > `ansible-playbook unprovision.yaml --limit asa,linux `  
- `Cisco CSR` and `Red Hat Enterprise Linux`:  
  > `ansible-playbook unprovision.yaml --limit ios,linux `  
- `F5 BigIP`, `Infoblox IPAM` and `Red Hat Enterprise Linux`:  
  > `ansible-playbook unprovision.yaml --limit tmos,linux,nios `  
- `Palo Alto NGFW` and `Red Hat Enterprise Linux`:  
  > `ansible-playbook unprovision.yaml --limit panos,linux `  
- `Red Hat Ansible Tower` and `Splunk Insights for Infrastructure`:  
  > `ansible-playbook unprovision.yaml --limit tower,splunk `  

###  Teardown
[`Teardown`](teardown.yaml) the following [environment's](####Environments) resources in:  
- [Amazon Web Services](https://aws.amazon.com):
  - [`vpc`](roles/provision_ec2/absent/)
  - [`subnets`](roles/provision_ec2/absent)
  - [`gateways`](roles/provision_ec2/absent/)
  - [`enis`](roles/provision_ec2/absent/)
  - [`eips`](roles/provision_ec2/absent/)
  - [`instances`](roles/provision_ec2/absent/)  

- Example:
  > `ansible-playbook teardown.yaml `  

### Remain
[`Remain`](remain.yaml): [`Teardown`](###Teardown) then `build`, `provision` and `deploy`.  

### Reprovision
[`Reprovision`](remain.yaml): [`Terminate`](###Terminate) then [`Provision`](###Provision).


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
