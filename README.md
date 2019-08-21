Docker Swarm
=========

Super simple Ansible role to initialize a Docker Swarm and add managers + workers.

Requirements
------------

None.

Role Variables
--------------

```yaml
---
# The Ansible group Swarm managers belong to
docker_swarm_managers_group: docker_swarm_managers

# The single manager the Swarm should be initialized on
docker_swarm_manager_init: "{{ groups[docker_swarm_managers_group][0] }}"

# The Ansible group Swarm workers belong to
docker_swarm_workers_group: docker_swarm_workers

# The network to which the swarm advertise address (--advertise-addr) belongs to
docker_swarm_advertise_address_cidr: 10.0.0.0/8

# The advertise address swarm managers & workers should use
docker_swarm_advertise_address: "{{ hostvars[inventory_hostname]['ansible_all_ipv4_addresses'] | ipaddr(docker_swarm_advertise_address_cidr) | first }}"

# The remote address swarm workers should use
docker_swarm_remote_address: "{{ hostvars[docker_swarm_manager_init]['ansible_all_ipv4_addresses'] | ipaddr(docker_swarm_advertise_address_cidr) | first }}"
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: docker
  roles:
    - leonkura.docker-swarm
```

With Docker installation:
```yaml
- hosts: docker
  roles:
    - leonkura.docker
    - leonkura.docker-swarm
```

With custom values for the managers & workers group and a non private network for the advertise address:
```yaml
- hosts: docker
  roles:
    - role: leonkura.docker-swarm
      docker_swarm_managers_group: my_custom_swarm_managers_group
      docker_swarm_workers_group: my_custom_swarm_workers_group
      docker_swarm_advertise_address_cidr: 198.51.100.0/24
```

License
-------

GPL-3.0