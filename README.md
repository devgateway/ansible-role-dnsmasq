# Dnsmasq role for Ansible

Configure Dnsmasq server and its zones

## Role Variables

### `dnsmasq_config`

Dictionary. Any options supported by Dnsmasq can go here.

Keys are Dnsmasq option names, hyphens and underscores are treated the same, i.e. `no_resolv` and `no-resolv` are the same option.

Values can be boolean, literals, or arrays.

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

License
-------

GPLv3 or later

Author Information
------------------

2017, Development Gateway
