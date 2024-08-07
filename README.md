# Ansible role saslauthd

Configure Cyrus saslauthd deamon.


## Features

- Idempotent.
- Able to manage all authentification mechanisms and their options.
- Configure `/etc/saslauthd.conf` and `/etc/ldap/ldap.conf` files for LDAP authentification mechanism.
- Debian friendly (Ubuntu soon, anyone for Redhat likes and other platforms?).
- A developer/maintainer willing to receive feedback and bug reports.


## Requirements

This role must be run as `root` but will **not** `become` by itself.


## Role Variables

| Name                          | Default             | Description                                                                           |
|-------------------------------|---------------------|---------------------------------------------------------------------------------------|
| `saslauthd_mechanism`         | `"pam"`             | Authentification mechanism.                                                           |
| `saslauthd_mech_options`      | `""`                | Mechanism specific options (`-O` of `saslauthd`, see `saslauthd(8)`).                 |
| `saslauthd_threads`           | `5`                 | Use threads processes for responding to authentication queries (`-n`).                |
| `saslauthd_options`           | `"-c -m /var/run/saslauthd"` | All other options for `saslauthd` (see `saslauthd(8)`).                      |
| `saslauthd_mech_ldap_servers` | required if `saslauthd_mechanism` is `ldap` | Arrays of LDAP servers (`ldap_servers` in `saslauthd.conf`).  |
| `saslauthd_mech_ldap_search_base` | required if `saslauthd_mechanism` is `ldap` | Search base for LDAP requests (`ldap_search_base` in `saslauthd.conf`). |
| `saslauthd_mech_ldap_config`  | `{ldap_version: 3}` | Options for `ldap` mechanism to be writen into `/etc/saslauthd.conf`.                 |
| `saslauthd_lib_ldap_config`   | `{TLS_CACERT: "/etc/ssl/certs/ca-certificates.crt"}` | Options for LDAP library to be writen into `/etc/ldap/ldap.conf` (see `ldap.conf(5)`). |


## Dependencies

None.


## Example Playbooks

Minimal playbook:

```
- name: Minimal playbook for role seb4itik.saslauthd (default mechanism "pam")
  hosts: mail
  roles:
    - "seb4itik.saslauthd"
```

More complete example:

```
- name: Example playbook for role seb4itik.saslauthd (mechanism "ldap")
  hosts: mail
  vars:
    saslauthd_mechanism: "ldap"
    saslauthd_threads: 10
    saslauthd_mech_ldap_servers: ["ldaps://ldap.{{ my_domain }}/"]
    saslauthd_mech_ldap_search_base: "{{ my_ldap_base_dn }}"
  roles:
    - "seb4itik.saslauthd"
```


## TODO

FIXME
- Write tests.
- Other platforms (Ubuntu, Redhat, ...).


## License

MIT


## Author Information

- [seb4itik](https://github.com/seb4itik)
