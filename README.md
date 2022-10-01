# Ansible role Cloudflare DNS

This is a role I use to manage my dynamic DNS and cloudflare entrypoints into my network. It is to be used as a utility to enhance DNS management for services.

## Usage

Include the role in your ansible requirements.yml

```yaml
---
roles:
  - src: git+http://github.com/f0rkznet/ansible-role-cloudflare-dns
    version: main
    name: f0rkznet.cloudflare_dns
```

Configure your API credentials:

```yaml
---
cloudflare_account_email: foo@bar.net
cloudflare_account_api_key: some-api-key-thing-here
```

Configure your zone or zones:

```yaml
---
# root zones
# A proxied host will be created with root.foobar.net and your public IP
# A proxied CNAME will be created for foobar.net pointing to root.foobar.net
root_zones:
  - foobar.net
# Public Zones
# These are zones that are proxied through cloudflare set up as CNAME to @
public_zones:
  - record: www
    zone: foobar.net
# Custom Zones
# These are zones that you want more configurable control
custom_zones:
  - record: node00.sub.foobar.net
    zone: foobar.net
    proxied: yes
    type: A
    value: 123.123.123.111
```

In a playbook:

```yaml
---
- name: Manage DNS for foobar
  hosts: localhost
  become: false
  gather_facts: false
  vars:
    root_zones:
      - foobar.net
    public_zones:
      - record: www
        zone: foobar.net
    custom_zones:
      - record: node00.sub.foobar.net
        zone: foobar.net
        proxied: no
        type: A
        value: 10.10.10.10
  roles:
    - f0rkznet.cloudflare_dns
```
