# OpenERP

Ansible role to install OpenERP 6.0, 6.1 or 7.0 from a Git or Mercurial
repository, and configure it.

## Systems supported

- Debian Wheezy (7.0)
- Ubuntu Precise (12.04)
- Ubuntu Trusty (14.04)

## Variables

```yaml
openerp_service: openerp-server
openerp_version: 7.0                                # 6.0 / 6.1 / 7.0
openerp_user: openerp
openerp_user_passwd: openerp
openerp_user_system: False
openerp_logdir: "/var/log/{{ openerp_user }}"
openerp_workdir: "/home/{{ openerp_user }}/oerp"
openerp_rootdir: "/home/{{ openerp_user }}/oerp/server"
openerp_init: True
openerp_config_file: "/home/{{ openerp_user }}/{{ openerp_service }}.conf"
openerp_force_config: False
openerp_repo_type: git     # git or hg
openerp_repo_url: https://github.com/odoo/odoo.git
openerp_repo_dest: "{{ openerp_rootdir }}"
openerp_repo_rev: 7.0
openerp_repo_update: True   # Update the working copy or not. This option is
                            # ignored on the first run (a checkout of the
                            # working copy is always processed on the given
                            # revision)
                            # WARNING: uncommited changes will be discarded!

# OpenERP parameters
openerp_config_addons_path: "/home/{{ openerp_user }}/oerp/server/addons,/home/{{ openerp_user }}/oerp/server/openerp/addons"
openerp_config_admin_passwd: admin
openerp_config_cache_timeout: 100000                # ignored for 7.0
openerp_config_cpu_time_limit: 60                   # 6.1 only
openerp_config_csv_internal_sep: ','
openerp_config_db_host: False
openerp_config_db_host_user: "{{ ansible_ssh_user }}"
openerp_config_db_maxconn: 64
openerp_config_db_name: False
openerp_config_db_passwd: False
openerp_config_db_port: False
openerp_config_db_template: template1               # ignored for 6.0
openerp_config_db_user: "{{ openerp_user }}"
openerp_config_dbfilter: '.*'                       # ignored for 6.0
openerp_config_debug_mode: False
openerp_config_email_from: False
openerp_config_limit_memory_hard: 805306368         # 7.0 only
openerp_config_limit_memory_soft: 671088640         # 7.0 only
openerp_config_limit_request: 8192                  # 7.0 only
openerp_config_limit_time_cpu: 60                   # 7.0 only
openerp_config_limit_time_real: 120                 # 7.0 only
openerp_config_list_db: True
openerp_config_log_level: info
openerp_config_logfile: None
openerp_config_logrotate: True
openerp_config_max_cron_threads: 2                  # ignored for 6.0
openerp_config_netrpc: True                         # ignored for 7.0
openerp_config_netrpc_interface: ''                 # ignored for 7.0
openerp_config_netrpc_port: 8070                    # ignored for 7.0
openerp_config_osv_memory_age_limit: 1.0
openerp_config_osv_memory_count_limit: False
openerp_config_pidfile: None
openerp_config_proxy_mode: False                    # ignored for 6.0
openerp_config_secure_cert_file: server.cert
openerp_config_secure_pkey_file: server.pkey
openerp_config_server_wide_modules: None            # ignored for 6.0
openerp_config_smtp_password: False
openerp_config_smtp_port: 25
openerp_config_smtp_server: localhost
openerp_config_smtp_ssl: False
openerp_config_smtp_user: False
openerp_config_static_http_document_root: None
openerp_config_static_http_enable: False
openerp_config_static_http_url_prefix: None
openerp_config_syslog: False
openerp_config_timezone: False
openerp_config_translate_modules: ['all']
openerp_config_unaccent: True                       # ignored for 6.0
openerp_config_virtual_memory_limit: 805306368      # 6.1 only
openerp_config_virtual_memory_reset: 671088640      # 6.1 only
openerp_config_without_demo: False
openerp_config_workers: 0                           # 7.0 only
openerp_config_xmlrpc: True
openerp_config_xmlrpc_interface: ''
openerp_config_xmlrpc_port: 8069
openerp_config_xmlrpcs: True
openerp_config_xmlrpcs_interface: ''
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
    - openerp_repo_dest: /home/openerp/oerp/server
    - openerp_repo_rev: 7.0
```

When deploying, you can set the passwords with the `--extra-vars` option:

```sh
$ ansible-playbook -i servers servers.yml -l openerp_server --extra-vars "openerp_user_passwd=pAssWorD openerp_config_admin_passwd=SuPerPassWorD openerp_config_db_passwd=PaSswOrd"
```
