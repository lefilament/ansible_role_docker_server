docker_server
=========
The main repo for this role is on [Le Filament GitLab](https://sources.le-filament.com/lefilament/ansible-roles/docker_server.git)
This roles deploys Docker and configures daemon, together with Traefik inverseproxy

Requirements
------------

This role requires Ansible collection community.docker
Also, this role requires passlib (either python package or python3-passlib apt package) for proper encryption of passwords

Role Variables
--------------

Variables defined in defaults/main.yaml:
* default_maintenance_email : default maintenance e-mail used to validate Let's Encrypt certificate (defaults to maintenance@example.org)
* docker_userns_remap : whether remapping of user namespace is being used for Docker (security feature defaults to true)
* restrict_internet_access : whether dockers should be granted access to Internet of if networks are internal only (defaults to true meaning docker containers have no direct internet access, whitelisted URLs should be used to grant specific access)
* default_sshd_port: Port on which SSH daemon listens (defaults to 10022)
* host_user : user used to connect to the server
* srv_proxy_pass: Password to access proxy protected pages (AUTH defaults to SuperSecureProxyP4$$)
* allow_iframe: whether iframes are allowed

This role makes use of a few variables which are set in case the target server is part of other groups (but still initialized to false in defaults/main.yml), namely :
* docker_auth
  * ldap_url
  * sso_url
* docker_nextcloud or docker_owncloud
  * cloud_url
  * cloud_collabora and cloud_collabora_url
  * cloud_onlyoffice and cloud_onlyoffice_url
* docker_odoo
  * extra_app
  * metabase
* docker_registry_auth : configuration for connecting to docker registry (goes in /root/.docker/config.json)

Note : all variables defined in defualts_main.yml might be useful in another role, in that case, it would be better to have them overwritten at play or host_vars level in order to make sure the same value is provided to each independant role

Variables from vars directory:
* OS specific (RedHat.yml / Debian.yml) :
  * packages_to_remove : list of packages that we want to remove from default delivered servers
  * packages_to_install : list of packages to install on server
* Global (main.yml):
  * pip_packages: Python pip packages to be installed / upgraded
  * timezone: for Traefik logs (defaults to "Europe/Paris")
  * traefik_version: "v2.7"

This role also makes use of variables gathered from facts :
* ansible_os_family : Family of Operating System (Debian or RedHat)
* ansible_distribution: name of the distribution (Ubuntu, CentOS, etc.)
* ansible_distribution_release; name of the distribution version (Trusty, Xenial, etc.)

This role also configures backup servers where daily docker facts should be pushed :
* backup_sftp_user : user to be configured on backup server used to push facts
* These backup servers should be in group backup_server (if none then corresponding tasks are not pushing anywhere)

Eventually, this role configures 2 variables in host_vars (only if docker_userns_remap is true):
* dockremap_subuid : first subuid used for user namespace remap for Docker
* dockremap_subgid : first subgid used for user namespace remap for Docker

Dependencies
------------

This role requires the following Ansible collection :
* community.docker

This role does not have dependencies per-se, while it can be dependant on variables defined in other groups (backup_servers, docker_auth, docker_nextcloud, docker_odoo, docker_owncloud)

Example Playbook
----------------


    - hosts: docker
      gather_facts: true
      become: true
      roles:
      - { role: docker_server, tags: docker }
      vars:
      - { default_maintenance_email: "maintenance@example.org" }
      - { default_sshd_port: 10022 }
      - { docker_userns_remap: true }
      - { restrict_internet_access: true }
      - { host_user: "testuser" }
      - { srv_proxy_pass: "SuperSecureProxyP4$$" }
      - { cloud_collabora: true }
      - { cloud_collabora_url: "collabora.example.org" }
      - { cloud_url: "cloud.example.org" }
      - { metabase: false }
      - { ldap_url: "ldap.example.org" }
      - { sso_url: "sso.example.org" }

License
-------

AGPL-3

Author Information
------------------

Le Filament (https://le-filament.com)
