Ansible Role: fabos.docker_swarm.setup
=========

The role provides task files for installing and uninstalling docker swarm on target hosts as well as scaling the number of swarm workers.

Requirements
------------

The role is only tested for specific Linux Distributions/Versions (see meta/main.yml).

Role Variables
--------------

No parameter required.

Dependencies
------------

- https://github.com/FabOS-AI/fabos-slm-dc-docker
  

Example Playbook
----------------

See playbook files in root folder:

- install.yml
- uninstall.yml
- scaleup.yml
- scaledown.yml

License
-------

MIT

Author Information
------------------

Benjamin Goetz - Fraunhofer IPA
