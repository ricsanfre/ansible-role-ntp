Ansible Role: NTP
=========

Install and configure NTP (chrony) on Linux.

Requirements
------------

None.

Role Variables
--------------

Available variables are listed below along with default values (see `defaults\main.yaml`)



    ntp_timezone: Europe/Madrid

Set timezone for the server


    ntp_daemon: chrony
    ntp_package: chrony

NTP package name and daemon. Default pakage for Ubuntu: chrony. ntp is deprecated

    ntp_config_file: /etc/chrony/chrony.conf

Path to NTP service config file

   ntp_servers:

NTP servers or pool to be used.

Format is a list of dictionaries with the following keys:
- server:  host or pool
- type: (Optional) Defaults to server. Maps to a time source in the configuration file. Can be one of server, peer, pool.
- options: (Optional) List of options that depends on type, see Chrony [documentation](https://chrony.tuxfamily.org/doc/4.0/chrony.conf.html) for details.

```
ntp_servers:
  - server: ntp.ubuntu.org
    type: pool
    options:
      - option: iburst
      - option: maxsources
        val: 4
  - server: 0.ubuntu.pool.ntp.org
    type: pool
    options:
      - option: iburst
      - option: maxsources
        val: 1
  - server: 1.ubuntu.pool.ntp.org
    type: pool
    options:
      - option: iburst
      - option: maxsources
        val: 1
  - server: 2.ubuntu.pool.ntp.org
    type: pool
    options:
      - option: iburst
      - option: maxsources
        val: 2
```

   ntp_allow_hosts:[]

Optionally specify a host, subnet, or network from which to allow NTP # connections to a machine acting as NTP server

```
ntp_allow_hosts:
  - 10.0.0.0/24
```

Dependencies
------------

None

Example Playbook
----------------

For NTP server

```
- hosts: ntp-server
  roles:
    - role: ricsanfre.ntp
      ntp_servers:
        - server: ntp.ubuntu.org
          type: pool
          options:
            - option: iburst
            - option: maxsources
          val: 4
        - server: 0.ubuntu.pool.ntp.org
          type: pool
          options:
            - option: iburst
            - option: maxsources
          val: 1
        - server: 1.ubuntu.pool.ntp.org
          type: pool
          options:
            - option: iburst
            - option: maxsources
          val: 1
        - server: 2.ubuntu.pool.ntp.org
          type: pool
          options:
            - option: iburst
            - option: maxsources
          val: 2
      ntp_allow_hosts:
        - 10.0.0.0/24

```



For NTP Client
```
- hosts: ntp-client
  roles:
    - role: ricsanfre.ntp
      ntp_servers:
        - server: 10.0.0.1
          type: server
      ntp_allow_hosts: []

```



Include `vars/main.yaml`

  ntp_time_zone: Europe/Madrid



License
-------

MIT/BSD

Author Information
------------------

Ricardo Sanchez (ricsanfre)
