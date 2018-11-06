# Dnsmasq role for Ansible

Configure Dnsmasq and its zones.

## Role Variables

All role variables are optional.

### `dnsmasq_config`

Dictionary. Any options supported by Dnsmasq can go here.

Keys are Dnsmasq option names.

Values can be boolean, literals, or arrays. Arrays yield one directive per each value.

### `dnsmasq_zones`

A list of dictionaries, each representing an authority zone, see `auth-zone` in `dnsmasq(8)`. The
member `hosts` is required, `subnets` and `cnames` are optional.

`subnets` is a list of subnets like `192.168.0.0/24`.

`hosts` is a dictionary, where keys are unqualified host names, and values are IP addresses.

`cnames` is a dictionary, where keys are unqualified host names (targets), and values are arrays of
unqualified aliases.

### `dnsmasq_config`

Path to the main Dnsmasq configuration file.

Default:

    Debian: /etc/dnsmasq.conf
    RedHat: /etc/dnsmasq.conf

### `dnsmasq_conf_d`

The directory for authority zone definitions.

Default:

    Debian: /etc/dnsmasq.d
    RedHat: /etc/dnsmasq.d

### `zone_path`

The directory where `hosts(5)` files will be stored.

Default:

    Debian: /etc/dnsmasq.d/hosts
    RedHat: /etc/dnsmasq.d/hosts

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
            - 1.1.1.1
            - 8.8.8.8
            - 8.8.4.4
        dnsmasq_zones:
          example.net:
            hosts:
              gateway: 10.128.1.1
              keystone: 10.128.1.2
            cnames:
              gateway:
                - ntp
                - firewall
                - router
            subnets:
              - 10.128.1.0/24

License
-------

GPLv3 or later

Author Information
------------------

2017-2018, Development Gateway
