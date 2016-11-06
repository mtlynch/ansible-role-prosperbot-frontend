# Ansible Role: ProsperBot Frontend

[![Build Status](https://travis-ci.org/mtlynch/ansible-role-prosperbot-frontend.svg?branch=master)](https://travis-ci.org/mtlynch/ansible-role-prosperbot-frontend)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-prosperbotfrontend-blue.svg?style=flat-square)](https://galaxy.ansible.com/mtlynch/prosperbot-frontend)
[![License](http://img.shields.io/:license-apache-blue.svg?style=flat-square)](LICENSE)

## Role Variables

Available variables are listed below, along with default values (see [defaults/main.yml](defaults/main.yml)):

```yaml
prosperbot_frontend_user: prosperbot-frontend
prosperbot_frontend_group: prosperbot-frontend
```

The user and group under which to run PropserBot Frontend.

```yaml
prosperbot_frontend_gopath: "/home/{{ prosperbot_frontend_user }}/go"
prosperbot_frontend_goroot: /usr/local/go
```

The filesystem path for Go source files and Go binaries.

```yaml
prosperbot_frontend_static_root: /var/www/prosperbot-frontend
```

Location of prosperbot-frontend's static web files.

```yaml
prosperbot_frontend_app_port: 8082
```

Port on which prosperbot-frontend listens for application requests.

```yaml
prosperbot_frontend_web_port: 80
```

Port on which prosperbot-frontend listens for external web requests.

## Dependencies

* [joshualund.golang](https://galaxy.ansible.com/joshualund/golang/)
* [geerlingguy.nginx](https://galaxy.ansible.com/geerlingguy/nginx/)

## Example Playbook

The following playbook will install ProsperBot and the ProsperBot frontend on a single host. It will configure ProsperBot to authenticate with your Prosper credentials.

ProsperBot and ProsperBot Frontend depend on Redis. The Redis server can be on a separate host, but for simplicity, in this example, we'll be installing the [DavidWittman.redis](https://github.com/DavidWittman/ansible-redis) Ansible role on the same host.

#### `secrets.yml`

```yaml
prosperbot_prosper_client_id: [insert your Prosper client ID]
prosperbot_prosper_client_secret: [insert your Prosper client secret]
prosperbot_prosper_username: [insert your Prosper username]
prosperbot_prosper_password: [insert your Prosper password]
```

`prosperbot_prosper_client_id` and `prosperbot_prosper_client_secret` are the values Prosper assigns to you when you generate OAuth credentials on the [OAuth Settings](https://www.prosper.com/oauth#/settings) page.

`prosperbot_prosper_username` and `prosperbot_prosper_password` are the credentials you use to log in to Prosper via the web site.

#### `example.yml`

```yaml
- hosts: all
  become: true
  become_user: root
  become_method: sudo
  vars_files:
    - secrets.yml
  vars:
    - prosperbot_enable_buying: false
  roles:
    - { role: DavidWittman.redis }
    - { role: mtlynch.prosperbot }
    - { role: mtlynch.prosperbot-frontend }
```

### Running Example Playbook

```shell
ansible-galaxy install \
  DavidWittman.redis mtlynch.prosperbot mtlynch.prosperbot-frontend
ansible-playbook example.yml
```

## License

Apache2

## Author Information

This role was created in 2016 by [Michael Lynch](http://mtlynch.io).
