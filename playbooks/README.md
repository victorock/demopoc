# Summary
Playbooks to manage disposable topologies.

#### Build
What the [`Build`](build.yaml) playbook does ?  
[`Build`](build.yaml) **environment's** resources **locally**:
- `download files...`
- `create files...`
- `compile code...`
- `etc...`

#### Provision
What the [`Provision`](provision.yaml) playbook does ?  
> [`Provision`](provision.yaml) the following [environment's](####Environments) resources:  
> - [Amazon Web Services](https://aws.amazon.com):
>   - [`vpc`](roles/provision_ec2/present/)
>   - [`subnets`](roles/provision_ec2/present)
>   - [`gateways`](roles/provision_ec2/present/)
>   - [`enis`](roles/provision_ec2/present/)
>   - [`eips`](roles/provision_ec2/present/)
>   - [`instances`](roles/provision_ec2/present/)  

> There are two types of `nodes` to provision in [`provision_ec2`](roles/provision_ec2):
> - [`network`](roles/provision_ec2/present/network.yaml) with **3 Network interfaces**.
> - [`host`](roles/provision_ec2/present/host.yaml) with **2 Network interfaces**.

#### Deploy
What the [`Deploy`](deploy.yaml) playbook does ?  
> [`Deploy`](deploy.yaml) the [**environments**](####Environments).  
> For each environment the role `deploy_<environment>` shall exist to perform environment specific tasks.
> Follow below roles in charge of environment specific deployments:  
>   - [`deploy_linux`](projet/roles/deploy_linux)
>   - [`deploy_tower`](projet/roles/deploy_tower)
>   - [`deploy_splunk`](projet/roles/deploy_splunk)
>   - [`deploy_nios`](projet/roles/deploy_nios)

#### Test
What the [`test`](test.yaml) playbook does ?  
> [`Test`](test.yaml) the deployed [**environments**](####Environments).  
> For each environment the role `test_<environment>` shall exist to perform environment specific tests.

#### Unprovision
What the [`Unprovision`](unprovision.yaml) playbook does ?  
> [`Unprovision`](unprovision.yaml) the following [environment's](####Environments) resources:  
> - [Amazon Web Services](https://aws.amazon.com):
>   - [`enis`](roles/provision_ec2/terminated/)
>   - [`eips`](roles/provision_ec2/terminated/)
>   - [`instances`](roles/provision_ec2/terminated/)  

#### Teardown
What the [`Teardown`](teardown.yaml) playbook does ?  
> [`Teardown`](teardown.yaml) the following [environment's](####Environments) resources:  
> - [Amazon Web Services](https://aws.amazon.com):  
>   - [`vpc`](roles/provision_ec2/absent/vpc.yaml)  
>   - [`subnets`](roles/provision_ec2/absentvpc.yaml)  
>   - [`gateways`](roles/provision_ec2/absent/vpc.yaml)  
>   - [`enis`](roles/provision_ec2/absent/vpc.yaml)  
>   - [`eips`](roles/provision_ec2/absent/vpc.yaml)  
>   - [`instances`](roles/provision_ec2/absent/vpc.yaml)  

#### Reprovision
What the [`Reprovision`](remain.yaml) playbook does ?  
> [`Terminate`](###Terminate) then [`Provision`](###Provision).  
