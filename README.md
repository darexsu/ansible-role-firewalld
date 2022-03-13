# Ansible role firewalld
[![CI Molecule](https://github.com/darexsu/ansible-role-firewalld/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-firewalld/actions/workflows/ci.yml)&emsp;![](https://img.shields.io/static/v1?label=idempotence&message=ok&color=success)&emsp;![Ansible Role](https://img.shields.io/ansible/role/d/57564?color=blue&label=downloads)

|  Testing         | Ready for use      |
| :--------------: | :--------------:   | 
| Debian 11        |:heavy_check_mark:  |
| Debian 10        |:heavy_check_mark:  | 
| Ubuntu 20.04     |:heavy_check_mark:  |  
| Ubuntu 18.04     |:heavy_check_mark:  | 
| Oracle Linux 8   |:heavy_check_mark:  |
| Rocky Linux 8    |:heavy_check_mark:  | 

### 1) Install role from Galaxy
```
ansible-galaxy install darexsu.firewalld --force
```

### 2) Example playbooks:
  
  - [full playbook](#full-playbook)  
    - install
      - [official repo](#install-from-official-repo) 
    - config
      - [add firewall rules](#add-firewall-rules)


Replace or Merge dictionaries (with "hash_behaviour=replace" in ansible.cfg):
```
# Replace             # Merge
---                   ---
  vars:                 vars:
    dict:                 merge:
      a: "value"            dict: 
      b: "value"              a: "value" 
                              b: "value"

# How does merge work?
Your vars [host_vars]  -->  default vars [current role] --> default vars [include role]
  
  dict:          dict:              dict:
    a: "1" -->     a: "1"    -->      a: "1"
                   b: "2"    -->      b: "2"
                                      c: "3"
    
```

##### Full playbook
```yaml

---
- hosts: all
  become: true

  vars:
    merge:
      # FirewallD
      firewalld:
        enabled: true
        service:
          enabled: true
          state: "started"

      # FirewallD -> install
      firewalld_install:
        enabled: true
        packages: [firewalld]

      # FirewallD -> rules
      firewalld_rules:
        port_80:
          enabled: true
          zone: "public"
          state: "enabled"
          port: "80/tcp"
          permanent: true
          immediate: true
        service_http:
          enabled: true
          zone: "public"
          state: "enabled"
          service: "http"
          permanent: true
          immediate: true
        service_https:
          enabled: true
          zone: "public"
          state: "enabled"
          service: "https"
          permanent: true
          immediate: true
      # ...
  tasks:
    - name: role darexsu firewalld
      include_role:
        name: darexsu.firewalld

```
##### install-from-official-repo
```yaml

---
- hosts: all
  become: true

  vars:
    merge:
      # FirewallD
      firewalld:
        enabled: true

      # FirewallD -> install
      firewalld_install:
        enabled: true
        packages: [firewalld]

  tasks:
    - name: role darexsu firewalld
      include_role:
        name: darexsu.firewalld

```
##### add firewall rules
```yaml

---
- hosts: all
  become: true

  vars:
    merge:
      # FirewallD
      firewalld:
        enabled: true
        service:
          enabled: true
          state: "started"

      # FirewallD -> rules
      firewalld_rules:
        port_80:
          enabled: true
          zone: "public"
          state: "enabled"
          port: "80/tcp"
          permanent: true
          immediate: true
        service_http:
          enabled: true
          zone: "public"
          state: "enabled"
          service: "http"
          permanent: true
          immediate: true
        service_https:
          enabled: true
          zone: "public"
          state: "enabled"
          service: "https"
          permanent: true
          immediate: true
      # rule_name:
      #   enabled: true
      #   key: value
      #   ...
  tasks:
    - name: role darexsu firewalld
      include_role:
        name: darexsu.firewalld

```