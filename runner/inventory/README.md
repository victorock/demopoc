# Overview
This is the inventory containing the devices and topologies to provision and deploy.

## Instructions

- The inventory **hosts** file is in ini format.  
- The following tree of groups:
  - all:
    - host -> Contains nodes that are `provision_role:` **host**
      - _applications..._
    - network -> Contains nodes that are `provision_role:` **network**
      - firewall -> Firewall `applications`
        - _applications..._
      - loadbalance -> LoadBalance `applications`
        - _applications..._
      - router -> Router `applications`
        - _applications..._