---
type:               "howto"
title:              "Basic Nginx/PHP environment"
date:               "2015-07-21"
publishdate:        "2015-07-21"
draft:              false
slug:               "basic-nginx-php-environment"
description:        "This recipe handle the basic configuration of a PHP environment (with nginx as web server)"

---

This recipe is quite simple and have as a goal to bootstrap a simple box with common administration tools and a web server (Nginx).

# Playbook definition

This time our playbook is far simple and look like the following:

{{< highlight yaml >}}
---

- hosts: all
  sudo:  yes

  roles:
    - { role: elao.php }
    - { role: elao.nginx }

{{< /highlight >}}

2 roles are involved in this setup PHP and Nginx, we don't need anything else.

# Configuration

## PHP

Even if configuration is most of the time different for each project we can afford to base our example on a common PHP configuration.

{{< highlight yaml >}}

#######
# Php #
#######

elao_php_sapis: ['cli', 'fpm']

elao_php_extensions:
  - curl
  - mysqlnd
  - intl
  - gd

elao_php_configs_exclusive: true
elao_php_configs:
  - file: 10-extensions.ini
    config:
     - zend_extension: opcache.so
     - extension: curl.so
     - extension: mysqlnd.so
     - extension: mysqli.so
     - extension: pdo_mysql.so
     - extension: pdo.so
     - extension: intl.so
     - extension: gd.so
  - file: 20-config.ini
    config:
      - date.timezone: UTC
      # App
      - upload_max_filesize: 80M
      - post_max_size: 80M

elao_php_fpm_configs:
- file: 20-config_fpm.ini
  template: "configs/{{ _env }}.ini.j2"
  config:
    - short_open_tag: 'On'

elao_php_fpm_pools_exclusive: true
elao_php_fpm_pools:
  - file:     www.conf
    template: fpm_pools/www_{{ _env }}.conf.j2
    config:
      - listen:               127.0.0.1:9000
      - pm:                   dynamic
      - pm.max_children:      150
      - pm.start_servers:     5
      - pm.min_spare_servers: 5
      - pm.max_spare_servers: 20
{{< /highlight >}}

## Nginx

{{< highlight yaml >}}

#########
# Nginx #
#########

elao_nginx_config_template: "{{ playbook_dir ~ '/templates/nginx/nginx.conf.j2' }}"

elao_nginx_configs_exclusive: true
elao_nginx_configs:
  # Log format
  - file: logformat.conf
    config:
      - log_format: "elao '{{ _nginx_logformat }}'"

  # Php fpm
  - file:     php_fpm_params
    template: configs/php_fpm_params_{{ _env }}.j2
    config:
      - fastcgi_pass: 127.0.0.1:9000

  # App
  - file: tuxboard.conf
    template: configs/server_{{ _env }}.conf.j2
    config:
      - listen:      "{{ ansible_venet0_0.ipv4.address }}:80"
      - server_name: tuxboard.com
      - rewrite:     ^  http://www.$server_name$request_uri? permanent
  - file: www.tuxboard.conf
    template: "{{ playbook_dir ~ '/templates/nginx/' ~ ('back.conf.j2' if _back else 'front.conf.j2') }}"

{{< /highlight >}}

