# Overview
Put in here your ssh key to install in the nodes, otherwise we'll generate one automatically.

## Instructions

- *Local* content _synchronized_ based on `.gitignore`:
  - **.gitignore**
  - **README\***
- The **filename MUST `perfect match`** the value of variable `ec2_vpc_name` (Default: "demobox")  
- The **file** containing the **public-key MUST end with .pub**

## HINT

The `build` executes locally in the machine where the playbook runs, and will try to download files using the role `victorock.download`, so if you are using `Red Hat Ansible Tower`, consider hosting any required licenses, binaries, etc in an **NON-PUBLIC** object store and specify the `download_*` variables as per [documentation](https://galaxy.com/victorock/download).

_If you need to download something as part of your **application deployment**, for better performance, do it in `deploy_<application>`._