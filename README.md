Provision disposable topology with network devices from AWS marketplace.

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
| **Firewall**     | 10.1.X.**254** | 10.1.X.**201** |  
| **Load Balance** | 10.1.X.**200** | 10.1.X.**101** |
| **Host**         | 10.1.X.**100** | 10.1.X.**1**   |

### Firewalls
| mgmt         | outside         | inside           | groups     | inventory_hostname |  
| :----------: | :-------------: | :------------: | :--------: | :----------------: |
| `10.1.0.254` | `10.1.1.254`    | `10.1.2.254`   | `panos`    | `fw01-panos`       |  
| `10.1.0.253` | `10.1.1.253`    | `10.1.2.253`   | `asa`      | `fw01-asa`         |  
| `10.1.0.252` | `10.1.1.252`    | `10.1.2.252`   | `fortios`  | `fw01-fortios`     |  

### Load Balancers
| mgmt         | outside         | inside         | groups     | inventory_hostname |  
| :----------: | :-------------: | :------------: | :--------: | :----------------: |
| `10.1.0.200` | `10.1.1.200`    | `10.1.2.200`   | `tmos`     | `lb01-tmos`        | 
  
### Hosts
| mgmt         | inside         | groups     | inventory_hostname |  
| :----------: | :------------: | :--------: | :----------------: |
| `10.1.0.100` | `10.1.2.100`   | `tower`    | `host01-tower`     |  
| `10.1.0.99`  | `10.1.2.99`    | `linux`    | `host01-linux`     |  
| `10.1.0.98`  | `10.1.2.98`    | `windows`  | `host01-windows`   |   
| `10.1.0.97`  | `10.1.2.97`    | `nios`     | `host01-nios`      |  
| `10.1.0.96`  | `10.1.2.96`    | `splunk`   | `host01-splunk`    |  

## Howto

To provision the entire topology: `ansible-playbook provision.yaml`

To provision subset elements of the topology, just use the standard `--limit` option from `ansible-playbook` command-line, to specify the inventory group(s).

Examples:

Provisioning:  
*NOTE: Ensure the existence of every required resources: `vpc, subnets, gateways, enis, eips and instances`.*
- Cisco ASA and Linux:  
  `ansible-playbook provision.yaml --limit asa,linux`  

- F5, Infoblox and Linux:  
  `ansible-playbook provision.yaml --limit tmos,linux,nios`

- PaloAlto and Linux:  
  `ansible-playbook provision.yaml --limit panos,linux`

- Tower and Splunk:  
  `ansible-playbook provision.yaml --limit tower,splunk`

Unprovision:
*NOTE: Ensure the absence of instance resources: `enis, eips and instances.`.*

- Cisco ASA and Linux:
  `ansible-playbook unprovision.yaml --limit asa,linux`

- F5, Infoblox and Linux:  
  `ansible-playbook unprovision.yaml --limit tmos,linux,nios`

- PaloAlto and Linux:  
  `ansible-playbook unprovision.yaml --limit panos,linux`

- Tower and Splunk:  
  `ansible-playbook unprovision.yaml --limit tower,splunk`

Destroy:  
*NOTE: Ensure the absence of every resource containing `"tag:Environment:": "{{ ec2_vpc_name }}"`*
  *  `ansible-playbook teardown.yaml`  
