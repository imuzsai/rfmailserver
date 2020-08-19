# ansible-role-roundcube

Install and configure Roundcube.
This includes configuration of the Roundcube Getmail Plugin.
You can choose a custom skin `larry-custom` that has additional links to accout settings.

## Dependencies

You need to install a web and database server before using this role.
Also, you need to configure the webserver to to deliver vhost `{{ roundcube_domain }}`.

## Role Variables

```
roundcube_repo: https://github.com/roundcube/roundcubemail
roundcube_version: "1.3.9"
roundcube_domain: mail.systemli.org
roundcube_username_domain: systemli.org
roundcube_product_name: Systemli Webmail
roundcube_support_url: https://www.systemli.org/kontakt.html
roundcube_path: /var/www/{{ roundcube_domain }}/www
roundcube_user: roundcube
roundcube_user_shell: /bin/false
roundcube_user_home: /var/www/{{ roundcube_domain }}
roundcube_mysql_user: roundcube
roundcube_mysql_password: "{{ roundcube_mysql_password_enc }}"
roundcube_mysql_db: roundcubemail
roundcube_default_host: localhost
roundcube_skin: larry
roundcube_smtp_server: 'tls://{{ roundcube_domain }}'
roundcube_smtp_port: 25
roundcube_smtp_user: '%u'
roundcube_smtp_pass: '%p'
roundcube_identities_level: 1
roundcube_date_format: d.m.Y
roundcube_cipher_method: AES-256-CBC
roundcube_password_charset: UTF-8
roundcube_sendmail_delay: 4
roundcube_max_recipients: 250
roundcube_draft_autosave: 60

roundcube_plugins:
 - archive
# - carddav
 - enigma
 - getmail
 - legacy_browser
 - managesieve
 - zipdownload

# list of additional configs, e.g. for plugins
roundcube_configs_extra:
  - plugins/enigma/config.inc.php
  - plugins/carddav/config.inc.php

# enigma plugin settings
enigma_password_time: 1

roundcube_dependencies:
  - "{{ 'php5-intl' if ansible_distribution_major_version|int <= 8 else 'php-intl' }}"
  - "{{ 'php5-mcrypt' if ansible_distribution_major_version|int <= 8 else 'php-mcrypt' }}"
  - "{{ 'php5-mysql' if ansible_distribution_major_version|int <= 8 else 'php-mysql' }}"
  - "{{ 'php5-curl' if ansible_distribution_major_version|int <= 8 else 'php-curl' }}" # rcmcarddav
  - python-mysqldb
  - git

# getmail
roundcube_getmail_repo: https://github.com/t2d/roundcube-getmail.git
```

## Download

Download latest release with `ansible-galaxy`

$ ansible-galaxy install systemli.roundcube

## Example Playbook

```
- hosts: servers
  roles:
    - { role: systemli.roundcube }
```

## License

GPL

## Author Information

https://www.systemli.org
