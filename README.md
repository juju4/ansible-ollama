[![Actions Status - Master](https://github.com/juju4/ansible-ollama/workflows/AnsibleCI/badge.svg)](https://github.com/juju4/ansible-ollama/actions?query=branch%3Amain)
[![Actions Status - Devel](https://github.com/juju4/ansible-ollama/workflows/AnsibleCI/badge.svg?branch=devel)](https://github.com/juju4/ansible-ollama/actions?query=branch%3Adevel)

# Ollama ansible role

Ansible role to setup [Ollama](https://github.com/ollama/ollama/).

See also
* https://ollama.com/

## Requirements & Dependencies

### Ansible
It was tested on the following versions:
 * 2.10-17

### Operating systems

Tested on Ubuntu 24.04, 22.04, 20.04, Centos/Rockylinux 9.

## Example Playbook

Just include this role in your list.
For example

```
- host: myhost
  roles:
    - juju4.ollama
```

you probably want to review variables

## Variables

TBD


## Continuous integration

```
$ pip install molecule docker
$ molecule test
$ MOLECULE_DISTRO=ubuntu:24.04 molecule test --destroy=never
```


## Troubleshooting & Known issues

* If using GPU and systemd hardening, you will require custom options for access (see defaults/main.yml for amdgpu example).

## License

BSD 2-clause
