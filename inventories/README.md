# Overview
Each folder contains the inventory with the devices and corresponding topology.

## Instructions

- The inventory file **hosts** is in YAML format.  
- The following groups are being used by default:
  - all:
    - host -> Nodes that are of type **host**
      - tower -> Red Hat Ansible Tower
      - linux -> Red Hat Enterprise Linux
      - windows -> Microsoft Windows
      - splunk -> Splunk ES
      - nios -> Infoblox
    - network -> Nodes that are of type **network**
      - firewall -> Firewalls Appliances
        - asa -> Cisco ASA
        - fortios -> Fortinet
        - panos -> Palo Alto
      - loadbalance -> LoadBalance Appliances
        - tmos -> F5 TMOS
      - router -> Router Appliances
        - ios -> Cisco IOS
    - sites -> Group devices by location.
      - site1 -> By default nodes are located in site1.
      - site2 -> Site2 if needed to balance nodes among multiple sites or build multisite topologies.
      - site3 -> Site3 if needed to balance nodes among multiple sites or build multisite topologies.