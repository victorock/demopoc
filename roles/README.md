# Overview
Ansible Roles

## Instructions
- *Local* content _synchronized_ based on `.gitignore`:
  - **.gitignore**
  - **README\***
  - **deploy\***
  - **provision\***
  - **requirements.y\*l**
- *External* content:
  - Are *NOT* _synchronized_.
  - Roles *MUST* come from galaxy.ansible.com and not directly from scm.
  - Version *SHALL* be specified for better control.

## Roles
- build:
  - Wrapper of other build_* roles, based on value of variable `build_provider`.

- provision:
  - Wrapper of other build_* roles, based on value of variable `provision_provider`.

- deploy:
  - Wrapper of other build_* roles, based on value of variable `deploy_provider`.

- test:
  - Wrapper of other build_* roles, based on value of variable `test_provider`.
