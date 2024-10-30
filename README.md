VH Proxmox VE VM Template Ansible Role
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

| Variable | Description | Required | Default Value |
| - | - | - | - |
| `pve_api_user` | Set the API user with which the API commands are executed | true | `root@pam` |
| `pve_api_token_id` | Set the API token to be used from the `pve_api_user`. This is the part behind the “!” | true | Not set |
| `pve_api_token_secret` | Set the API token secret of the api token | true | Not set |
| `pve_vm_id` | Set the VMID with which the template is to be created | true | Not set |

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

N/A
