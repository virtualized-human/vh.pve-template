Proxmox VE Cloud-Image VM Template Ansible Role
=========

The goal of this role is to roll out the latest cloud images on Proxmox nodes in the form of a Proxmox VM template in a simple way and with a lot of customization. A lot has been variabilized. If something is missing, I will add it.

Requirements
------------

Not known. But you need ansible!

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

    - name: Deploy Proxmox VE Cloud-Image Template
      hosts: pve
      become: true
      tasks:
        - name: Create Debian 12 Template
          include_role:
            name: vh.pve-template
          vars:
            pve_vm_id: "9000"
            pve_vm_name: "Debian-12"
            template_image_url: "https://cloud.debian.org/images/cloud/bookworm/latest/debian-12-generic-amd64.qcow2"

License
-------

GPL