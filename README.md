# Dnsmasq role for Ansible

Configure Dnsmasq server and its zones

## Role Variables

All role variables are optional.

### `dnsmasq_config`

Dictionary. Any options supported by Dnsmasq can go here.

Keys are Dnsmasq option names, hyphens and underscores are treated the same, i.e. `no_resolv` and `no-resolv` are the same option.

Values can be boolean, literals, or arrays.

### `dnsmasq_zones`

A list of dictionaries, each representing an authority zone, see `auth-zone` in `dnsmasq(8)`. The members `domain` and `hosts` are mandatory, `subnets` is optional.

`subnets` is a list of subnets in a zone.

`hosts` is a dictionary, where keys are *unqualified* host names. Values are dictionaries where `address` is mandatory IP address, and `cnames` is an optional list of *unqualified* aliases.

### `dnsmasq_config`

Path to the main Dnsmasq configuration file.

### `dnsmasq_conf_d`

Path to the directory for additionally included config files. Zone definition files (in `hosts(5)` format) will be generated in `hosts` subdirectory here.

Example Playbook
----------------

    - hosts: dns
      roles:
        - dnsmasq
      vars:
        dnsmasq_settings:
          no-hosts: true # don't parse /etc/hosts
          no-resolv: true # ignore /etc/resolv.conf
          domain-needed: true # don't forward unqualified domain lookups
          bogus-priv: true # don't forward private reverse lookups
          server:
            - 8.8.8.8
            - 8.8.4.4
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
