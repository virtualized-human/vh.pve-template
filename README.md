Proxmox VE Cloud-Image VM Template Ansible Role
=========

The goal of this role is to roll out already existing / prebuild images on Proxmox nodes in the form of a Proxmox VM template in a simple way. Additionally you can automatically customize images.

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

```yaml
- name: Deploy Proxmox VE Cloud-Image Template
  hosts:
    - localhost # must be defined since its the default build-host which prepares the image, or changed here and in var "template_build_host"
    - pve
  become: true
  tasks:
    - name: Create Ubuntu 26.04 Template
      include_role:
        name: vh.pve-template
      vars:
        pve_vm_id: "11000"
        pve_vm_name: "Ubuntu-26.04-LDAP"
        template_image_url: "https://cloud-images.ubuntu.com/resolute/current/resolute-server-cloudimg-amd64.img"
        template_image_custom_cmd_script:
          - ../files/pve-templates/cmd.sh # see example cmd script
        template_image_custom_firstboot_script:
          - ../files/pve-templates/firstboot.sh # see example firstboot script
        template_image_custom: "true"
```

### Example Custom Command Script

Tested on Ubuntu 26.04 Cloudimg.

```bash
#!/bin/bash
## Refresh Repos
DEBIAN_FRONTEND=noninteractive apt update -y
## Install guest image
DEBIAN_FRONTEND=noninteractive apt install -y qemu-guest-agent
## Enable Root Login via SSH
# sed -i 's/disable_root: true/disable_root: false/' /etc/cloud/cloud.cfg
# passwd -d root
# passwd -l root
# sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
## Reset Machine-ID
# truncate -s 0 /etc/machine-id
## Install Other Customizations...
### Resize Disk image in combination with ansible var "template_image_resize"
# growpart /dev/sda 1
# resize2fs /dev/sda1
# apt install -y firefox
```

### Example Firstboot Command Script

Tested on Ubuntu 26.04 Cloudimg.

```bash
#!/bin/bash
# Configure Cloud-Init for Proxmox Environment (defaults)
sed -i 's/^preserve_hostname: false/preserve_hostname: true/' /etc/cloud/cloud.cfg
sed -i 's/^ - update_etc_hosts/# - update_etc_hosts/' /etc/cloud/cloud.cfg
sed -i '1i ssh_deletekeys: false' /etc/cloud/cloud.cfg
```

License
-------

GPL