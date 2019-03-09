# Overview

Folder to store licence files required by devices.

## Instructions
- *Local* content _synchronized_ based on `.gitignore`:
  - **.gitignore**
  - **README\***
- The **filename** containing the license **MUST `perfect match`** the **name** of `environment`.
  - Ex:
    - _License_ filename is `tower` for environment `tower`.

## HINT

The `build` executes locally in the machine where the playbook runs, and will try to download files, solve pre-reqs using the role `victorock.download`, so if you are using `Red Hat Ansible Tower`, consider hosting the licenses in an **non-public** object store and specify the `download_*` variables as per [documentation](https://galaxy.com/victorock/download).

_If you need to download something as part of your **application deployment**, for better performance, do it in `deploy_<application>`._