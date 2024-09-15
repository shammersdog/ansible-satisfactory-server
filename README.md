Satisfactory Dedicated Server Ansible Role
==========================================

An unofficial role designed to configure and deploy a Satisfactory dedicated server on Linux systems in a "manual" way.

Requires some libraries provided by apt, yum, or dnf.

Not tested on systems without these package managers, but may work fine. SteamCMD requires certain libraries. YMMV.

**Important note about the `firewall` variable:** Only specify this if you have an **existing and pre-configured firewall** on your system. Changing this when there isn't an existing firewall may make your system inaccessible. At the very least, ensure TCP port 22 is open for SSH access before attempting to use this.

Requirements
------------

* Linux with SystemD
* x86/64 systems (ARM, etc. are not supported)
* 8-16 GB of RAM (Depends on number of players)

Install from GitHub
-------------------

To install this role, use Ansible-Galaxy:  
`ansible-galaxy install git+https://github.com/shammersdog/ansible-satisfactory-server.git`


Role Variables
--------------

| Variable           | Purpose                                                               | Default                                                                  |
|--------------------|-----------------------------------------------------------------------|--------------------------------------------------------------------------|
| `steam_user`       | Unprivileged user for running SteamCMD and the server.                | `steam`                                                                  |
| `steam_group`      | Group for `steam_user`.                                               | `steam`                                                                  |
| `steamcmd_dir`     | Where SteamCMD is installed to.                                       | `/home/steam/Steam`                                                      |
| `steamcmd_tarball` | Custom location for SteamCMD tarball.                                 | `https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz` |
| `install_dir`      | Location of Satisfactory Dedicated Server after installation.         | `/home/steam/SatisfactoryDedicatedServer`                                |
| `bind_address`     | Bind to a particular interface based on IP address (supports v6).     | `*` (any/every)                                                          |
| `port`             | TCP/UDP port for server to listen on.                                 | `7777`                                                                   |
| `players_number`   | Number of players allowed on the dedicated server at once.            | `4`                                                                      |
| `firewall`         | Configure an **existing firewall service** (firewalld, ufw, iptables) | `none`                                                                   |
| `start_firewall`   | Start and enable any firewalls specified with the `firewall` var.     | `true` (only executes if `firewall` is a valid service name, not `none`) |


Example Playbooks
-----------------

This installs and starts the Satisfactory server with sane defaults and no firewall configuration.

    - hosts: myserver
      roles:
        - { role: shammersdog.satisfactory-server }


Installing and starting a Satisfactory server with a custom port 4649, user satisfactory, and player count of 16.

    - hosts: secret-hidden-work-server.example.com
      roles:
        - { role: shammersdog.satisfactory-server, steam_user: satisfactory, steam_group: satisfactory, port: '4649', players_number: '16' }


An 8-player Satisfactory server configured for FirewallD for all systems in an example group of `all-satisfactory`. (**Read note at top of document before doing this.**)

    - hosts: all-satisfactory
      roles:
        - { role: shammersdog.satisfactory-server, players_number: '8', firewall: firewalld}


Default Satisfactory server with IPv6-only binding.

    - hosts: myip6server
      roles:
        - { role: shammersdog.satisfactory-server, bind_address: '::'}


License
-------

MIT

Author Information
------------------

Shammersdog - https://shamme.rs - sham@huskypa.ws
