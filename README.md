Redis
========

An Ansible role to install Redis in standalone, cluster or sentinel mode.

## Role Variables

Port and interface on which Redis will listen.
```yaml
redis_port: 6379
redis_bind_interface: 0.0.0.0
```

Redis log level. Can be debug, verbose, notice and warning.
```yaml
redis_log_level: notice
```

Redis Cluster configs.
```yaml
cluster_enabled: false
cluster_port: 16379
```

Redis Sentinel configs.
```yaml
sentinel_enabled: false
sentinel_port: 26379
sentinel_quorum: 2
sentinel_down_after_ms: 5000
sentinel_failover_timeout_ms: 60000
sentinel_parallel_syncs: 1
```
If redis sentinel is enabled, you have to provide an additional config which doesn't have any default values:
```yaml
sentinel_master_ip: "<ip_of_master_node>"
```

Any additional Redis config can go here.
```yaml
redis_extra_config: ""
```

Any additional Redis sentinel config can go here.
```yaml
sentinel_extra_config: ""
```

## Usage
Clone repo and put your playbook file adjacent to `roles/redis` directory. Import the role in one of your tasks and start using it.
To run playbook: `ansible-playbook -i /path/to/inventory.txt /path/to/playbook.yaml`

Cluster installation
----------------

```yaml
- name: Install Redis
  hosts: redis1
  become: yes
  tasks:
    - name: Run install Redis role
      import_role:
        name: redis
      vars:
        cluster_enabled: true
        install_redis_exporter: true
        redis_extra_config: |
          protected-mode no
```

Sentinel installation
----------------

```yaml
- name: Install Redis
  hosts: redis1
  become: yes
  tasks:
    - name: Run install Redis role
      import_role:
        name: redis
      vars:
        cluster_enabled: false
        sentinel_enabled: true
        sentinel_master_ip: "127.0.0.1"
        install_redis_exporter: true
        redis_extra_config: |
          protected-mode no
```

Standalone installation
----------------

```yaml
- name: Install Redis
  hosts: redis1
  become: yes
  tasks:
    - name: Run install Redis role
      import_role:
        name: redis
      vars:
        cluster_enabled: false
        sentinel_enabled: false
        install_redis_exporter: true
        redis_extra_config: |
          protected-mode no
```
