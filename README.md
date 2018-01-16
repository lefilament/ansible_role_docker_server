# Ansible role to deploy docker packages on Debian/Ubuntu server and proxy docker

[![](https://img.shields.io/badge/licence-AGPL--3-blue.svg)](http://www.gnu.org/licenses/agpl "License: AGPL-3")

This role allows you to install on a Debian/Ubuntu server the packages needed to run docker services and docker-compose.
It also deploys and start a dockerized proxy service based on [Traefik](https://github.com/containous/traefik) and a docker protecting the docker socket from [Tecnativa](https://github.com/Tecnativa/docker-socket-proxy)

No specific configuration is needed (apart from configuration for Ansible to reach your server and elevate privileges).



# Credits

## Contributors

* Remi Cazenave <remi-filament>


## Maintainer

[![](https://le-filament.com/img/logo-lefilament.png)](https://le-filament.com "Le Filament")

This role is maintained by Le Filament
