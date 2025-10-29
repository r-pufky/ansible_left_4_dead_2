# Left 4 Dead 2
Left 4 Dead 2 dedicated server.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_left_4_dead_2/blob/main/meta/main.yml)

Resources | Minimum | Recommended
----------|---------|----------------------------------------------
CPU       | 2c/2t   | 4c/8t@2.8Ghz
RAM       | 2GB     | 4GB
Disk      | 9GB     | 10GB (metamod, sourcemod; no additional mods)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_left_4_dead_2/tree/main/defaults/main)

### Ports
All ports and protocols have been defined for the role.

[defaults/ports.yml](https://github.com/r-pufky/ansible_left_4_dead_2/blob/main/defaults/main/ports.yml)

## Dependencies
**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.game](https://github.com/r-pufky/ansible_collection_game) collection.

## Example Playbook
WARNING
> L4D2 install requires valid Steam login with entitlements as of 2024-11-26.
> Steam Guard users MUST approve login on phone during role execution. Login
> is not required after dedicated server files downloaded.

Read defaults documentation.

The following example will get an instance quickly up and running. Server will
be created using the steamcmd user from `r_pufky.game.steam` and install both
metamod and sourcmod; granting admin privileges.
``` yaml
- name: 'Left 4 Dead 2 server'
  hosts: 'l4d2.example.com'
  become: true
  roles:
     - 'r_pufky.game.left_4_dead_2'
  vars:
    left_4_dead_2_cfg_host: 'My host message.'
    left_4_dead_2_cfg_motd: 'Have fun!'
    left_4_dead_2_cfg_server:
      hostname: 'L4D2 Server'
      sv_search_key: 'L4D2'
      sv_allow_lobby_connect_only: 0
      sv_steamgroup: 0
      sv_steamgroup_exclusive: 0
      sv_gametypes: 'coop,survival,versus,teamversus,realism,teamscavenge'
      mp_gamemode: 'coop,survival,versus,teamversus,realism,teamscavenge'
      rcon_password: ''
      sv_rcon_banpenalty: 360
      sv_rcon_log: 1
      sv_rcon_maxfailures: 3
      sv_rcon_minfailuretime: 600
      motd_enabled: 1
      mp_disable_autokick: 1
      sv_allow_wait_command: 0
      sv_alltalk: 0
      sv_alternateticks: 0
      sv_cheats: 0
      sv_consistency: 1
      sv_contact: ''
      sv_downloadurl: ''
      sv_pausable: 0
      sv_lan: 0
      sv_region: 1
      sv_minrate: 40000
      sv_maxrate: 50000
      sv_mincmdrate: 0
      sv_maxcmdrate: 67
      sv_log_onefile: 0
      sv_logbans: 1
      sv_logecho: 1
      sv_logfile: 1
      sv_logflush: 0
      sv_logsdir: 'logs'
    left_4_dead_2_cfg_admins:
      - user: 76561198021925108
        priv: '99:z'  # Root privileges.
```

Changes updating the configuration only can be done to speed role application:
``` bash
ansible-playbook site.yml --tags l4d \
  -e '{"left_4_dead_2_srv_update_server": false}'
```

## Troubleshooting

### Rate Limit Exceeded
Too many failed login attempts to Steam. **12-24** hour required wait before
trying again.

Steam service will timeout invalid logins for users after a given amount. You
must wait before trying again. Additional attempts before timeout expires will
only extend the timeout and potentially disable your account.

Ensure:
* **!unsafe** is used with `left_4_dead_2_srv_steam_pass`.
* `left_4_dead_2_srv_steam_user` is set.

## Development
Configure [environment](https://github.com/r-pufky/ansible_collection_docs/blob/main/ansible/environment.md)

Run all unit tests:
``` bash
molecule test --all
```

### Releases
Release format: **{OS}-{SERVICE}-{ROLE}**

Each type inherits the versioning system used; defaulting to schematic
versioning.

`12.0.0-2.0.3-1.0.0`

* 12.0.0 - Debian 12 (bookworm).
* 2.0.3 - Service/app version.
* 1.0.0 - Role version.

Releases are branched on Debian releases:

* **[13.x.x](https://github.com/r-pufky/ansible_left_4_dead_2)**: 13 Trixie.
* **[12.x.x](https://github.com/r-pufky/ansible_left_4_dead_2/tree/12.x)**: 12 Bookworm.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_left_4_dead_2/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
