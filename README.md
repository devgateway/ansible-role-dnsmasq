# Dnsmasq role for Ansible

Configure Dnsmasq server and its zones

## Role Variables

Both variables are optional.

### `dnsmasq_config`

Any options supported by Dnsmasq can go here. Replace hyphens with underscores in options names, e.g. `no-resolv` becomes `no_resolv`. Values can be boolean, literals, or arrays.

### `dnsmasq_zones`

This is a list of dictionaries, each representing an authority zone. Only `domain` member is mandatory. Optional members are `subnets` and `hosts`.

`subnets` is a list of subnets in a zone, see `auth-zone` in `dnsmasq(8)`.

`hosts` is a dictionary, where keys are *unqualified* host names. Values are dictionaries where `address` is mandatory IP address, and `cnames` is a list of *unqualified* aliases.

Example Playbook
----------------

    - hosts: dns
      roles:
        - dnsmasq
      vars:
        dnsmasq_config:
            no_resolv: true
            domain_needed: true
            bogus_priv: true
            server:
                - 8.8.4.4
                - 8.8.8.8
        dnsmasq_zones:
            - domain: user.example.net
              subnets:
                  - 10.128.0.0/24
            - domain: empty.example.net
            - domain: mgmt.example.net
              subnets:
                  - 10.128.1.0/24
              hosts:
                  gateway:
                      address: 10.128.1.1
                      cnames:
                          - router
                          - ntp
                  keystone:
                      address: 10.128.1.2

License
-------

GPLv3 or later

Author Information
------------------

2017, Development Gateway
