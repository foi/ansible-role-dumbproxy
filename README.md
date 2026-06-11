# ansible-role-dumbproxy

ansible role for dumbproxy - https://github.com/SenseUnit/dumbproxy.

<img src="https://github.com/foi/ansible-role-dumbproxy/actions/workflows/ci.yml/badge.svg?branch=main">

Compatibility
--------------

python: 3.12, 3.13, 3.14
ansible: 12, 13

Install
--------------

`ansible-galaxy role install foi.dumbproxy`

or add it in `requirements.yml`

```
roles:
  - name: foi.dumbproxy
```

and run `ansible-galaxy install -r requirements.yml`

Role Variables
--------------
role defaults:
```yml
dumbproxy_version: 1.49.0
dumbproxy_download_url: https://github.com/SenseUnit/dumbproxy/releases/download/v{{ dumbproxy_version }}/dumbproxy.linux-amd64
dumbproxy_user: dumbproxy
dumbproxy_append_to_groups: []
dumbproxy_etc_path: /etc/dumbproxy
dumbproxy_etc_path_mode: '0750'
dumbproxy_etc_path_files_mode: '0640'
dumbproxy_etc_files: {}
dumbproxy_binary_path: /usr/local/bin
dumbproxy_args:
  - -bind-address 127.0.0.1:1080
  - -deny-dst-addr 127.0.0.0/8
dumbproxy_default_unit_options:
  Wants: network-online.target
  After: network-online.target
dumbproxy_default_service_options:
  User: "{{ dumbproxy_user }}"
  Restart: always
  TimeoutStopSec: 5s
  LimitNOFILE: 1048576
  ProtectSystem: full
  ProtectHome: tmpfs
  PrivateTmp: true
  PrivateDevices: true
  ProtectKernelTunables: true
  ProtectKernelModules: true
  ProtectKernelLogs: true
  ProtectControlGroups: true
  MemoryDenyWriteExecute: true
  LockPersonality: true
  RestrictRealtime: true
  ProtectClock: true
  RestartSec: 10
  StartLimitBurst: 30
  StartLimitIntervalSec: 30
  AmbientCapabilities: CAP_NET_BIND_SERVICE
  NoNewPrivileges: yes
  ExecStart: "{{ dumbproxy_binary_path }}/dumbproxy {{ dumbproxy_args | join(' ') }}"
```

Example Playbook
----------------
```yml
# inventory
[dumbproxy]
server
# host_vars/server.yml
dumbproxy_args:
  - -bind-address :443
  - -deny-dst-addr 127.0.0.0/8
  - -autocert
  - -auth 'static://?username=admin&password=123456&hidden_domain=testdomain.com'
# playbook.yml
- hosts: dumbproxy
  roles:
    - role: foi.dumbproxy
  become: true
```
License
-------

MIT
