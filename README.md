# Ansible Role: Wiregard-Rest

This module deploys a [wireguard-rest API](https://github.com/yanosz/wireguard-rest)  written in ruby. It is used within the KBU freifunk-network. It:

- Creates a user `wireguard-rest` running the application, having sudo access for applying wireguard configurations
- Installs wireguard-dkms and wireguard-tools as of Debian Sid
- Installs system Ruby + Rails, sqlite3, git
- Configures a new network interface `wg-rest` using configuration generated from the database
- Clones and updates https://github.com/yanosz/wireguard-rest (master branch) to / in `/usr/local/lib/wireguard-rest`
- It contains configuration for https://github.com/yanosz/wireguard-rest --- according to the network interface
- Starts Puma (Ruby server) using systemd socket-activation on port 8172/tcp (localhost)

## Dependencies

* Ansible
* ipcalc
* A webserve is supposed to forward requests to 8172/tcp (localhost). This is not defined by this role

## Distributions

* Debian 9 (Stretch) *only*

## Variables
`wireguard_rest` is used as a common name-space. The names are self-explaining.
Defaults are applied as follows. `apps` is used for variables applying to the rest-application, *only*.

*Please consider re-generating the ULA-address-range when deploying.*

```
wireguard_rest:         
  server_ipv4: '172.27.128.2'
  netmask_ipv4: '255.255.248.0'
  server_ipv6: 'fd78:e474:9f6f:df64::1'
  netmask_ipv6: '64'
  port: 1195
  client_ipv4_start: '172.27.128.2'
  client_ipv6_start: 'fd78:e474:9f6f:df64::1'
  app:
    config_file: '/etc/wireguard/rest.conf'
    secret_key_file: '/etc/wireguard/privatekey_rest'
    public_key_file: '/etc/wireguard/publickey_rest'
    wg_reload_cmd: 'sudo /usr/bin/wg setconf wg-rest /etc/wireguard/rest.conf'
```

## Example Playbook

```
- hosts: servers
  roles:
    - wireguard-rest
```

## License

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
