# Summary
Provision disposable topology with network devices from AWS marketplace.

# Introduction
The content of this repository must be seen as an application, responsible to perform the following:

1. `build`
> - Manipulation of **`local artifacts`**, mainly files operatios.  

2. `provision`
> - Provisonning of **`resources`** in an cloud provider.

3. `deploy`
> - Setup of **`applications`** in the provisionned instances.

4. `test`
> - Validation of the **`applications`** from end-to-end.

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
> - [ansible-runner](https://pypi.org/project/ansible-runner/)
> - [ansible](https://pypi.org/project/ansible/)
> - [netaddr](https://pypi.org/project/netaddr/)
> - [boto](https://pypi.org/project/boto/)
> - [boto3](https://pypi.org/project/boto3/)

- Installation as normal user:
> `pip --user install ansible ansible-runner netaddr boto boto3`

- Installation as privileged user:
> `pip install ansible ansible-runner netaddr boto boto3`

### Applications
#### Tower
Request `Red Hat Ansible Tower` subscription [here](https://www.ansible.com/license):
  - **Save** the _`license`_ **file** in _`project/licenses/tower`_.

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
  - BigIP (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B0182WYGRA)

### PaloAlto
**Accept** the following subscriptions:
  - Firewall BYOL (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00OC1T2D4)
  - Firewall 1 (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00PJ2VDFA)
  - Firewall 2 (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00PJ2V04O)

### Splunk
**Accept** the following subscriptions:
  - Insights for Infrastructure (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B07GJ4JD93)
  - Enterprise (_optional_): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00PUXWXNE)

### Fortinet
**Accept** the following subscriptions:
  - Fortigate (**required**): Click [_Continue to Subscribe_](https://aws.amazon.com/marketplace/pp/B00PCZSWDA)

## Howto
How to customize my `environment`?
> - Edit the file [`env/extravars`](env/extravars).  
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
deploy_password: "demobox"
```
How to setup my own ssh_keys?
> - Save the **public ssh_key** in `project/keychain/<ec2_vpc_name>`.
> - Replace the **private ssh_key** in `env/ssh_key`.

How to `filter inventory` to list only my resources?
> - Edit the file [`inventory/demobox.aws_ec2.yaml`](inventory/demobox.aws_ec2.yaml):
- Uncomment the line `tag:Environment: "demobox"` and replace the value `demobox` by what is specified for variable `ec2_vpc_name`.

How to **`build`, `provision`** and **`deploy` all applications** in the topology:
> Foreground:  
> - `ansible-runner --playbook main.yaml run .`  
> Background:
> - `ansible-runner --playbook main.yaml start .`

How to choose `specific applications`?
> In order to **`build`, `provision`** and **`deploy specific applications`** in the topology:  
>  - Just use the standard `ansible-runner` command-line argument `--limit`:  
>    - `ansible-runner --playbook main.yaml --limit tower,linux start .`

_What happens when the previous command ends?_  
> It will **`build`, `provision` and `deploy`** `Red Hat Enterprise Linux` and `Red Hat Ansible Tower`.  

_What are the overall steps?_
> 1. [`Build`](##Build): Runs locally, calling the role [`build`](project/roles/build/).
> 2. [`Provision`](##Provision): Runs locally, calling the role [`provision`](project/roles/provision).
> 3. [`Deploy`](##Deploy): Runs against the provisioned device, calling the role [`deploy`](project/roles/deploy).

_What happens behind the scenes? What is the logical path of the playbook `main.yaml`?_
> - _[`main.yaml`](project/main.yaml):_
>    - _[`build.yaml`](project/build.yaml):_
>      - _roles:_
>        - _[`build`](project/roles/build):_
>          - _[`download`](https://galaxy.ansible.com/victorock/download)_
>    - _[`provision.yaml`](project/provision.yaml):_
>      - _roles:_
>        - _[`provision`](project/roles/provision):_
>          - _[`provision_ec2`](project/roles/provision_ec2):_
>              - _[`group_vars/all/ec2.yaml`](project/group_vars/all/ec2.yaml)_
>              - _[`group_vars/tower/ec2.yaml`](project/group_vars/tower/ec2.yaml)_
>              - _[`group_vars/linux/ec2.yaml`](project/group_vars/linux/ec2.yaml)_
>              - _[`host_vars/host01-tower/ec2.yaml`](project/group_vars/host01-tower/ec2.yaml)_
>              - _[`host_vars/host01-linux/ec2.yaml`](project/group_vars/host01-linux/ec2.yaml)_
>    - _[`deploy.yaml`](project/roles/deploy):_
>      - _roles:_
>        - _[`deploy_tower`](project/roles/deploy_tower):_ 
>          - _[`deploy_linux`](project/roles/deploy_linux):_
>              - _network configuration_
>              - _baseline packages_
>              - _repositories (redhat-rhui)_
>              - _subscription (optional)_
>          - _[`tower_setup`](https://galaxy.ansible.com/victorock/tower_setup):_
>            - _[`group_vars/tower/ansible_tower_setup.yaml`](project/group_vars/tower/ansible_tower_setup.yaml)_ 
>          - _[`tower_configure`](https://galaxy.ansible.com/victorock/tower_config):_
>            - _[`group_vars/tower/ansible_tower_config.yaml`](project/group_vars/tower/ansible_tower_config.yaml)_ 
>          - _[`tower_facts`](https://galaxy.ansible.com/victorock/tower_facts)_

### Build
[Build](project/roles/build) **application's** resources **locally**:
> NOTE: Only on Linux.
- `download files`
- `create files`
- `compile code`
- `etc`

### Provision 
[`Provision`](project/provision.yaml) the following [application's](####Applications) resources in:  
- [Amazon Web Services](https://aws.amazon.com):
  - [`vpc`](roles/provision_ec2/present/)
  - [`subnets`](roles/provision_ec2/present)
  - [`gateways`](roles/provision_ec2/present/)
  - [`enis`](roles/provision_ec2/present/)
  - [`eips`](roles/provision_ec2/present/)
  - [`instances`](roles/provision_ec2/present/)

- Examples:
  - `Cisco ASAv` and `Red Hat Enterprise Linux`:  
    > `ansible-runner --playbook provision.yaml --limit asa,linux start .`  
  - `F5 BigIP`, `Infoblox IPAM` and `Red Hat Enterprise Linux`:  
    > `ansible-runner --playbook provision.yaml --limit tmos,linux,nios start .`  
  - `PaloAlto NGFW` and `Red Hat Enterprise Linux`:  
    > `ansible-runner --playbook provision.yaml --limit panos,linux start .`  
  - `Red Hat Ansible Tower` and `Splunk Insights for Infrastructure`:  
    > `ansible-runner --playbook provision.yaml --limit tower,splunk start .`  
  - **All** `Routers`:
    > `ansible-runner --playbook provision.yaml --limit router start .`  
  - **All** `Firewalls`, `Load Balancers` and `Hosts`:
    > `ansible-runner --playbook provision.yaml --limit firewall,loadbalance,host start .`  

### Deploy 
[Deploy](project/deploy.yaml) the following [**applications**](####Applications).

Examples:
  - `Cisco ASAv` and `Red Hat Enterprise Linux`:  
    > `ansible-runner --playbook deploy.yaml --limit asa,linux start .`  
  - `Cisco CSR` and `Red Hat Enterprise Linux`:  
    > `ansible-runner --playbook deploy.yaml --limit ios,linux start .`  
  - `F5 BigIP`, `Infoblox IPAM` and `Red Hat Enterprise Linux`:  
    > `ansible-runner --playbook deploy.yaml --limit tmos,linux,nios start .`  
  - `Palo Alto NGFW` and `Red Hat Enterprise Linux`:  
    > `ansible-runner --playbook deploy.yaml --limit panos,linux start .`  
  - `Fortinet FortiGate` and `Red Hat Enterprise Linux`:  
    > `ansible-runner --playbook deploy.yaml --limit fortios,linux start .`  
  - `Red Hat Ansible Tower` and `Splunk Insights for Infrastructure`:  
    > `ansible-runner --playbook deploy.yaml --limit tower,splunk start .`  
  - **All** `Firewalls`:
    > `ansible-runner --playbook provision.yaml --limit firewall start .`  
  - **All** `Firewalls` and `Load Balancers`:
    > `ansible-runner --playbook provision.yaml --limit firewall,loadbalance start .`  

### Unprovision
[`Unprovision`](project/unprovision.yaml) the following [application's](####Applications) resources in:  
- [Amazon Web Services](https://aws.amazon.com):
  - [`enis`](project/roles/provision_ec2/terminated/)
  - [`eips`](project/roles/provision_ec2/terminated/)
  - [`instances`](project/roles/provision_ec2/terminated/)  

Examples:  
- `Cisco ASAv` and `Red Hat Enterprise Linux`:  
  > `ansible-runner --playbook unprovision.yaml --limit asa,linux start .`  
- `Cisco CSR` and `Red Hat Enterprise Linux`:  
  > `ansible-runner --playbook unprovision.yaml --limit ios,linux start .`  
- `F5 BigIP`, `Infoblox IPAM` and `Red Hat Enterprise Linux`:  
  > `ansible-runner --playbook unprovision.yaml --limit tmos,linux,nios start .`  
- `Palo Alto NGFW` and `Red Hat Enterprise Linux`:  
  > `ansible-runner --playbook unprovision.yaml --limit panos,linux start .`  
- `Red Hat Ansible Tower` and `Splunk Insights for Infrastructure`:  
  > `ansible-runner --playbook unprovision.yaml --limit tower,splunk start .`  

###  Teardown
[`Teardown`](project/teardown.yaml) the following [application's](####Applications) resources in:  
- [Amazon Web Services](https://aws.amazon.com):
  - [`vpc`](project/roles/provision_ec2/absent/)
  - [`subnets`](project/roles/provision_ec2/absent)
  - [`gateways`](project/roles/provision_ec2/absent/)
  - [`enis`](project/roles/provision_ec2/absent/)
  - [`eips`](project/roles/provision_ec2/absent/)
  - [`instances`](project/roles/provision_ec2/absent/)  

- Example:
  > `ansible-runner --playbook teardown.yaml run .`  

## TODO
deploy_\<application\> for the following:
- ios
- panos
- asa
- tmos
- fortios
- nios

Performing the following tasks:
- configure application administrative password according to the value of variable `deploy_password`.

## Disclaimer  
Don't use any of the content from this repository to manage real production environments.  

## Future 
Split the content of this repository in different repositories, adopting a lab/topology/scenario oriented approach.
