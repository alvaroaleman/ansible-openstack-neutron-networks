# openstack-neutron-networks

A role to idempotently configure neutron networks, subnets and routers on
a per project basis.

## Synopsis

```yaml
- hosts: localhost
  gather_facts: false
  vars:
    os_auth:
      adminproject:
        username: adminproject_user
        password: adminproject_userpass
        auth_url: https://10.10.0.39:5000/v2.0
      project1:
        username: project1_user
        password: project1_userpass
        auth_url: https://10.10.0.39:5000/v2.0
    external_netname: 'physnet'
    os_networks:
      adminproject:
        - name: "{{ external_netname }}"
          provider_network_name: extnet
          allocation_pool:
            start: '192.168.0.70'
            end: '192.168.0.99'
          enable_dhcp: false
          gateway_ip: '102.168.0.1'
          cidr: '192.168.0.0/24'
          external: True
          dns_nameservers:
            - '8.8.8.8'
      project1:
        - name: privnet
          allocation_pool:
            start: '10.0.0.2'
            end: '10.0.0.254'
          cidr: '10.0.0.0/24'
          external_netname: "{{ external_netname }}"
          dns_nameservers:
            - 8.8.8.8
  roles:
    - alvaroaleman.openstack-neutron-networks
```

## Requirements

* [Python shade libraries](https://github.com/openstack-infra/shade/tree/master/shade)
* [Openstack cli tools](http://docs.openstack.org/cli-reference/common/cli_install_openstack_command_line_clients.html)

E.g. for Fedora:

```bash
sudo dnf copr enable larsks/python-shade
sudo dnf install python-shade python-openstackclient
```

## License

AGPLv3

## Author

* Alvaro Aleman