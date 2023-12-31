# Nutanix Role to create or delete an AHV subnet (port group)

This Ansible role will create or delete a defined subnet on a Nutanix cluster running AHV.

## Role Variables

| Variable                                           | Required | Default  | Choices                                                                         | Comments                                                                                                                                                                                                                          |
|----------------------------------------------------|----------|----------|---------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| role_nutanix_prism_network_subnet_host                | yes      |          |                                                                                 | The IP address or FQDN for the Prism (Element or Central) to which you want to connect.                                                                                                                                           |
| role_nutanix_prism_network_subnet_host_username       | yes      |          |                                                                                 | A valid username with appropriate rights to access the Nutanix API.                                                                                                                                                               |
| role_nutanix_prism_network_subnet_host_password       | yes      |          |                                                                                 | A valid password for the supplied username.                                                                                                                                                                                       |
| role_nutanix_prism_network_subnet_host_port           | no       | 9440     |                                                                                 | The Prism TCP port.                                                                                                                                                                                                               |
| role_nutanix_prism_network_subnet_host_validate_certs | no       | false    | true / false                                                                    | Whether to check if Prism UI certificates are valid.                                                                                                                                                                              |
| role_nutanix_prism_network_subnet_debug               | no       | false    | true / false                                                                    | Enable debug logging.                                                                                                                                                                                                             |
| role_nutanix_prism_network_subnet_list                | no       | []       |                                                                                 |                                                                                                                                                                                                                                   |

### role_nutanix_prism_network_subnet_list dict format

| Variable                       | Required | Default  | Choices                                                                         | Comments                                                                                                                                                                                                                          |
|--------------------------------|----------|----------|---------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                           | yes      |          |                                                                                 | The name of the virtual switch                                                                                                                                                                                                    |
| state                          | yes      |          | [ present , absent ]                                                            | Whether the switch should be created or removed                                                                                                                                                                                   |
| virtual_switch                 | no       | vs0      |                                                                                 | The virtual switch where the new subnet should be created                                                                                                                                                                         |
| vlan_id                        | yes      |          |                                                                                 | The VLAN ID for the new subnet                                                                                                                                                                                                    |
| net_address                    | no       |          |                                                                                 | Required for AHV IPAM. The network address in CIDR format (eg. 192.168.0.0/24)                                                                                                                                                    |
| gateway                        | no       |          |                                                                                 | Required for AHV IPAM. The gateway for the new network                                                                                                                                                                            |
| dns_server_list                | no       |          |                                                                                 | AHV IPAM. List of DNS servers for the new network                                                                                                                                                                                 |
| domain_name                    | no       |          |                                                                                 | AHV IPAM. Domain suffix for all servers on this subnet                                                                                                                                                                            |
| domain_search                  | no       |          |                                                                                 | AHV IPAM. Domain search string for all servers on this subnet                                                                                                                                                                     |
| ipam.start_ip                  | no       |          |                                                                                 | AHV IPAM. Start address for AHV IPAM                                                                                                                                                                                              |
| ipam.end_ip                    | no       |          |                                                                                 | AHV IPAM. End address for AHV IPAM                                                                                                                                                                                                |

## Dependencies

None

## Example Playbooks

This playbook will create a subnet called Primary on the default VLAN.

```YAML
- hosts: localhost
  gather_facts: false
  roles:
    - role: grdavies.role_nutanix_prism_network_subnet_host
  vars:
    role_nutanix_prism_network_subnet_host: 10.38.185.37
    role_nutanix_prism_network_subnet_host_username: admin
    role_nutanix_prism_network_subnet_host_password: nx2Tech165!
    role_nutanix_prism_network_subnet_list:
      - name: Primary
        state: present
        virtual_switch: vs0
        vlan_id: 0
```

This playbook will remove a subnet called Primary.

```YAML
- hosts: localhost
  gather_facts: false
  roles:
    - role: grdavies.role_nutanix_prism_network_subnet_host
  vars:
    role_nutanix_prism_network_subnet_host: 10.38.185.37
    role_nutanix_prism_network_subnet_host_username: admin
    role_nutanix_prism_network_subnet_host_password: nx2Tech165!
    role_nutanix_prism_network_subnet_list:
      - name: Primary
        state: absent
```

This playbook will create a subnet called Primary configured with AHV IPAM.

```YAML
- hosts: localhost
  gather_facts: false
  roles:
    - role: grdavies.role_nutanix_prism_network_subnet_host
  vars:
    role_nutanix_prism_network_subnet_host: 10.38.185.37
    role_nutanix_prism_network_subnet_host_username: admin
    role_nutanix_prism_network_subnet_host_password: nx2Tech165!
    role_nutanix_prism_network_subnet_list:
      - name: Primary
        state: present
        virtual_switch: vs0
        vlan_id: 0
        net_address: 10.38.186.0/25
        gateway: 10.38.186.1
        dns_server_list:
          - 10.42.194.10
        domain_name: ntnxlab.local
        domain_search: ntnxlab.local
        ipam:
          - start_ip: 10.38.185.50
            end_ip: 10.38.185.126
```

## License

See LICENSE.md

## Author Information

Ross Davies
