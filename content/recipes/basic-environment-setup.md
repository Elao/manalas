---
type:               "recipe"
title:              "Basic environment setup"
date:               "2015-07-21"
publishdate:        "2015-07-21"
draft:              false
slug:               "basic-environment-setup"
description:        "This recipe handle the basic configuration of a environment"

---

This recipe is about basic setup of a linux box, we are using it on most of our containers to ensure we have a fully usable environment.

{{< highlight yaml >}}

# Playbook
- hosts: all
  sudo: yes
  vars_files:
    - group_vars/all_{{ ansible_distribution|lower }}.yml
 roles:
    - { role: elao.apt, tags: ['apt'],
        elao_apt_update: true,
        elao_apt_repositories: "{{ _all_apt_repositories }}",
        elao_apt_preferences:  "{{ _all_apt_preferences }}",
        elao_apt_packages:     "{{ _all_apt_packages }}"
    - { role: elao.users, tags: ['users'],
        elao_users: "{{ _all_users }}",
        elao_users_authorized_keys: "{{ _all_users_authorized_keys }}"
      }
    - { role: elao.ssh }
    - { role: elao.motd }
    - { role: elao.timezone }
    - { role: elao.vim }
    - { role: elao.git }
    - { role: elao.zsh }
    - { role: elao.ohmyzsh, tags: ['users'] }
    - { role: elao.alternatives }
    
{{< /highlight >}}


# Configuration

