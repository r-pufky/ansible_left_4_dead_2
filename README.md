# Left 4 Dead 2
Left 4 Dead 2 dedicated server.

## [Requirements][i]
Requires [r_pufky.game][g] galaxy-ng collection.

  Resource | Minimum | Recommended
 ----------|---------|-------------
  CPU      | 2c/2t   | 4c/8t @2.8Ghz
  RAM      | 2GB     | 4GB
  Disk     | 7.5GB   | 10GB (without additional mods)

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

* [ports][k] - Ports are **not** managed (defined for external use).

## Usage

### Warning
> [L4D2 requires valid Steam login with entitlements as of 2024-11-26.][l]
> Errors during download with RC=5 (Rate Limit Exceeded) REQUIRES a delay of
> 12-24 hours before attempting again, per Steam. This is non-negotiable.
> See [Rate Limit Exceeded](#rate-limit-exceeded).
>
> A Steam Guard login request should appear during role application if enabled.
> * Relaunch Steam app if nothing appears.
> * Server will be updated after approval. This may take a few minutes.

### Feature Flags
Tasks are gated by feature flags and executed in the following order.

  Step | Flag                        | Notes
 ------|-----------------------------|-------
  1    | left_4_dead_2_flg_update    | Update server on launch or if already installed.
  2    | left_4_dead_2_flg_config    | Set configuration files.
  3    | left_4_dead_2_flg_metamod   | Install Metamod.
  3    | left_4_dead_2_flg_sourcemod | Install Sourcemod.

### Example Playbooks

``` yaml
- name:  'Left 4 Dead 2 Server with metamod, sourcemod and admin users.'
  hosts: 'l4d2.example.com'
  become: true
  roles:
     - 'r_pufky.game.left_4_dead_2'
  vars:
    left_4_dead_2_srv_steam_user: 'SteamUser'
    left_4_dead_2_srv_steam_pass: '{{ vault_steam_password }}'
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

## Troubleshooting

### Rate Limit Exceeded
Too many failed login attempts to Steam. **12-24** hour required wait before
trying again.

Steam service will timeout invalid logins for users after a given amount. You
must wait before trying again. Additional attempts before timeout expires will
only extend the timeout and potentially **disable** your account.

Ensure:
* **!unsafe** is used with `left_4_dead_2_srv_steam_pass`.
* `left_4_dead_2_srv_steam_user` is set.

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

Testing variables:

  Variable            | Type | Description
 ---------------------|------|-------------
  molecule_flg_inject | bool | Disable **get_url** to inject files locally.

### [Releases][b]

  Release | Debian | Ansible | Notes
 ---------|--------|---------|-------
  2.x.x   | 13     | 2.20    | Ansible 2.20, feature flags, and semantic versioning.
  1.x.x   | 12     | 2.11    | Migration from private repository.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]


[a]: https://r-pufky.github.io/ansible_docs
[b]: https://semver.org/spec/v2.0.0
[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22

[f]: https://github.com/r-pufky/ansible_left_4_dead_2/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_game
[i]: https://github.com/r-pufky/ansible_left_4_dead_2/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_left_4_dead_2/tree/main/defaults/main/main.yml
[k]: https://github.com/r-pufky/ansible_left_4_dead_2/blob/main/defaults/main/ports.yml
[l]: https://github.com/ValveSoftware/steam-for-linux/issues/11522