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
#### Tower
Request `Red Hat Ansible Tower` subscription [here](https://www.ansible.com/license):
  - **Save** the _`license`_ **file** in _`licenses/tower`_.

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

## Howto
How to create my own `topology`?
> - Create a directory for your topology: `mkdir topologies/mytopology`.  
> - Copy the file [`topologies/default.yaml`](topologies/environment.yaml) to `topologies/mytopology/scenario.yaml`.  
> - Edit the file [`topologies/mytopology.yaml`](topologies/mytopology/scenario.yaml).  
> - Customize the subnets, vpcs, regions...  
> - Pick and choose the list of nodes, vendors, technologies and put them in `environments`.  
> - Run `./main.yaml -e @topologies/mytopology/scenario.yaml`  

How to define my own ssh-keys?  
_NOTE: if missing, ssh-keys are generated automatically_
> - Save the **public ssh_key** in `keychain/<ec2_vpc_name>.pub`.
> - Replace the **private ssh_key** in `keychain/<ec2_vpc_name>`.  

How to **`build`, `provision`** and **`deploy` all nodes**:
> - `./main.yaml -e @topologies/default.yaml`  

How to choose `specific topologies`?
> In order to **`build`, `provision`** and **`deploy specific environments`** in the topology:  
> - `./main.yaml -e @topologies/cisco_ios/multisite_site1.yaml`  
> - `./main.yaml -e @topologies/cisco_ios/multisite_site2.yaml`  
> - `./main.yaml -e @topologies/cisco_ios/multisite_site3.yaml`  

How to **`teardown`**?
> - `./teardown.yaml -e @topologies/cisco_ios/multisite_site1.yaml`  
> - `./teardown.yaml -e @topologies/cisco_ios/multisite_site2.yaml`  
> - `./teardown.yaml -e @topologies/cisco_ios/multisite_site3.yaml`  

### FAQ 
What topologies are available?  
```
topologies/
├── cisco_ios
│   ├── multisite_site1.yaml
│   ├── multisite_site2.yaml
│   └── multisite_site3.yaml
├── default.yaml
├── f5_tmos
│   └── loadbalance.yaml
├── infoblox_nios
│   └── ipam_ios.yaml
├── microsoft_windows
│   └── server.yaml
├── paloalto_panos
│   └── firewall.yaml
├── redhat_rhel
│   └── server.yaml
└── splunk_es
    └── logging.yaml
```

Why the public access to Infoblox's WEBUI is not working?
> Due to a limitation in the Infoblox's image, **the Infoblox AMI is only accessible through the `inside` interface (LAN1).**.  
> As workaround, you can create a SSH tunnel from your machine, in order to be able to access Infoblox's WEBUI:  
> - Add ssh-key to ssh-agent:
>   - `ssh-add keychain/<ssh_private_key_file>` 
> - Establish SSH Tunnel (localhost:8443 -> 10.1.2.97:443):
>   - `ssh -l ec2-user@<tower_public_ip> -L 8443:10.1.2.97:443`
> - Open Browser:
>   - `open -a "Google Chrome" https://localhost:8443/`

_What are the overall steps happening in background?_
> 1. [`Build`](##Build): Runs locally, calling the role [`build`](runner/roles/build/).
> 2. [`Provision`](##Provision): Runs locally, calling the role [`provision`](runner/roles/provision).
> 3. [`Deploy`](##Deploy): Runs against the provisioned device, calling the role [`deploy`](runner/roles/deploy).

_What happens behind the scenes? What is the logical path of the topology when i run `main.yaml`?_
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

#### Build
What the [`Build`](runner/project/roles/build) playbook does ?  
[`Build`](runner/project/roles/build) **environment's** resources **locally**:
- `download files...`
- `create files...`
- `compile code...`
- `etc...`

#### Provision
What the [`Provision`](runner/project/provision.yaml) playbook does ?  
> [`Provision`](runner/project/provision.yaml) the following [environment's](####Environments) resources:  
> - [Amazon Web Services](https://aws.amazon.com):
>   - [`vpc`](runner/roles/provision_ec2/present/)
>   - [`subnets`](runner/roles/provision_ec2/present)
>   - [`gateways`](runner/roles/provision_ec2/present/)
>   - [`enis`](runner/roles/provision_ec2/present/)
>   - [`eips`](runner/roles/provision_ec2/present/)
>   - [`instances`](runner/roles/provision_ec2/present/)  

> There are two types of `nodes` to provision in [`provision_ec2`](runner/project/roles/provision_ec2):
> - [`network`](runner/project/roles/provision_ec2/present/network.yaml) with **3 Network interfaces**.
> - [`host`](runner/project/roles/provision_ec2/present/host.yaml) with **2 Network interfaces**.

#### Deploy
What the [`Deploy`](runner/project/deploy.yaml) playbook does ?  
> [`Deploy`](runner/project/deploy.yaml) the [**environments**](####Environments).  
> For each environment the role `deploy_<environment>` shall exist to perform environment specific tasks.
> Follow below roles in charge of environment specific deployments:  
>   - [`deploy_linux`](runner/projet/roles/deploy_linux)
>   - [`deploy_tower`](runner/projet/roles/deploy_tower)
>   - [`deploy_splunk`](runner/projet/roles/deploy_splunk)
>   - [`deploy_nios`](runner/projet/roles/deploy_nios)

#### Unprovision
What the [`Unprovision`](runner/project/unprovision.yaml) playbook does ?  
> [`Unprovision`](runner/project/unprovision.yaml) the following [environment's](####Environments) resources:  
> - [Amazon Web Services](https://aws.amazon.com):
>   - [`enis`](runner/roles/provision_ec2/terminated/)
>   - [`eips`](runner/roles/provision_ec2/terminated/)
>   - [`instances`](runner/roles/provision_ec2/terminated/)  

#### Teardown
What the [`Teardown`](runner/project/teardown.yaml) playbook does ?  
> [`Teardown`](runner/project/teardown.yaml) the following [environment's](####Environments) resources:  
> - [Amazon Web Services](https://aws.amazon.com):
>   - [`vpc`](runner/roles/provision_ec2/absent/)
>   - [`subnets`](runner/roles/provision_ec2/absent)
>   - [`gateways`](runner/roles/provision_ec2/absent/)
>   - [`enis`](runner/roles/provision_ec2/absent/)
>   - [`eips`](runner/roles/provision_ec2/absent/)
>   - [`instances`](runner/roles/provision_ec2/absent/)  

#### Reprovision
What the [`Reprovision`](runner/project/remain.yaml) playbook does ?  
> [`Terminate`](###Terminate) then [`Provision`](###Provision).  

## TODO
deploy_\<environment\> for the following:
- asa
- fortios

Performing the following tasks:
- configure environment administrative password according to the value of variable `deploy_password`.

## Disclaimer  
Don't use any of the content from this repository to manage real production environments.  

## Future 
Split the content of this repository in different repositories, adopting a lab/topology/scenario oriented approach.
