Redis
=========

An Ansible role to install Redis

Role Variables
--------------

Port and interface on which Redis will listen.
```yaml
redis_port: 6379
redis_bind_interface: 127.0.0.1
```

Redis log level. Can be debug, verbose, notice and warning.
```yaml
redis_log_level: notice
```

Redis Cluster configs.
```yaml
redis_cluster_enabled: "no"
redis_cluster_port: 16379
```

Any additional config can go here.
```yaml
redis_extra_config: ""
```

Example Playbook
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
        redis_cluster_enabled: yes
        install_redis_exporter: true
        redis_extra_config: |
          protected-mode no
```
