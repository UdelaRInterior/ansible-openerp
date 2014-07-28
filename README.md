# OpenERP

Ansible role to install OpenERP from a Git or Mercurial repository,
and configure it.

## Systems supported

- Debian Wheezy (7.0)
- Ubuntu Precise (12.04)
- Ubuntu Trusty (14.04)

## Variables

```yaml
openerp_service: openerp-server
openerp_version: 7.0
openerp_user: openerp
openerp_user_passwd: openerp
openerp_logdir: "/var/log/{{ openerp_user }}"
openerp_workdir: "/home/{{ openerp_user }}/openerp"
openerp_rootdir: "/home/{{ openerp_user }}/openerp/server"
openerp_init: True
openerp_databases: []      # [{name: prod, locale: 'en_US.UTF-8'}]
openerp_config_file: "/home/{{ openerp_user }}/{{ openerp_service }}.conf"
openerp_force_config: False
openerp_repo_type: git     # git or hg
openerp_repo_url: https://github.com/odoo/odoo.git
openerp_repo_dest: "{{ openerp_rootdir }}"
openerp_repo_rev: 8.0

# OpenERP parameters
openerp_config_admin_passwd: admin
openerp_config_addons_path: "/home/{{ openerp_user }}/openerp/server/addons,/home/{{ openerp_user }}/openerp/server/openerp/addons"
openerp_config_auto_reload: False
openerp_config_db_host: False
openerp_config_db_host_user: "{{ ansible_ssh_user }}"
openerp_config_db_port: False
openerp_config_db_user: openerp
openerp_config_db_passwd: False
openerp_config_dbfilter: '.*'
openerp_config_proxy_mode: False
openerp_config_unaccent: True
openerp_config_workers: 0
openerp_config_xmlrpc_port: 8069
openerp_config_xmlrpcs_port: 8071

# Extra options
openerp_user_sshkeys: False    # ../../path/to/public_keys/*
```


## Example (Playbook)

```yaml
- name: OpenERP Server
  hosts: openerp_server
  sudo: yes
  roles:
    - openerp
  vars:
    - openerp_config_db_host: pg_server
    - openerp_config_db_user: openerp
    - openerp_repo_type: git
    - openerp_repo_url: https://github.com/odoo/odoo.git
    - openerp_repo_dest: /home/openerp/openerp/server
    - openerp_repo_rev: 6.0
```

When deploying, you can set the passwords with the `--extra-vars` option:

```sh
$ ansible-playbook -i servers servers.yml -l openerp_server --extra-vars "openerp_user_passwd=pAssWorD openerp_config_admin_passwd=SuPerPassWorD openerp_config_db_passwd=PaSswOrd"
```
